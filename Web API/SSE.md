## SSE

[MDN](https://developer.mozilla.org/zh-CN/docs/Server-sent_events/EventSource)
SSE（Server-Sent Event，服务端推送事件）是一种允许服务端向客户端推送新数据的 HTML5 技术。
**基于 HTTP 协议，只要服务端数据有更新，不需要客服端请求主动像客户端推送。**

实际上，HTTP 协议是无法做到服务器主动推送信息的，但是可以通过服务器像客服端声明，接下来要发送的是流信息（streaming）。也就是说发送的不是一次性的数据包而是一个数据流。

特点：

1. 单向通信。（server to client）
2. 基于 http 协议
3. 默认支持断线重连
4. 以 text/event-stream 格式发送事件，文本流形式。
5. 一般只用来传送文本，二进制数据需要编码后传送，WebSocket 默认支持传送二进制数据

```js
if ("EventSource" in window) {
  // url可以与当前网址同域，也可以跨越。
  // 跨域时，可以指定第二个参数，打开withCredentials, 表示是否携带cookie
  let source = new EventSource(url, { withCredentials: true });
}
```

EventSource 实例的 readyState 属性，表明连接的当前连接状态。该属性只读，可以取以下值。

0：相当于常量 EventSource.CONNECTING，表示连接还未建立，或者断线正在重连。
1：相当于常量 EventSource.OPEN，表示连接已经建立，可以接受数据。
2：相当于常量 EventSource.CLOSED，表示连接已断，且不会重连。

```js
// 连接建立，就会触发open事件，可以在open属性定义回调函数
source.addEventListener(
  "open",
  function (event) {
    // ...
  },
  false
);
// 客户端收到服务器发来的数据，就会触发message事件，
// 可以在onmessage属性的回调函数;

source.addEventListener(
  "message",
  function (event) {
    var data = event.data;
    // handle message
  },
  false
);

// 如果发生通信错误（比如连接中断），就会触发error事件，
// 可以在onerror属性定义回调函数

source.addEventListener(
  "error",
  function (event) {
    // handle error event
  },
  false
);

// 关闭连接
source.close();
```

重大事件新闻、股票等推送！
