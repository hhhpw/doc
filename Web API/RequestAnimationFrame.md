## 基础知识

## setinterval、setTimeout的问题
 1. 首先，当相应的浏览器窗口最小化，JavaScript 计时器在背景标签仍然持续运行。因此，浏览器继续运行看不见的动画，导致不必要的 CPU 和电池寿命的消耗。在移动设备尤其严重。

 2. settimeout任务被放入异步队列，只有当主线程任务执行完后才会执行队列中的任务，因此实际执行时间总是比设定时间要晚

 3. settimeout的固定时间间隔不一定与屏幕刷新时间相同，会引起丢帧


## 优势
  由系统决定回调函数的执行时机。60Hz的刷新频率，那么每次刷新的间隔中会执行一次回调函数，不会引起丢帧，不会卡顿

## 使用
```css
  #status {
    background: red;
    height: 50px;
    width: 10px;
  }
```
```js
  // 简易进度条
  function updateProgess() {
    let div = document.getElementById("status");
    console.log("div.style.width", div.style.width);
    div.style.width = (parseInt(div.style.width || 0) + 5) + "px"
    if (div.style.width !== "500px") {
      window.requestAnimationFrame(updateProgess)
    }
  }

  // requestAnimationFrame只运行一次传入的函数
  // 所以不仅在第一次使用时候需要调用它
  // UI发生变化时也需要调用它
  window.requestAnimationFrame(updateProgess);
```

```js

var requestAnimationFrame = window.requestAnimationFrame ||
 window.mozRequestAnimationFrame ||
 window.webkitRequestAnimationFrame ||
 window.msRequestAnimationFrame
```
在requestAnimationFrame回调函数里会有一个方法，时间。