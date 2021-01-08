# **可以用for循环直接删除ArrayList的特定元素吗？可能会出现什么问题？怎样解决**

```java
for循环直接删除ArrayList中的特定元素是错的，不同的for循环会发生不同的错误，泛型for会抛出 ConcurrentModificationException，普通的for想要删除集合中重复且连续的元素，只能删除第一个。
错误原因：打开JDK的ArrayList源码，看下ArrayList中的remove方法（注意ArrayList中的remove有两个同名方法，只是入参不同，这里看的是入参为Object的remove方法）是怎么实现的，一般情况下程序的执行路径会走到else路径下最终调用faseRemove方法，会执行System.arraycopy方法，导致删除元素时涉及到数组元素的移动。针对普通for循环的错误写法，在遍历第一个字符串b时因为符合删除条件，所以将该元素从数组中删除，并且将后一个元素移动（也就是第二个字符串b）至当前位置，导致下一次循环遍历时后一个字符串b并没有遍历到，所以无法删除。针对这种情况可以倒序删除的方式来避免。
public boolean remove(Object o) {
    if (o == null) {
        for (int index = 0; index < size; index++)
            if (elementData[index] == null) {
                fastRemove(index);
                return true;
            }
    } else {
        for (int index = 0; index < size; index++)
            if (o.equals(elementData[index])) {
                fastRemove(index);
                return true;
            }
    }
    return false;
}

private void fastRemove(int index) {
    modCount++;
    int numMoved = size - index - 1;
    if (numMoved > 0)
        System.arraycopy(elementData, index+1, elementData, index, numMoved);
    elementData[--size] = null; // clear to let GC do its work
}
            

解决方案：用 Iterator:
public static void main(String[] args) {

        List<String> list = new ArrayList<>(Arrays.asList("a", "b" , "c", "d"));
        Iterator<String> iterator = list.iterator();
        while (iterator.hasNext()) {
            if(iterator.next().equals("b")) {
                iterator.remove();
            }
        }
        System.out.println(list);
    }
```

 输出结果：a, c, d