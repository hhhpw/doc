

### OAuth 2.0 是目前最流行的授权机制，用来授权第三方应用，获取用户数据。


**简单说，OAuth 就是一种授权机制。数据的所有者告诉系统，同意授权第三方应用进入系统，获取这些数据。系统从而产生一个短期的进入令牌（token），用来代替密码，供第三方应用使用。**


在生活中，比较常见的 OAuth2 的使用场景是授权登录，并且也广泛应用于 web、桌面应用和移动 APP 的第三方服务提供授权登录验证机制，以实现不同应用直接数据访问的权限。


### 定义四种角色

- 资源拥有者（Resource Owner)，代表授权客户端访问本身资源信息的用户
- 客户端（Client），代表访问受限资源的第三方应用。
- 资源服务器（Resource Server），代表托管了受保护的用户账号信息的服务器，它与认证服务器，可以是同一台服务器，也可以是不同的服务器
- 授权服务器（Authorization Server），代表验证用户身份然后为客户端派发资源访问令牌的服务器，即服务提供商专门用来处理认证的服务器

### 工作流程
![oauth](../../images/oauth.jpg)

大致流程概括就是：

（A）Authrization Request
用户打开客户端以后，客户端要求用户给予授权。

（B）Authorization Grant（Get）
用户同意给予客户端授权。

（C）Authorization Grant（Post）
客户端向授权服务器发送它自己的客户端身份标识和上一步中获得的授权（authorization grant），向认证服务器申请令牌。

（D）Access Token（Get）
认证服务器对客户端进行认证以后，确认无误，同意发放令牌（access token），授权阶段至此全部结束。

（E）Access Token（Post && Validate）
客户端使用令牌，向资源服务器申请获取资源。

（F）Protected Resource（Get）

### 授权方式

OAuth 2.0 规定了四种获得令牌的流程。你可以选择最适合自己的那一种，向第三方应用颁发令牌。即以下四种授权方式：

- 授权码（authorization-code）
- 隐藏式（implicit）
- 密码式（password）
- 客户端凭证（client credentials）

### OAuth2 优缺点
- 优点：
适合快速开发实施，代码量少，API需要被不同APP使用，且每个APP使用方式也不同的情况。

- 缺点：
学习和理解的成本比较大，并且 OAuth2 不是一个严格的标准协议，在实施过程中更容易出错。


[参考资料](https://blog.csdn.net/qq_24313635/article/details/106900838)
[参考资料](http://www.ruanyifeng.com/blog/2019/04/oauth-grant-types.html)


### 实际案例

微信登录