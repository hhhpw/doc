

```js
const EventUtil = {
    addHandler: function(el, type, handler) {
        if (el.addEventListener) {
        el.addEventListener(type, handler, false)   
    } else if (el.attachEvent){
        el.attachEvent('on' + type, handler);
    };
    removeHandler: function(el, type, handler) {
        if (el.removeEventListener) {
            el.removeEventListener(type, handler, false)
        }else if (el.detachEvent) {
            el.detachEvent('on', type, handler)
        }
    }
}
```
事件对象兼容： const event = e || window.event;
注意： currentTarget始终等于event的this对象,而target则包含事件的实际目标。
bubbles 事件是否冒泡
cancelable 是否可以取消事件的默认行为
currentTarget 其事件处理程序当前正在处理事件的那个元素
defaultPrevented 是否已经调用了preventDefault()
eventPhase 事件处理程序的阶段 1表示捕获阶段 2表示处于目标 3表示冒泡阶段
preventDefault() 取消事件的默认行为，如果cancelable是true，则可以调用这个方法
stopImmdiatePropagation() 取消事件的冒泡，如果bubbles为true，则可以使用
stopPropagation() 取消事件的冒泡，如果bubbles为true，则可以使用

IE
cancelBubble 默认为false，ture则取消事件冒泡
returnValue 默认为true，false则取消事件的默认行为
srcElement 事件的目标和target属性相同
type 被触发的事件类型

- currentTarget和target的区别


currentTarget是事件的绑定目标
target是事件最终指向的目标
```html
<body>
  <div id="a">
    a
    <div id="b">
      B
      <div id="c">
        C
        <div id="d">D</div>
      </div>
    </div>
  </div>
</body>

</html>
```
```js
<script>
  document.getElementById('a').addEventListener('click', function (e) {
    console.log('target:' + e.target.id + '&currentTarget:' + e.currentTarget.id);
  });
  document.getElementById('b').addEventListener('click', function (e) {
    console.log('target:' + e.target.id + '&currentTarget:' + e.currentTarget.id);
  });
  document.getElementById('c').addEventListener('click', function (e) {
    console.log('target:' + e.target.id + '&currentTarget:' + e.currentTarget.id);
  });
  document.getElementById('d').addEventListener('click', function (e) {
    console.log('target:' + e.target.id + '&currentTarget:' + e.currentTarget.id);
  });
</script>
```
点击最里层的D，
以上，无论是冒泡还是在捕获阶段，target始终是D, 而currentTarget则不同。
在事件处理程序中，this始终指向currentTarget。