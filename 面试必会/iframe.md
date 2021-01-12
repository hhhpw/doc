

### iframe

主要的属性：height、width、src、scrolling






### 父子交互

```js
// 获取元素
let sonIframe = document.getElementById('soniframe');
// 获取子iframe的window
let sonWindow = sonIframe.contentWindow;
// 向子页面传递发送消息  
sonWindow.postMessage(JSON.stringify(data), iframeUrl)
// 监听子iframe传来的数据
window.addEventListener('message', receiveMsg, false)
function receiveMsg(event) {
  console.log(e.data)  //e.data为传递过来的数据
  console.log(e.origin)  //e.origin为调用 postMessage 时消息发送方窗口的 origin（域名、协议和端口）
  console.log(e.source)  //e.source为对发送消息的窗口对象的引用，可以使用此来在具有不同origin的两个窗口之间建立双向通信
  // 这里不准确，chrome没有这个属性
  // var origin = event.origin || event.originalEvent.origin;
  var origin = event.origin
  if (origin !== "http://example.org:8080")
    return;

  // ...
  console.log("event", event.data)
}

// iframe子页面调用父页面的变量、js方法、元素(非跨域)：
window.parent.varName; //获取父页面js全局变量
window.parent.fnName; //获取父页面js全局方法
window.parent.document.getElementById(id); //获取父页面元素

// 父页面调用iframe页面中的变量、js方法、元素(非跨域)：
window.frames[frameName][0] =  document.getElementById(frameId).contentWindow;
window.frames[frameName][0].varName; //获取子页面js全局变量
window.frames[frameName][0].fnName; //获取子页面js全局方法
window.frames[frameName][0].document.getElementById(id); //获取子页面元素
```

[postMessage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage)
