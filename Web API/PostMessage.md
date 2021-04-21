[参考地址](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage)


**window.postMessage() 方法可以安全地实现跨源通信**
```js

otherWindow.postMessage(message, targetOrigin, [transfer]);
```
```js
// a.js
      let otherWindow = window.frames[0];
      // 接受两个参数 
      // data：需要传递的数据，
      // targetOrigin： 通过窗口的origin属性来指定哪些窗口能接收到消息事件，其值可以是字符串"*"（表示无限制）或者一个URI。
      // 在发送消息的时候，如果目标窗口的协议、主机地址或端口这三者的任意一项不匹配targetOrigin提供的值，
      // 那么消息就不会被发送；只有三者完全匹配，消息才会被发送。这个机制用来控制消息可以发送到哪些窗口；
      // 例如，当用postMessage传送密码时，这个参数就显得尤为重要，必须保证它的值与这条包含密码的信息的预期接受者的origin属性完全一致，
      // 来防止密码被恶意的第三方截获。如果你明确的知道消息应该发送到哪个窗口，那么请始终提供一个有确切值的targetOrigin，而不是*。
      // 不提供确切的目标将导致数据泄露到任何对数据感兴趣的恶意站点。
      otherWindow.postMessage("message from index page", "*");

// b.js

window.addEventListener("message", receiveMessage, false);

function receiveMessage(event)
{
  // event.origin 来源域名
  // event.soruce 来源window对象引用
  // event.data window对象传递过来的数据
  // For Chrome, the origin property is in the event.originalEvent
  // object.
  // 这里不准确，chrome没有这个属性
  // var origin = event.origin || event.originalEvent.origin;
  var origin = event.origin
  if (origin !== "http://example.org:8080")
    return;

  // ...
}

```


### 安全问题

如果要使用，始终要验证origin和source属性，验证发件人的信息