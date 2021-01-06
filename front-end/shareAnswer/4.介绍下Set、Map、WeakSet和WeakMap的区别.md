

# 介绍下 Set、Map、WeakSet 和 WeakMap 的区别？

1、Set：用来保存不同的值，类似于数组

2、weakSet:类似于 set,但是是用来保存对象的，且是弱引用，弱其他对象不引用里面的对象，则会自动清除

3、map: 用来保存键值对的，传统上的对象只能用字符串来当做键，而 map 可用对象当做键

4、weakMap: WeakMap 只接受对象作为键名（null 除外），不接受其他类型的值作为键名。WeakMap 的键名所指向的对象，不计入垃圾回收机制。其余和 map 差不多

概念性的看阮老师文章：https://es6.ruanyifeng.com/#docs/set-map