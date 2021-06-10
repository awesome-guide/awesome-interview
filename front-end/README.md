## 前端技术栈分享内容参考列表

1. 写一个深拷贝，考虑 正则，Date 这种类型的数据
2. Vue 自定义指令懒加载
3. 判断 DOM 标签的合法性，标签的闭合，span 里面不能有 div，写一个匹配 DOM 标签的正则
4. 替换日期格式，xxxx-yy-zz 替换成 xxx-zz-yy，可以使用 正则的捕获组来实现
5. 实现 Promise.all, Promise.allSettled
6. 获取一段 DOM 节点中标签个数最多的标签
7. 写一个简单的 diff
8. 手写节流
9. 手写 ES6 的继承

第 1 题：写 React / Vue 项目时为什么要在列表组件中写 key，其作用是什么？

解析：[1 题](./shareAnswer/React&Vue项目时为什么要在列表组件中写key.md)

第 2 题：['1', '2', '3'].map(parseInt) what & why ?

解析：[2 题](<./shareAnswer/数组.map(parseInt)-what&why.md>)

第 3 题：什么是防抖和节流？有什么区别？如何实现？

解析：[3 题](./shareAnswer/什么是防抖和节流？有什么区别？如何实现.md)

第 4 题：介绍下 Set、Map、WeakSet 和 WeakMap 的区别？

解析：[4 题](./shareAnswer/介绍下Set、Map、WeakSet和WeakMap的区别.md)

第 5 题：介绍下深度优先遍历和广度优先遍历，如何实现？

第 6 题：请分别用深度优先思想和广度优先思想实现一个拷贝函数？

第 7 题：ES5/ES6 的继承除了写法以外还有什么区别？

第 8 题：setTimeout、Promise、Async/Await 的区别

第 9 题：异步笔试题

第 10 题：Async/Await 如何通过同步的方式实现异步

第 11 题：算法手写题

第 12 题：JS 异步解决方案的发展历程以及优缺点。

第 13 题：Promise 构造函数是同步执行还是异步执行，那么 then 方法呢？

第 14 题：情人节福利题，如何实现一个 new

第 15 题：简单讲解一下 http2 的多路复用

第 16 题：谈谈你对 TCP 三次握手和四次挥手的理解

第 17 题：A、B 机器正常连接后，B 机器突然重启，问 A 此时处于 TCP 什么状态

第 18 题：React 中 setState 什么时候是同步的，什么时候是异步的？

第 19 题：React setState 笔试题，下面的代码输出什么？

第 20 题：介绍下 npm 模块安装机制，为什么输入 npm install 就可以自动安装对应的模块？

第 21 题：有以下 3 个判断数组的方法，请分别介绍它们之间的区别和优劣

第 22 题：介绍下重绘和回流（Repaint & Reflow），以及如何进行优化

第 23 题：介绍下观察者模式和订阅-发布模式的区别，各自适用于什么场景

第 24 题：聊聊 Redux 和 Vuex 的设计思想

第 25 题：说说浏览器和 Node 事件循环的区别

第 26 题：介绍模块化发展历程

第 27 题：全局作用域中，用 const 和 let 声明的变量不在 window 上，那到底在哪里？如何去获取？。

第 28 题：cookie 和 token 都存放在 header 中，为什么不会劫持 token？

第 29 题：聊聊 Vue 的双向数据绑定，Model 如何改变 View，View 又是如何改变 Model 的

第 30 题：两个数组合并成一个数组

第 31 题：改造下面的代码，使之输出 0 - 9，写出你能想到的所有解法。

第 32 题：Virtual DOM 真的比操作原生 DOM 快吗？谈谈你的想法。

第 33 题：下面的代码打印什么内容，为什么？

第 34 题：简单改造下面的代码，使之分别打印 10 和 20。

第 35 题：浏览器缓存读取规则

第 36 题：使用迭代的方式实现 flatten 函数。

第 37 题：为什么 Vuex 的 mutation 和 Redux 的 reducer 中不能做异步操作？

第 38 题：下面代码中 a 在什么情况下会打印 1？

第 39 题：介绍下 BFC 及其应用。

第 40 题：在 Vue 中，子组件为何不可以修改父组件传递的 Prop

第 41 题：下面代码输出什么

第 42 题：实现一个 sleep 函数

第 43 题：使用 sort() 对数组 [3, 15, 8, 29, 102, 22] 进行排序，输出结果

第 44 题：介绍 HTTPS 握手过程

第 45 题：HTTPS 握手过程中，客户端如何验证证书的合法性

第 46 题：输出以下代码执行的结果并解释为什么

第 47 题：双向绑定和 vuex 是否冲突

第 48 题：call 和 apply 的区别是什么，哪个性能更好一些

第 49 题：为什么通常在发送数据埋点请求的时候使用的是 1x1 像素的透明 gif 图片？

第 50 题：实现 (5).add(3).minus(2) 功能。

第 51 题：Vue 的响应式原理中 Object.defineProperty 有什么缺陷？

解析：[51 题](https://github.com/hanyueqiang/frontEnd_book/issues/7)

第 52 题：怎么让一个 div 水平垂直居中

解析：[52 题](https://github.com/hanyueqiang/frontEnd_book/issues/8)

第 53 题：输出以下代码的执行结果并解释为什么

解析：[53 题](https://github.com/hanyueqiang/frontEnd_book/issues/9)

第 54 题：冒泡排序如何实现，时间复杂度是多少， 还可以如何改进？

解析：[54 题](https://github.com/hanyueqiang/frontEnd_book/issues/10)

第 55 题：某公司 1 到 12 月份的销售额存在一个对象里面

解析：[55 题](https://github.com/hanyueqiang/frontEnd_book/issues/11)

第 56 题：要求设计 LazyMan 类，实现以下功能。

解析：[56 题](https://github.com/hanyueqiang/frontEnd_book/issues/12)

第 57 题：分析比较 opacity: 0、visibility: hidden、display: none 优劣和适用场景。

解析：[57 题](https://github.com/hanyueqiang/frontEnd_book/issues/13)

第 58 题：箭头函数与普通函数（function）的区别是什么？构造函数（function）可以使用 new 生成实例，那么箭头函数可以吗？为什么？

解析：[58 题](https://github.com/hanyueqiang/frontEnd_book/issues/14)

第 59 题：给定两个数组，写一个方法来计算它们的交集。

解析：[59 题](https://github.com/hanyueqiang/frontEnd_book/issues/15)

第 60 题：已知如下代码，如何修改才能让图片宽度为 300px ？注意下面代码不可修改。

解析：[60 题](https://github.com/hanyueqiang/frontEnd_book/issues/16)

第 61 题：介绍下如何实现 token 加密

解析：[61 题](https://github.com/hanyueqiang/frontEnd_book/issues/17)

第 62 题：redux 为什么要把 reducer 设计成纯函数

解析：[62 题](https://github.com/hanyueqiang/frontEnd_book/issues/18)

第 63 题：如何设计实现无缝轮播

解析：[63 题](https://github.com/hanyueqiang/frontEnd_book/issues/24)

第 64 题：模拟实现一个 Promise.finally

解析：[64 题](https://github.com/hanyueqiang/frontEnd_book/issues/23)

第 65 题： a.b.c.d 和 a['b']['c']['d']，哪个性能更高？

解析：[65 题](https://github.com/hanyueqiang/frontEnd_book/issues/19)

第 66 题：ES6 代码转成 ES5 代码的实现思路是什么

解析：[66 题](https://github.com/hanyueqiang/frontEnd_book/issues/20)

第 67 题：数组编程题

解析：[67 题](https://github.com/hanyueqiang/frontEnd_book/issues/21)

第 68 题： 如何解决移动端 Retina 屏 1px 像素问题

解析：[68 题](https://github.com/hanyueqiang/frontEnd_book/issues/22)

第 69 题： 如何把一个字符串的大小写取反（大写变小写小写变大写），例如 ’AbC' 变成 'aBc' 。

解析：[69 题](https://github.com/hanyueqiang/frontEnd_book/issues/25)

第 70 题： 介绍下 webpack 热更新原理，是如何做到在不刷新浏览器的前提下更新页面的

第 71 题： 实现一个字符串匹配算法，从长度为 n 的字符串 S 中，查找是否存在字符串 T，T 的长度是 m，若存在返回所在位置。

第 72 题： 为什么普通 for 循环的性能远远高于 forEach 的性能，请解释其中的原因。

第 73 题： 介绍下 BFC、IFC、GFC 和 FFC

第 74 题： 使用 JavaScript Proxy 实现简单的数据绑定

第 75 题：数组里面有 10 万个数据，取第一个元素和第 10 万个元素的时间相差多少

第 76 题：输出以下代码运行结果

第 77 题：算法题「旋转数组」

第 78 题：Vue 的父组件和子组件生命周期钩子执行顺序是什么

第 79 题：input 搜索如何防抖，如何处理中文输入

第 80 题：介绍下 Promise.all 使用、原理实现及错误处理

第 81 题：打印出 1 - 10000 之间的所有对称数

第 82 题：周一算法题之「移动零」

第 83 题：var、let 和 const 区别的实现原理是什么

第 84 题：请实现一个 add 函数，满足以下功能。

第 85 题：react-router 里的 `<Link>` 标签和 `<a>` 标签有什么区别

第 86 题：周一算法题之「两数之和」

第 87 题：在输入框中如何判断输入的是一个正确的网址。

第 88 题：实现 convert 方法，把原始 list 转换成树形结构，要求尽可能降低时间复杂度

第 89 题：设计并实现 Promise.race()

第 90 题：实现模糊搜索结果的关键词高亮显示

第 91 题：介绍下 HTTPS 中间人攻击

第 92 题：已知数据格式，实现一个函数 fn 找出链条中所有的父级 id

第 93 题：给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。请找出这两个有序数组的中位数。要求算法的时间复杂度为 O(log(m+n))。

第 94 题：vue 在 v-for 时给每项元素绑定事件需要用事件代理吗？为什么？

第 95 题：模拟实现一个深拷贝，并考虑对象相互引用以及 Symbol 拷贝的情况

第 96 题：介绍下前端加密的常见场景和方法

第 97 题：React 和 Vue 的 diff 时间复杂度从 O(n^3) 优化到 O(n) ，那么 O(n^3) 和 O(n) 是如何计算出来的？

第 98 题：写出如下代码的打印结果

第 99 题：编程算法题

第 100 题：请写出如下代码的打印结果
