[参考资料](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch)



 [参考资料](https://segmentfault.com/a/1190000003810652)


 

 
 


 ### 与传统ajax的区别：


  1.  基于Promise
  2. 返回的是一个**可读流形式**，需要调用相应方法去解析。

  ```js
res.json() // 解析为json
res.blob() // 解析为blob
res.formData // 解析为FormData
res.text() // 解析为文本
res.arrayBuffer() // 解析为ArrayBuffer

  ```

  3. 当HTTP状态是400,500时，不会被reject。只有当网络请求错误才会被reject,
  判断是否请求成功是res.ok

  4. fetch默认不cookie，需要设置fetch(url, {credentials:'include'})