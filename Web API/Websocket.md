[参考资料](https://www.runoob.com/html/html5-websocket.html)

## 定义

基于TCP协议的双工通信协议。


## 特点

- 只需完成一次握手，便可在Server、client端实现持久性连接
- 双工通信，数据双向传输，实时通讯，服务端可主动push
- 更好的节省宽带和资源

## 常用API

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