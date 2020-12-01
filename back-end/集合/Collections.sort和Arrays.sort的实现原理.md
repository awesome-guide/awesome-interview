Collections.sort和Arrays.sort的实现原理

==结论：不带自定义排序规则时都是用的是ComparableTimSort.sort方法，带有自定义排序规则的都使用的是
TimSort.sort方法==

Arrays.sort参数类型是数组类型的基本数据类型和引用数据类型，可以自定义引用类型的排序规则，自定义的规则需实现Comparator比较器。

Collections.sort参数是List类型的，主要有常用的ArrayList，Arrays.asList，LinkedList。自定义的排序规则跟Arrays.sort的一致。有些list的实现类是有自己的sort的方法的，有些没有的就使用List接口提供的实现方法，比如LinkedList。

Arrays.sort和Collections.sort不带自定义参数的排序底层是使用的ComparableTimSort.sort方法。

Arrays.sort的代码


```
public static void sort(Object[] a) {
        if (LegacyMergeSort.userRequested)
            legacyMergeSort(a);
        else
            ComparableTimSort.sort(a, 0, a.length, null, 0, 0);
    }
```
Collections.sort是通过参数本身实现的sort方法进行的排序

Collections.sort代码

```
public static <T extends Comparable<? super T>> void sort(List<T> list) {
        list.sort(null);
    }
```
ArrayList实现的sort方法

 
```
public void sort(Comparator<? super E> c) {
        final int expectedModCount = modCount;
        Arrays.sort((E[]) elementData, 0, size, c);
        if (modCount != expectedModCount) {
            throw new ConcurrentModificationException();
        }
        modCount++;
    }
    
public static <T> void sort(T[] a, int fromIndex, int toIndex,
                                Comparator<? super T> c) {
        if (c == null) {
            sort(a, fromIndex, toIndex);
        } else {
            rangeCheck(a.length, fromIndex, toIndex);
            if (LegacyMergeSort.userRequested)
                legacyMergeSort(a, fromIndex, toIndex, c);
            else
                TimSort.sort(a, fromIndex, toIndex, c, null, 0, 0);
        }
        
public static void sort(Object[] a, int fromIndex, int toIndex) {
        rangeCheck(a.length, fromIndex, toIndex);
        if (LegacyMergeSort.userRequested)
            legacyMergeSort(a, fromIndex, toIndex);
        else
            ComparableTimSort.sort(a, fromIndex, toIndex, null, 0, 0);
    }
```
Arrays.sort和Collections.sort不带自定义参数的排序底层是使用的TimSort.sort方法。

Arrays.sort代码


```
public static <T> void sort(T[] a, Comparator<? super T> c) {
        if (c == null) {
            sort(a);
        } else {
            if (LegacyMergeSort.userRequested)
                legacyMergeSort(a, c);
            else
                TimSort.sort(a, 0, a.length, c, null, 0, 0);
        }
    }
```
Collections.sort代码跟不带自定义排序规则的基本相同

```
public void sort(Comparator<? super E> c) {
        final int expectedModCount = modCount;
        Arrays.sort((E[]) elementData, 0, size, c);
        if (modCount != expectedModCount) {
            throw new ConcurrentModificationException();
        }
        modCount++;
    }
    
public static <T> void sort(T[] a, int fromIndex, int toIndex,
                                Comparator<? super T> c) {
        if (c == null) {
            sort(a, fromIndex, toIndex);
        } else {
            rangeCheck(a.length, fromIndex, toIndex);
            if (LegacyMergeSort.userRequested)
                legacyMergeSort(a, fromIndex, toIndex, c);
            else
                TimSort.sort(a, fromIndex, toIndex, c, null, 0, 0);
        }
```


