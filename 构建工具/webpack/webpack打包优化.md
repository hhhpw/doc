
## 查看资源依赖度

```js
npm install webpack-bundle-analyzer --save-dev

```


## 插件

  mini-css-extract-plugin 分离抽取css
  uglifyjs-webpack-plugin 压缩JS
  Happypack 

## externals外部配置扩展
webpack 中的 externals 配置提供了不从 bundle 中引用依赖的方式。**解决的是，所创建的 bundle 依赖于那些存在于用户环境(consumer environment)中的依赖。**
怎么理解呢，**意思是如果需要引用一个库，但是又不想让webpack打包（减少打包的时间），并且又不影响我们在程序中以CMD、AMD或者window/global全局等方式进行使用（一般都以import方式引用使用），那就可以通过配置externals。**

**这样做的目的就是将不怎么需要更新的第三方库脱离webpack打包，不被打入bundle中，从而减少打包时间，但又不影响运用第三方库的方式，例如import方式等。**
```
externals: {
    "react": 'React'
  },
```

## cdn方式
```js

<script src={cdnPath}"/react.js"></script>
```


## tree-shaking 剔除无用代码

Tree-shaking是依赖ES6模块静态分析的，ES6 module的特点如下：

**只能作为模块顶层的语句出现**
**import 的模块名只能是字符串常量**
**import binding 是 immutable的**
**依赖关系确定，与运行时无关，静态分析。正式因为ES6 module的这些特点，才让Tree-shaking更加流行。

主要特点还是依赖于ES6的静态分析，在编译时确定模块。如果是require，在运行时确定模块，那么将无法去分析模块是否可用，只有在编译时分析，才不会影响运行时的状态。

副作用 side-effect
[副作用](https://segmentfault.com/a/1190000019220154)

在production环境下，mode:在production环境下，mode
在package.json sideEffects: false

##  alias 别名

```
alias: {
  Util: path.resolve(dirname, 'src/Util/'),
}
```

减少导入路径检索


## 优化loader配置
exclude、include都应配置且准确

## DLL

DLL方式就是通过配置，告诉webpack指定库在项目中的位置，从而直接引入，不将其打包在内
DLL方式就是指定包在的项目中，build的时候不在打包对应的包，使用的时候引入。
webpack通过webpack.DllPlugin与webpack.DllReferencePlugin两个内嵌插件实现此功能
新建webpack.dll.config.js

```js
  plugins: [
    new webpack.DllPlugin({
      path: './build/bundle.manifest.json',
      name: '[name]_library',
    })
  ]

  path：manifest.json #文件的输出路径，这个文件会用于后续的业务代码打包；
  name：dll 暴露的对象名，要跟output.library保持一致;
  context：解析包路径的上下文，这个要跟接下来配置的 


## image-webpack-loader压缩图片
			{
				test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
				use: [
					{
						loader: "url-loader",
						options: {
							limit: 10000,
							name: "static/images/[name].[hash:7].[ext]",
							outputPath: "carinpro", // 指定输出目录 配合ng
							publicPath: "./"
						}
					},
					{
						loader: "image-webpack-loader",
						options: {
							mozjpeg: {
								progressive: true,
								quality: 65
							},
							optipng: {
								enabled: false
							},
							pngquant: {
								quality: [0.65, 0.90],
								speed: 4
							}
						}
					}
				]
			},
```

## Happypack

在使用 Webpack 对项目进行构建时，会对大量文件进行解析和处理。当文件数量变多之后，Webpack 构件速度就会变慢。由于运行在 Node.js 之上的 Webpack 是单线程模型的，所以 Webpack 需要处理的任务要一个一个进行操作。

而 Happypack 的作用就是将文件解析任务分解成多个子进程并发执行。子进程处理完任务后再将结果发送给主进程。所以可以大大提升 Webpack 的项目构件速度
