HTML5新提供的一个api， 作用是记录当前标签页在浏览器中的激活状态。所谓“激活状态”指当前标签是否正在被用户浏览。

属性： document.visibilityState 事件: document.addEventListener('visibilitychange', () => ())

visibilitychange有以下四个值 hidden: 浏览器最小化、切换tab等 visible: 可见（部分支持） unloaded: 卸载时（部分支持） prerender：当文档被加载到屏幕画面以外或者不可见时返回 prerender，这个是非必要属性，浏览器可选择性的支持。


```js

function visible() {
 let visibleState ='';
 let visibleChange ='';
 if (typeof document.visibilityState !='undefined') {
   visibleChange ='visibilitychange';
   visibleState ='visibilityState';
 } else if (typeof document.webkitVisibilityState !='undefined') {
   visibleChange ='webkitvisibilitychange';
   visibleState ='webkitVisibilityState';
 } else if (typeof  document.msVisibilityState !== 'undefined') {
   visibleChange ='msvisibilitychange';
   visibleState ='msVisibilityState';
 } else if (typeof  document.mozVisibilityState !== 'undefined') {
   visibleChange ='mozvisibilitychange';
   visibleState ='mozVisibilityState';
 } else if (typeof  document.oVisibilityState !== 'undefined') {
   visibleChange ='ovisibilitychange';
   visibleState ='oVisibilityState';
 }

 if (visibleChange) {
     document.addEventListener(visibleChange, function() {
         if (document[visibleState] =='visible') {
             alert('页面可见');
             // 操作
         }
     })
 }
}
```