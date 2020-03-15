[参考文档](https://juejin.im/post/5bf3ad55f265da61682afc9b)

DNS 解析:将域名解析成 IP 地址
TCP 连接：TCP 三次握手
发送 HTTP 请求
服务器处理请求并返回 HTTP 报文
浏览器解析渲染页面
断开连接：TCP 四次挥手


### URL(Uniform Resource Locator)

URL（Uniform Resource Locator），统一资源定位符，用于定位互联网上资源，俗称网址。
比如 http://www.w3school.com.cn/html/index.asp，
遵守以下的语法规则：
scheme://host.domain:port/path/filename
各部分解释如下：
scheme - 定义因特网服务的类型。常见的协议有 http、https、ftp、file，其中最常见的类型是 http，而 https 则是进行加密的网络传输。
host - 定义域主机（http 的默认主机是 www.[www可有可无原因](https://www.jb51.net/yunying/483942.html)）
domain - 定义因特网域名，比如 w3school.com.cn
port - 定义主机上的端口号（http 的默认端口号是 80）
path - 定义服务器上的路径（如果省略，则文档必须位于网站的根目录中）。
filename - 定义文档/资源的名称


### DNS 域名解析

 - IP （IP Address）127.0.0.1为本机IP，不利于人类的记忆习惯。但对于计算机来说，更擅长处理一长串数字。为此，DNS服务器产生。

- 解析流程

      浏览器缓存：浏览器会按照一定的频率缓存 DNS 记录。
      操作系统缓存：如果浏览器缓存中找不到需要的 DNS 记录，那就去操作系统中找。
      路由缓存：路由器也有 DNS 缓存。
      ISP 的 DNS 服务器：ISP 是互联网服务提供商(Internet Service Provider)的简称，ISP 有专门的 DNS 服务器应对 DNS 查询请求。
      根服务器：ISP 的 DNS 服务器还找不到的话，它就会向根服务器发出请求，进行递归查询（DNS 服务器先问根域名服务器.com 域名服务器的 IP 地址，
      然后再问.baidu 域名服务器，依次类推）

      
      
##### 迭代查询

 - 本地域名服务器向根域名服务器查询，根域名服务器告诉它下一步到哪里去查询，然后它再去查，每次它都是以客户机的身份去各个服务器查询。

      
##### 递归查询

- 本机向本地域名服务器发出一次查询请求，就静待最终的结果。如果本地域名服务器无法解析，自己会以DNS客户机的身份向其它域名服务器查询，直到得到最终的IP地址告诉本机 


### TCP三次握手


**在客户端发送数据之前会发起TCP三次握手用以同步客户端和服务端的序列号和确认号，并交换 TCP 窗口大小信息。**

1. 握手过程

  第一次： client => server  SYN=1,Seq=X。发送数据到服务端
  第二次： server => client  SYN=1,ACK=X+1,Seq=Y。传达确认信息。
  第三次： client => server  ACK=Y+1,Seq=Z。回传信息，代表握手结束。

2. 为啥需要三次握手

**谢希仁著《计算机网络》中讲“三次握手”的目的是“为了防止已失效的连接请求报文段突然又传送到了服务端，因而产生错误”。**

### 发送HTTP请求

请求行： GET /index.html HTTP1.1，包括请求方法，请求地址等
请求头： Request Headers，请求头部通知服务器有关于客户端请求的信息，比如Host,Connection: keep-alive持久连接等,User-agent等。
请求体： 请求携带的参数，name=tom&password=1234&realName=tomson，携带了name.password.realName参数



### 服务器处理请求并返回HTTP报文

常见的web server有apache、nginx等。

- 响应报文

  1. 响应行: 包含协议版本、状态码等
  2. 响应头部: 响应报文的附加信息，key/value对组成
  3. 响应主体: 返回响应数据，并不是所有响应报文都有响应数据



### 浏览器解析渲染页面

**查看面试必会-页面解析渲染**


### 四次挥手

ACK、Seq、Fin数据包

- 发起方向被动方发送报文，Fin、Ack、Seq，表示已经没有数据传输了。并进入 FIN_WAIT_1 状态。(第一次挥手：由浏览器发起的，发送给服务器，我请求报文发送完了，你准备关闭吧)
- 被动方发送报文，Ack、Seq，表示同意关闭请求。此时主机发起方进入 FIN_WAIT_2 状态。(第二次挥手：由服务器发起的，告诉浏览器，我请求报文接受完了，我准备关闭了，你也准备吧)
- 被动方向发起方发送报文段，Fin、Ack、Seq，请求关闭连接。并进入 LAST_ACK 状态。(第三次挥手：由服务器发起，告诉浏览器，我响应报文发送完了，你准备关闭吧)
- 发起方向被动方发送报文段，Ack、Seq。然后进入等待 TIME_WAIT 状态。被动方收到发起方的报文段以后关闭连接。发起方等待一定时间未收到回复，则正常关闭。(第四次挥手：由浏览器发起，告诉服务器，我响应报文接受完了，我准备关闭了，你也准备吧)

