## 自定义事件


```js

void initCustomEvent(
 in DOMString type, 事件类型
 in boolean bubbles, 表明该事件是否会冒泡.
 in boolean cancelable, 表明该事件是否可以被取消.
 in any detail 事件初始化时传递的数据.
);


const ev = document.querySelector('.div')
const event = new CustomEvent('eventName', {
 detail: {
   message: 'Hello World',
   time: new Date(),
 },
 bubbles : true,
 cancelable: true,
});
ev.addEventListener('eventName', function (e) {
 console.log(e);
});
setTimeout(function () {
 ev.dispatchEvent(event);//给节点分派一个合成事件
}, 1000);


```