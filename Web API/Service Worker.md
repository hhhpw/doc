[离线缓存](https://blog.csdn.net/weixin_39939012/article/details/102730199?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_title-0&spm=1001.2101.3001.4242)


[参考文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Service_Worker_API)
[参考文档](https://www.jianshu.com/p/cc506d408d69)

[参考文档-使用](https://blog.csdn.net/mevicky/article/details/86605882)
[参考文档-使用](https://segmentfault.com/a/1190000015050724)


[报错解决](https://stackoverflow.com/questions/49566059/service-worker-registration-error-unsupported-mime-type-text-html)


## chrome devtool
chrome://serviceworker-internals/

## 注意
1. 只能在https或localhost下使用

## 注册

```js
  // 如果这个前端服务挂在http://localhost:8080下
  // 那一定要保证http://localhost:8080/sw.js可以访问！！！！！
  // 否则报错
  // 报错信息如下：
  // DOMException: Failed to register a ServiceWorker for scope
  // ('http://localhost:8080/') with script 
  // ('http://localhost:8080/sw1./// js'):
  // The script has an unsupported MIME type ('text/html').
  if ('serviceWorker' in navigator) {
    // 在页面加载后
    window.addEventListener('load', function () {
      // 这里的路径也是个坑
      // 如果定义为/a/sw.js
      // 则指会对http://localhost:8080/a下进行优化
      navigator.serviceWorker.register('./sw.js')
      .then(reg => { //注册成功
          console.log('注册成功', reg)
      }).catch(err => { //注册成功
          console.log('注册失败', err)
      })
    });
  } else {
      console.log('当前浏览器不支持SW')
  }

```
## 生命周期
![service-worker](../images/service-worker.jpeg)

## PWA应用库

workbox
## 插件

workbox-webpack-plugin

## Service Worker是什么
Service Worker 是一个 基于HTML5 API ，也是PWA技术栈中最重要的特性， 它在 Web Worker 的基础上加上了持久离线缓存和网络代理能力，结合Cache API面向提供了JavaScript来操作浏览器缓存的能力，这使得Service Worker和PWA密不可分。

## Service Worker概述：
一个独立的执行线程，单独的作用域范围，单独的运行环境，有自己独立的context上下文。
一旦被 install，就永远存在，除非被手动 unregister。即使Chrome（浏览器）关闭也会在后台运行。利用这个特性可以实现离线消息推送功能。
处于安全性考虑，必须在 HTTPS 环境下才能工作。当然在本地调试时，使用localhost则不受HTTPS限制。
提供拦截浏览器请求的接口，可以控制打开的作用域范围下所有的页面请求。需要注意的是一旦请求被Service Worker接管，意味着任何请求都由你来控制，一定要做好容错机制，保证页面的正常运行。
由于是独立线程，Service Worker不能直接操作页面 DOM。但可以通过事件机制来处理。例如使用postMessage。

## Service Worker生命周期：
注册（register）：这里一般指在浏览器解析到JavaScript有注册Service Worker时的逻辑，即调用navigator.serviceWorker.register()时所处理的事情。
安装中( installing )：这个状态发生在 Service Worker 注册之后，表示开始安装。
安装后( installed/waiting )：Service Worker 已经完成了安装，这时会触发install事件，在这里一般会做一些静态资源的离线缓存。如果还有旧的Service Worker正在运行，会进入waiting状态，如果你关闭当前浏览器，或者调用self.skipWaiting()，方法表示强制当前处在 waiting 状态的 Service Worker 进入 activate 状态。
激活( activating )：表示正在进入activate状态，调用self.clients.claim())会来强制控制未受控制的客户端，例如你的浏览器开了多个含有Service Worker的窗口，会在不切的情况下，替换旧的 Service Worker 脚本不再控制着这些页面，之后会被停止。此时会触发activate事件。
激活后( activated )：在这个状态表示Service Worker激活成功，在activate事件回调中，一般会清除上一个版本的静态资源缓存，或者其他更新缓存的策略。这代表Service Worker已经可以处理功能性的事件fetch (请求)、sync (后台同步)、push (推送)，message（操作dom）。
废弃状态 ( redundant )：这个状态表示一个 Service Worker 的生命周期结束。
