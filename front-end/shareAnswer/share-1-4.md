## 1-4 题

# 1、写 React / Vue 项目时为什么要在列表组件中写 key，其作用是什么？

总结两点：
1、更快找到对应的节点：
给每一个 vnode 加 key，会形成一个 key => index 的 map 映射，如果不加 key 寻找对应节点只能通过遍历的方式
但值得注意的是：加 key 的效率不一定有不加 key 效率快，因为不加 key 是进行就地复用的，而加 key 会对节点进行删除和创建，所以所谓的快只是在寻找 node 中体现
2、更准确：
给每一个 node 加唯一标识 key，能够更加准确的判断,因为带 key 就不是就地复用了，在 sameNode 函数 a.key === b.key 对比中可以避免就地复用的情况。所以会更加准确。
看对应例子

# 2、['1', '2', '3'].map(parseInt) what & why ?

答案：[1,NaN,NaN]
解析：分步解析
map 接收的第一个参数为一个 callBack
callBack 接收两个参数：第一个是正在处理的元素，第二个参数是正在处理元素的索引值

可把上述操作看为：
前提：parseInt 第二参数只接收 2-36 的进制数
parseInt('1',0) 第二参数为 0 时 若 string 不是以'o'或者'ox'开头，则会默认为 10 进制，所以结果为 1
parseInt('2',1) 第二参数为 1,直接报 NaN
parseInt('3',2) 进制数为 2，但表示的数中只能含 0 或者 1，表示的数大于 2，所以无法解析，结果为 NaN

# 3、什么是防抖和节流？有什么区别？如何实现？

防抖：触发高频事件后 n 秒内函数只会执行一次，如果 n 秒内高频事件再次被触发，则重新计算时间
function debounce(fn,time) {
&emsp;let timeout = null; // 存放延时器
&emsp;return function () {
&emsp;clearTimeout(timeout); // 每次触发这个函数，都会清除延时器
&emsp;timeout = setTimeout(() => { // 然后又创建一个新的 setTimeout, 这样就能保证输入字符后的 interval 间隔内如果还有字符输入的话，就不会执行 fn 函数
&emsp;fn.apply(this, arguments);
&emsp;}, time);
&emsp;};
&emsp;}
&emsp;function sayHi() {
&emsp;console.log('防抖成功');
&emsp;}

var inp = document.getElementById('inp');
inp.addEventListener('input', debounce(sayHi,500)); // 防抖

节流：高频事件触发，但在 n 秒内只会执行一次，所以节流会稀释函数的执行频率

function throttle(fn,time) {
let canRun = true; // 通过闭包保存一个标记
return function () {
if (!canRun) return; // 在函数开头判断标记是否为 true，不为 true 则 return
canRun = false; // 立即设置为 false
setTimeout(() => { // 将外部传入的函数的执行放在 setTimeout 中
fn.apply(this, arguments);
// 最后在 setTimeout 执行完毕后再把标记设置为 true(关键)表示可以执行下一次循环了。当定时器没有执行的时候标记永远是 false，在开头被 return 掉
canRun = true;
}, time);
};
}
function sayHi(e) {
console.log(e.target.innerWidth, e.target.innerHeight);
}
window.addEventListener('resize', throttle(sayHi,500));

# 4、介绍下 Set、Map、WeakSet 和 WeakMap 的区别？

1、set：用来保存不同的值，类似于数组
2、weakSet:类似于 set,但是是用来保存对象的，且是弱引用，弱其他对象不引用里面的对象，则会自动清除

3、map: 用来保存键值对的，传统上的对象只能用字符串来当做键，而 map 可用对象当做键
4、weakMap: WeakMap 只接受对象作为键名（null 除外），不接受其他类型的值作为键名。WeakMap 的键名所指向的对象，不计入垃圾回收机制。其余和 map 差不多
概念性的看阮老师文章：https://es6.ruanyifeng.com/#docs/set-map
