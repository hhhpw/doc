


![POST](../images/post.jpeg)
### GET 和 POST 请求区别


**对于 GET 方式的请求，浏览器会把 http header 和 data 一并发送出去，服务器响应 200（返回数据）；
而对于 POST，浏览器先发送 header，服务器响应 100 continue，浏览器再发送 data，服务器响应 200 ok（返回数据）**
**GET 产生一个 TCP 数据包；POST 产生两个 TCP 数据包**

post请求默认是不可以去设置缓存，但可以手动去设置如exprise、cache-control