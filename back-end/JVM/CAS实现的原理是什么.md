# CAS实现原理是什么

CAS：比较并替换。

CAS操作包含三个操作数——**内存位置**、**预期原值**及**新值**。执行CAS操作的时候，将内存位置的值与预期原值比较，如果相匹配，那么处理器会自动将该位置值更新为新值，否则，处理器不做任何操作。

CAS是一条CPU的**原子指令**（**cmpxchg**指令），不会造成所谓的数据不一致问题，Unsafe提供的CAS方法（如compareAndSwapXXX）底层实现即为CPU指令cmpxchg。

##### 处理器如何实现原子操作

首先处理器会自动保证基本的内存操作的原子性。同时，处理器提供**总线锁定**和**缓存锁定**两个机制来保证复杂内存操作的原子性。

- **总线锁定**

  当一个处理器要操作共享变量时，在 BUS 总线上发出一个 Lock 信号，其他处理就无法操作这个共享变量了。

  缺点很明显，总线锁定在阻塞其它处理器获取该共享变量的操作请求时，也可能会导致大量阻塞，从而增加系统的性能开销。
  
  在x86架构中，提供了指令前缀LOCK。LOCK保证了指令不会受其他处理器或cpu核的影响。
  
  在PentiumPro之前，LOCK的实现，是通过锁住bus（总线），从而阻止其他cpu核的内存访问。这种实现是非常低效的。
  
  从PentiumPro开始，LOCK只会阻塞其他cpu核对相关内存的缓存块的访问。
  
- **缓存锁定**

  后来的处理器都提供了缓存锁定机制，也就说当某个处理器对缓存中的共享变量进行了操作，其他处理器会有个嗅探机制，将其他处理器的该共享变量的缓存失效，待其他线程读取时会重新从主内存中读取最新的数据，基于 MESI 缓存一致性协议来实现的。

现代的处理器基本都支持和使用的缓存锁定机制。



##### CAS的源码:

CAS底层使用`JNI`调用C代码实现的，`Hotspot`源码在`Unsafe.cpp`里可以找到它的实现：

```c
static JNINativeMethod methods_15[] = {
    //省略一堆代码...
    {CC"compareAndSwapInt",  CC"("OBJ"J""I""I"")Z",      FN_PTR(Unsafe_CompareAndSwapInt)},
    {CC"compareAndSwapLong", CC"("OBJ"J""J""J"")Z",      FN_PTR(Unsafe_CompareAndSwapLong)},
    //省略一堆代码...
};
```

我们可以看到compareAndSwapInt实现是在`Unsafe_CompareAndSwapInt`里面，再深入到`Unsafe_CompareAndSwapInt`:

```c
UNSAFE_ENTRY(jboolean, Unsafe_CompareAndSwapInt(JNIEnv *env, jobject unsafe, jobject obj, jlong offset, jint e, jint x))
  UnsafeWrapper("Unsafe_CompareAndSwapInt");
  oop p = JNIHandles::resolve(obj);
  jint* addr = (jint *) index_oop_from_field_offset_long(p, offset);
  return (jint)(Atomic::cmpxchg(x, addr, e)) == e;
UNSAFE_END
```

p是取出的对象，addr是p中offset处的地址，最后调用了`Atomic::cmpxchg(x, addr, e)`, 其中参数x是即将更新的值，参数e是原内存的值。代码中能看到cmpxchg有基于各个平台的实现，Linux X86平台下的源码分析：

```c
// Adding a lock prefix to an instruction on MP machine
#define LOCK_IF_MP(mp) "cmp $0, " #mp "; je 1f; lock; 1: "

inline jint Atomic::cmpxchg (jint exchange_value, volatile jint* dest, jint compare_value) {
  int mp = os::is_MP();
  __asm__ volatile (LOCK_IF_MP(%4) "cmpxchgl %1,(%3)"
                    : "=a" (exchange_value)
                    : "r" (exchange_value), "a" (compare_value), "r" (dest), "r" (mp)
                    : "cc", "memory");
  return exchange_value;
}
```

这是一段小汇编，`__asm__`说明是ASM汇编，`__volatile__`禁止编译器优化

```c
// Adding a lock prefix to an instruction on MP machine
#define LOCK_IF_MP(mp) "cmp $0, " #mp "; je 1f; lock; 1: "
```

`os::is_MP`判断当前系统是否为多核系统，如果是就给总线加锁，所以同一芯片上的其他处理器就暂时不能通过总线访问内存，保证了该指令在多处理器环境下的原子性。



程序会根据当前处理器的类型来决定是否为cmpxchg指令添加lock前缀。如果程序是在多处理器上运行，就为cmpxchg指令加上lock前缀（lock cmpxchg）。反之，如果程序是在单处理器上运行，就省略lock前缀（单处理器自身会维护单处理器内的顺序一致性，不需要lock前缀提供的内存屏障效果）。

##### lock前缀

lock前缀的指令的说明：

- 保证指令的执行的原子性
  带有lock前缀的指令在执行期间会锁住总线，使得其他处理器暂时无法通过总线访问内存。很显然，这会带来昂贵的开销。从Pentium 4，Intel Xeon及P6处理器开始，intel在原有总线锁的基础上做了一个很有意义的优化：如果要访问的内存区域（area of memory）在lock前缀指令执行期间已经在处理器内部的缓存中被锁定（即包含该内存区域的缓存行当前处于独占或以修改状态），并且该内存区域被完全包含在单个缓存行（cache line）中，那么处理器将直接执行该指令。由于在指令执行期间该缓存行会一直被锁定，其它处理器无法读/写该指令要访问的内存区域，因此能保证指令执行的原子性。

- 禁止该指令与之前和之后的读和写指令重排序
  在AI-32架构软件开发者手册第8中内存排序中，有说明LOCK前缀会禁止指令与之前和之后的读和写指令重排序。这相当于JMM中定义的StoreLoad内存屏障的效果。也正是因为这个内存屏障的效果，会使得线程把其写缓冲区中的所有数据刷新到内存中。注意，这里不是单单被修改的数据会被回写到主内存，而是写缓存中所有的数据都回写到主内存。
  而将写缓冲区的数据回写到内存时，就会通过缓存一致性协议（如，MESI协议）和窥探技术来保证写入的数据被其他处理器的缓存可见。
  而这就相当于实现了volatile的内存语义。

所以，我们可以知道CAS的指令的原子性，以及内存语义就是通过lock前缀指令来完成的。





链接：https://www.jianshu.com/p/bd68ddf91240

