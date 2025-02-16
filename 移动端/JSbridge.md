## Bridge 源码

```js
(function () {
  if (window.bridge) {
    return;
  }

  window.bridge = {};
  var messages = {};
  var call_id = 0;

  // register方法端上会自动调用
  bridge.register = function (name) {
    if (this[name]) {
      return;
    }
    // name是与native协商好的函数名, 如getParams
    this[name] = function (params, callback) {
      // params 向端上传递的数据， callback 交互后的回调
      invokeNative(name, params, callback);
    };
  };

  bridge.obtain = function (id) {
    return JSON.stringify(messages[id].params);
  };

  bridge.callback = function (id, data) {
    var message = messages[id];
    if (!message) {
      return;
    }

    if (message.callback) {
      message.callback(JSON.parse(data));
    }
    delete messages[id];
  };

  function invokeNative(name, params, callback) {
    call_id++;
    messages[call_id] = { params: params, callback: callback };
    if (
      navigator.userAgent.indexOf("Android") > -1 ||
      navigator.userAgent.indexOf("Adr") > -1
    ) {
      // 在Android端，会重写alert、prompt、confirm等原生JS方法。
      // 之所以使用prompt是因为该方法使用频率很低，通讯副作用很少
      // 端上会解析出传递的数据
      prompt(JSON.stringify({ cmd: name, id: call_id, params: params }), "");
    } else {
      // IOS端通过创建iframe的方式交流数据
      // 发现以jsbridge：//开头的地址会执行相应的逻辑，不是加载内容
      var iframe = document.createElement("iframe");
      iframe.src =
        "jsbridge://invoke.native.method?handler=" + name + "&id=" + call_id;
      iframe.style.display = "none";
      document.documentElement.appendChild(iframe);
      setTimeout(function () {
        document.documentElement.removeChild(iframe);
      }, 0);
    }
  }
})();
```

## 调用

```js
requrie("bridge.js");

// birdgeReady是为检测bridge方法是否成功引入
// 理论上，第一次调用bridge方法都应该处于bridgeReady方法内
// native会自动检测这个函数，并执行
window.bridgeReady = function () {
  if (window.bridge && window.bridge.getParams) {
    bridge.getParams(null, (data) => {
      // 业务逻辑
    });
  }
};
```

[参考文章](https://segmentfault.com/a/1190000010356403)

## 两端调用为什么不同

这与语言以及 webview 的特性相关

## 为什么 android 是 prompt

在 Android 端，会重写 alert、prompt、confirm 等原生 JS 方法。
之所以使用 prompt 是因为该方法使用频率很低，通讯副作用很少
重写该方法后，便可以操作数据

## 为什么 IOS 是 iframe

iframe 并不是唯一的方法

- 通过 localtion.href；

**通过 location.href 有个问题，就是如果我们连续多次修改 window.location.href 的值，在 Native 层只能接收到最后一次请求，前面的请求都会被忽略掉。**

基于以上副作用，因此选择 iframe

- 通过 iframe 方式；

### JSBridge 的开发，需要哪些知识储备

- scheme 协议是什么
  可以简单理解为自定义的 url
  形式如：[scheme:][//domain][path][?query][#fragment]
  举个栗子：jsbridge://openPage?url=https%3A%2F%2Fwww.baidu.com
- js 调用 native 的方法
  native 通过拦截约定好的 scheme 协议 去执行一些 native 的方法
  约定固定格式的 scheme 协议，例如：[customscheme:][//methodname][?params={data, callback}]
  customscheme：自定义需要拦截的 scheme
  methodName：需要调用的 native 的方法
  params：传递给 native 的参数 和 回调函数名
  通过在 webview 中挂载到全局对象上的对象方法来调用 native 的方法
  举个栗子：window.JSBridge.openPage("https://www.baidu.com")
- native 注入到 webview 全局对象为 JSBridge，通过全局对象 JSBridge 可以调用挂载在其上的方法，来触发调用 native 的方法
- native 还会拦截 js 调用的 alert、confirm、prompt 方法，通过在 js 里使用这三个方法 也能进行 js 对 native 的通信



### 如何实现 JSBridge

(JSBridge 端内实现)[https://zhuanlan.zhihu.com/p/32899522]

1. 功能

   通用功能：app 唤起，关闭 webview，自定义 titlebar，打开一个新的 webview 承接跳转 url，下拉刷新等
   业务功能：微信、微博分享，支付，调用相机、图片上传等、定位信息

2.  两端的规范和统一。统一的对调码表

3. 桥协议的实现。 端调用 js，js 调用端。解析、调用、回调。

### 安卓与 JS 的交互

    对于android调用JS代码的方法有2种：
    1. 通过WebView的loadUrl（）
    2. 通过WebView的evaluateJavascript（）
    3. Android端通过shouldOverrideUrlLoading 拦截URL Schema

    对于JS调用Android代码的方法有3种：
    1. 通过WebView的addJavascriptInterface（）进行对象映射
    2. 通过 WebViewClient 的shouldOverrideUrlLoading ()方法回调拦截 url
    3. 通过 WebChromeClient 的onJsAlert()、onJsConfirm()、onJsPrompt（）方法回调拦截JS对话框alert()、confirm()、prompt（） 消息

[参考](https://mp.weixin.qq.com/s/PipKSnMQaTBhE5kSwG3DVQ)


### JSbridge基本概念

JSBridge 的基本原理是通过一种约定的通信协议，使得 JavaScript 代码能够调用原生应用提供的功能，同时原生应用也能够向 JavaScript 传递信息。这种机制通常通过以下方式实现：

WebView 与 JavaScript 之间的通信：原生应用加载一个 Web 页面（通过 WebView 或类似的组件），在页面中嵌入 JavaScript 脚本。通过 JSBridge，Web 页面可以调用原生代码，原生代码也可以调用 Web 页面中的 JavaScript 函数。
消息的传递：JSBridge 的核心是消息传递机制。通常，消息被封装成对象（例如 JSON），并通过协议传递。原生应用和 JavaScript 通过预定义的协议来解码和处理这些消息。


### 工作原理
JSBridge 主要涉及两个方面的交互：

1. JavaScript 调用原生方法
2. 原生应用调用 JavaScript

#####  JavaScript 调用原生方法
当 JavaScript 需要调用原生平台的功能时（如获取相机权限、分享功能等），JavaScript 代码会发出一个请求，JSBridge 负责将请求转发给原生平台。原生平台执行相应的操作后，可能会返回结果给 JavaScript。

流程：

- JavaScript 发送请求：通过调用预定义的 JavaScript 函数来发起请求，例如 window.JSBridge.callNativeMethod('methodName', params)。
- JSBridge 转发请求：JSBridge 会将请求通过原生平台的通信接口（通常是通过 URL Scheme、消息队列或本地调用等机制）转发给原生代码。
- 原生处理请求并返回结果：原生平台处理请求，执行相应操作，然后通过 JSBridge 将结果传递回 Java路由。
JavaScript 处理返回结果：JavaScript 接收到结果后，通过回调函数或 Promise 等机制处理返回值。

#####  原生应用调用 JavaScript
在某些情况下，原生应用也需要调用 Web 页面中的 JavaScript 函数。这通常发生在 WebView 中加载的页面需要做出响应（例如，处理用户交互后通知原生应用进行某些操作）。

流程：

- 原生调用 JavaScript：原生平台通过 JSBridge 向 WebView 发送 JavaScript 执行请求，例如调用 webView.evaluateJavascript('javascriptFunction();') 或者通过消息队列来发送请求。
- WebView 执行 JavaScript：WebView 内部执行传递的 JavaScript 代码。
返回结果：如果 JavaScript 函数有返回值，WebView 会将结果通过 JSBridge 返回给原生应用。

### 通信协议与实现
#####  URL Scheme（URL 路由）

**在 iOS 中，可以通过监听 URL Scheme 来解析消息并调用原生方法。**
**在 Android 中，可以通过 WebView 的 shouldOverrideUrlLoading 方法来拦截并解析 URL。**

### 如何保证bridge一定加载成功

 1. 延迟加载和初始化时机
 2. 轮询检测 JSBrigde是否加载成功。这个方法会定期检查 JSBridge 是否加载完成，直到它加载成功为止。
 3. 错误回调和重试机制。为了确保在初始化过程中发生错误时能够重新尝试加载，可以为 JSBridge 的加载和调用设置一个回调机制。如果 JSBridge 初始化失败，可以执行一些回退策略，例如重新尝试初始化。


### 调试

1. 日志输出
2. 验证 JSBridge 是否初始化成功
3. 调试 JavaScript 和原生代码的通信协议
4. 模拟错误和断点调试
5. 检查版本和兼容性