[参考资料](https://www.runoob.com/html/html5-websocket.html)

## 定义

基于 TCP 协议的双工通信协议。
WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。

WebSocket 使得客户端和服务端之间的数据交换变得更加简单，允许服务端主动向客户端推送。且客户端和服务端只需一次握手，两者之间就可以直接创建永久性的连接，并进行双向数据传输。

使用场景便是替换 Ajax 轮询。传统轮询的模式需要不断向服务端发出请求，浪费很多资源。WebSocket 协议更好的节省服务器资源和带宽，更好的实时通讯。

## 特点

- 只需完成一次握手，便可在 Server、client 端实现持久性连接
- 双工通信，数据双向传输，实时通讯，服务端可主动 push
- 更好的节省宽带和资源

## 常用 API

```js

// 创建
let ws = new WebStocket();

// readyState属性
0: 连接尚未建立
1: 已连接，可以通信
2: 连接正在关闭
3: 连接已关闭或不能打开

// 事件
onopen: 连接时触发
onclose: 连接关闭触发
onmessage: 客户端接收服务端数据触发
onerror: 连接发生错误触发

ws.send() // 使用连接发送数据
ws.close() // 关闭连接
```

```js

 HTTP/1.1 101 Switching Protocols //1
Upgrade: websocket. //2
Connection: Upgrade. //3
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=  //4
Sec-WebSocket-Protocol: chat. //5

```

Upgrade：upgrade 是 HTTP1.1 中用于定义转换协议的 header 域。它表示，如果服务器支持的话，客户端希望使用现有的「网络层」已经建立好的这个「连接（此处是 TCP 连接）」，切换到另外一个「应用层」（此处是 WebSocket）协议。

Connection：HTTP1.1 中规定 Upgrade 只能应用在「直接连接」中，所以带有 Upgrade 头的 HTTP1.1 消息必须含有 Connection 头，因为 Connection 头的意义就是，任何接收到此消息的人（往往是代理服务器）都要在转发此消息之前处理掉 Connection 中指定的域（不转发 Upgrade 域）。
如果客户端和服务器之间是通过代理连接的，那么在发送这个握手消息之前首先要发送 CONNECT 消息来建立直接连接。

Sec-WebSocket-＊：第 7 行标识了客户端支持的子协议的列表（关于子协议会在下面介绍），第 8 行标识了客户端支持的 WS 协议的版本列表，第 5 行用来发送给服务器使用（服务器会使用此字段组装成另一个 key 值放在握手返回信息里发送客户端）。

Origin：作安全使用，防止跨站攻击，浏览器一般会使用这个来标识原始域。

如果服务器接受了这个请求，可能会发送如下这样的返回信息，这是一个标准的 HTTP 的 Response 消息。101 表示服务器收到了客户端切换协议的请求，并且同意切换到此协议。RFC2616 规定只有切换到的协议「比 HTTP1.1 更好」的时候才能同意切换。

# 相同

- 与 http 一样都是基于 TCP 的，都是可靠性传输协议
- 都是应用层协议
- 都使用 Request/Response 模型进行连接的建立。

### 不同

WebSocket 是双向通信协议，模拟 Socket 协议，可以双向发送或接受信息。HTTP 是单向的。

### 联系

WebSocket 在建立握手时，数据是通过 HTTP 传输的。但是建立之后，在真正传输时候是不需要 HTTP 协议的。
