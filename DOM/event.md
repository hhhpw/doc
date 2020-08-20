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
注意： **currentTarget 始终等于 event 的 this 对象,而 target 则包含事件的实际目标。**
bubbles 事件是否冒泡
cancelable 是否可以取消事件的默认行为
currentTarget 其事件处理程序当前正在处理事件的那个元素
defaultPrevented 是否已经调用了 preventDefault()
eventPhase 事件处理程序的阶段 1 表示捕获阶段 2 表示处于目标 3 表示冒泡阶段
preventDefault() 取消事件的默认行为，如果 cancelable 是 true，则可以调用这个方法
stopImmdiatePropagation() 取消事件的冒泡，如果 bubbles 为 true，则可以使用
stopPropagation() 取消事件的冒泡，如果 bubbles 为 true，则可以使用

IE
cancelBubble 默认为 false，ture 则取消事件冒泡
returnValue 默认为 true，false 则取消事件的默认行为
srcElement 事件的目标和 target 属性相同
type 被触发的事件类型

- currentTarget 和 target 的区别

currentTarget 是事件的绑定目标，this 都会只向它
target 是事件触发的目标

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

点击最里层的 D，
**以上，无论是冒泡还是在捕获阶段，target 始终是 D, 而 currentTarget 则不同。
在事件处理程序中，this 始终指向 currentTarget。**
