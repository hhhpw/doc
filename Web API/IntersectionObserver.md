##  IntersectionObserver

```js
// IntersectionObserver接口
//(从属于Intersection Observer API)为开发者提供了一种可以异步监听目标元素与其
//祖先或视窗(viewport)交叉状态的手段。祖先元素与视窗(viewport)被称为根(root)。

// 实例
const io = new IntersectionObserver(callback, options);
// 接受两个参数
// 第一个参数为回调函数，第二参数可选，配置项。

io.observe(document.getElement('img')) //开始观察，接受一个DOM点
io.unobserve(element) //停止观察
io.disconnet() 关闭观察器

// options
// 包括root,threshold,rootMargin
root 默认是viewport，可指定具体的元素，但被观察的元素必须是指定元素的子元素
rootMargin 方向与css Margin相同，用于扩大元素或者缩小视窗的大小
threshold 用于制定交叉比例，默认是个数组[0]，如[0,0.5,1]，则观察元素在0%，50%，100%时会触发回调函数

// callback
var io = new IntersectionObserver((entries)=>{
    console.log(entries)
});
io.observe(document.getElementById('#test'));

// entries对象数组IntersectionObserverEntry
boundingClientRect 目标元素的矩形信息
intersectionRatio 相交区域和目标元素的比例值 intersectionRect/boundingClientRect的比例 不可见时小于等于0
intersectionRect 目标元素和视窗（根）相交的矩形信息 可以称为相交区域，即该元素出现在视窗可见的高度宽度等。
isIntersecting 目标元素当前是否可见 Boolean值 可见为true
rootBounds 根元素的矩形信息，没有指定根元素就是当前视窗的矩形信息
target 观察的目标元素
time 返回一个记录从IntersectionObserver的时间到交叉被触发的时间的时间戳

```