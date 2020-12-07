poll和remove都是队列queue的删除第一个元素的方法。

不同点：poll方法当队列为空时，会返回null，而remove方法当队列为空时会抛出一个NoSuchElementException异常

以LinkedList代码为例


```
public E poll() {
        final Node<E> f = first;
        return (f == null) ? null : unlinkFirst(f);
    }
    
public E removeFirst() {
        final Node<E> f = first;
        if (f == null)
            throw new NoSuchElementException();
        return unlinkFirst(f);
    }
```

