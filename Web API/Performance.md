## 性能检测 window.performance.timing



[参考资料](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/performance)

白屏时间: window.performance.timing.domLoading - window.performance.timing.navigationStart

首屏时间: window.performance.timing.domInteractive  - window.performace.timing.navigationStart




### 几个重要的属性

- navigationStart： 页面开始请求的时间点

- domloading： 返回当前网页DOM结构开始解析时（即Document.readyState属性变为“loading”)

- domInteractive：返回当前网页DOM结构结束解析、开始加载内嵌资源时（即Document.readyState属性变为“interactive”）

- domContentLoadedEventStart：返回当解析器发送DOMContentLoaded 事件，即所有需要被执行的脚本已经被解析时的Unix毫秒时间戳

- domComplete： 返回当前文档解析完成，即Document.readyState 变为 'complete'且相对应的readystatechange 被触发时的Unix毫秒时间戳


