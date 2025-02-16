# SEO
[参考资料](https://www.cnblogs.com/smile-xin/p/11637073.html)

### 什么是SEO

搜索引擎优化（Search Engine Optimization）,简称SEO。是按照搜索引擎给出的优化建议，以增强网站核心价值为目标，从网站结构、内容建设方案、用户互动传播等角度进行合理规划，以改善网站在搜索引擎中的表现，吸引更多搜索引擎用户访问网站。SEO与搜索引擎，互相促进，互利互助。 



### 为什么需要SEO
做SEO是为了提高网站的权重，增强搜索引擎友好度，以达到提高排名，增加流量，改善用户体验，促进销售的作用。


### 前端角度如何进行SEO
1. 能用CSS做的尽量不使用背景图片，背景图片尽量压缩
2. 结构、表现、行为样式的分离。不要把CSS、JS放在同一个页面，鼓励采用外链方式
3. 合理的语义化标签，如strong用于文本加粗，会增加搜索引擎的重视。H标签也会无形增加搜索权重，谨慎使用
4. 为图片指定宽度、高度，设置alt属性
5. a标签尽量加上title属性
6. 集中网站权重。由于蜘蛛分配到每个页面的权重是一定的，这些权重也将平均分配到每个a链接上，那么为了集中网站权重，可以使用”rel=nofollow”属性，它告诉蜘蛛无需抓取目标页,可以将权重分给其他的链接。
```js
  nofollow有两种用法：
  1.用于meta元标签：<meta name="robots" content="nofollow" />，告诉爬虫该页面上所有链接都无需追踪。
  2.用于a标签：<a href="login.aspx" rel="nofollow">登录</a>,告诉爬虫该页面无需追踪。 
``` 
7. 压缩，精减代码。去除无用代码，减少页面体积。
8. 浏览器缓存
9. gzip压缩
10. 设置合理的meta标签。
```html
<meta name=”description”content=”Everything you need toknow about meta tags forsearch engine optimization”/>
<meta name=”robots”content=”noindex,nofollow”/>
```
11. 设置网站的title
12. 少用iframe

### SPA SEO遇到的问题

**SEO效果差，因为搜索引擎只认识html里的内容，不认识js渲染生成的内容，搜索引擎不识别，也就不会给一个好排名，会导致单页应用做出来的网页在搜索引擎上的排名差。**
**专门的技术解决：SSR（Server-Side Rendering）服务端渲染**

**简单理解是将组件或页面通过服务器生成html字符串，再发送到浏览器，最后将静态标记"混合"为客户端上完全交互的应用程序**

SSR常用框架
React 的 Next
Vue.js 的 Nuxt
既然说到SSR就，说一下它的优缺点

优点：
1.更快的响应速度
2.容易被爬虫爬到页面数据

缺点：
1.增加服务器压力
2.开发难度增大
3.可能会由于某些因素导致服务器端渲染的结果与浏览器端的结果不一致

