## Bridge源码
```js
; (function () {
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
    messages[call_id] = { 'params': params, 'callback': callback };
    if (navigator.userAgent.indexOf('Android') > -1 || navigator.userAgent.indexOf('Adr') > -1) {
      // 在Android端，会重写alert、prompt、confirm等原生JS方法。
      // 之所以使用prompt是因为该方法使用频率很低，通讯副作用很少
      // 端上会解析出传递的数据
      prompt(JSON.stringify({ 'cmd': name, 'id': call_id, 'params': params }), '');
    } else {
      // IOS端通过创建iframe的方式交流数据
      // 发现以jsbridge：//开头的地址会执行相应的逻辑，不是加载内容
      var iframe = document.createElement('iframe');
      iframe.src = 'jsbridge://invoke.native.method?handler=' + name + '&id=' + call_id;
      iframe.style.display = 'none';
      document.documentElement.appendChild(iframe);
      setTimeout(function () { document.documentElement.removeChild(iframe); }, 0);
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
    bridge.getParams(null, data => {
      // 业务逻辑
    })
  }
}
```

[参考文章](https://segmentfault.com/a/1190000010356403)


## 两端调用为什么不同

  这与语言以及webview的特性相关

## 为什么android是prompt

  在Android端，会重写alert、prompt、confirm等原生JS方法。
  之所以使用prompt是因为该方法使用频率很低，通讯副作用很少
  重写该方法后，便可以操作数据

## 为什么IOS是iframe

  iframe并不是唯一的方法

  - 通过localtion.href；

  
  **通过location.href有个问题，就是如果我们连续多次修改window.location.href的值，在Native层只能接收到最后一次请求，前面的请求都会被忽略掉。**

  基于以上副作用，因此选择iframe

  - 通过iframe方式；



### 如何实现JSBridge
(JSBridge端内实现)[https://zhuanlan.zhihu.com/p/32899522]




### 安卓与JS的交互

    对于android调用JS代码的方法有2种： 
    1. 通过WebView的loadUrl（） 
    2. 通过WebView的evaluateJavascript（）

    对于JS调用Android代码的方法有3种： 
    1. 通过WebView的addJavascriptInterface（）进行对象映射 
    2. 通过 WebViewClient 的shouldOverrideUrlLoading ()方法回调拦截 url 
    3. 通过 WebChromeClient 的onJsAlert()、onJsConfirm()、onJsPrompt（）方法回调拦截JS对话框alert()、confirm()、prompt（） 消息