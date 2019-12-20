## 使用

```js
// 全局
Vue.filter('capitalize', function(value) {

})

// 局部注册
filters: {
  capitalize: function(value) {
  
  }
}
```

```js

<!-- 在双花括号中 -->
{{ message | capitalize }}

<!-- 在 `v-bind` 中 -->
<div v-bind:id="rawId | formatId"></div>
```

过滤器函数总接收表达式的值 (之前的操作链的结果) 作为第一个参数。在上述例子中，capitalize 过滤器函数将会收到 message 的值作为第一个参数。

过滤器可以串联：

```js
{{ message | filterA | filterB }}

```

过滤器是 JavaScript 函数，因此可以接收参数：
```js
{{ message | filterA('arg1', arg2) }}
```
这里，filterA 被定义为接收三个参数的过滤器函数。其中 message 的值作为第一个参数，普通字符串 'arg1' 作为第二个参数，表达式 arg2 的值作为第三个参数。

