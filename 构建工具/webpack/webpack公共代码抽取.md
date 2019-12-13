## CommonsChunkPlugin(4.0以后移除)

    CommonsChunkPlugin 插件，是一个可选的用于建立一个独立文件(又称作 chunk)的功能，
    这个文件包括多个入口 chunk 的公共模块。
    通过将公共模块拆出来，最终合成的文件能够在最开始的时候加载一次，
    便存到缓存中供后续使用。
    这个带来速度上的提升，因为浏览器会迅速将公共的代码从缓存中取出来，
    而不是每次访问一个新页面时，再去加载一个更大的文件。


## optimization.splitChunks

webpack4新增optimiaztion选项
```js

// 优化
optimization: {
  // 可选项
  splitChunks: {
    chunks: "async”, 
    //默认作用于异步chunk，值为all/initial(初始)/async/function(chunk),值为function时第一个参数为遍历所有入口chunk时的chunk模块，chunk._modules为gaichunk所有依赖的模块，通过chunk的名字和所有依赖模块的resource可以自由配置,会抽取所有满足条件chunk的公有模块，以及模块的所有依赖模块，包括css
    minSize: 30000,  //表示在压缩前的最小模块大小， 默认值是30kb
    minChunks: 1,  // 表示被引用次数
    maxAsyncRequests: 5,  //所有异步请求不得超过5个
    maxInitialRequests: 3,  //初始话并行请求不得超过3个
    name: true,  //打包后的名称，默认是chunk的名字通过分隔符（默认是～）分隔开，如vendor~
    automaticNameDelimiter: "~", // 文件名的连接符
    // 缓存组
    cacheGroups: {
      // chunkName。在html-webpack-plugin里chunks添加，
      // 里面的配置同外部，但会根据具体的打包规则覆盖
      vendors: { 
        name: 'vendor', // 生成的文件名
        chunks: "all",  // 同上chunks
        priority: 10, // 权重，权重越高越先提取
        test: (module, chunks) {
          // 也可是正则表达式，满足提取代码的条件
        },
        reuseExistingChunk: true，
        //如果该chunk中引用了已经被抽取的chunk，直接引用该chunk，不会重复打包代码
      }
    }
  }
}

```

##  提取的公共代码添加到Html里
```js
new HtmlWebpackPlugin({ 
  	chunks: ["vendors", "home"], //指定所引用的JS 不需要再加js后缀
})
```
如果不生效，可下载html-webpack-plugin@next
再重新build
