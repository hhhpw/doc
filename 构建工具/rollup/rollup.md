## 安装

  ```js
  npm install rollup --global //全局安装
  rollup 
  ```
  rollup指令检测是否成功安装

## 创建第一个bundle.js

  
  在工程下创建如下目录

  ```js
  -src
    -main.js 
  -a.js 

  // main.js
  import a from '../a.js'; 
  export default function () { 
    console.log(a);
  }
  // a.js
  export default "hello world";
  ```
  执行如下指令
  ```
    rollup src/main.js -o bundle.js -f cjs
  ```
  将会看到打包后的bundle.js出现在项目中

## 添加配置文件 roullup.config.js

  根目录下添加 roullup.config.js文件
 ```js
 export default {
   input: 'src/main.js', // 输入文件
   output: {
     file: 'bundle.js',
     format: 'cjs', //commonJS
   }
 }
 ```
 ```js
  rm -rf bundle.js // 删除老的bundle
  rollup -c //执行配置文件
 ```

  在大多情况下我们将在不同的环境执行不同的配置项，例如请求接口的区别。
  
  - 多份配置文件
  ```js
    rollup.config.dev.js // dev配置
    rollup.config.prod.js // prod配置

    rollup --config rollup.config.dev.js //执行dev环境配置
  ```
  - package.json指令

  ```js
  npm init
  npm install rollup -D
  // 在windows环境下,mac环境大同小异请自己查询
  npm install cross-env -D

  // package.json
  "scripts": {
    "dev": "cross-env NODE_ENV=dev rollup -c",
    "build": "cross-env NODE_ENV=production rollup -c"
  },
  // rollup.config.js添加
  console.log("NODE_ENV", process.env.NODE_ENV);

  npm run dev // 控制台将会输出dev
  npm run build // 控制台将会输出production
  ```

## 添加插件 Plugins

  ```js
  npm install --save-dev rollup-plugin-node-resolve // 以此插件为例

  // rollup.config.js
  import resolve from 'rollup-plugin-node-resolve';
  plugins: [resolve()]
  ```

  常用插件推荐
  - rollup-plugin-node-resolve  ---帮助 Rollup 查找外部模块，然后导入
  - rollup-plugin-commonjs   ---将CommonJS模块转换为 ES2015 供 Rollup 处理
  - rollup-plugin-babel   --- 让我们可以使用es6新特性来编写代码
  - rollup-plugin-terser  --- 压缩js代码，包括es6代码压缩
  - rollup-plugin-eslint  --- js代码检测
  - rollup-plugin-alias --- 别名插件
  - rollup-plugin-serve --- 开启本地服务
  

## webpack比较

  - 不支持代码拆分
  - 不支持运行时态的动态导入
  - 主打Tree-shaking, 也被称为 "live code inclusion," 它是清除实际上并没有在给定项目中使用的代码的过程，但是它可以更加高效。

  


## 完整的配置结构

```js

// rollup.config.js
console.log("NODE_ENV", process.env.NODE_ENV);
import resolve from 'rollup-plugin-node-resolve';
import babel from 'rollup-plugin-babel';
import commonjs from 'rollup-plugin-commonjs';
import { terser } from 'rollup-plugin-terser';
import * as meta from "./package.json";
import serve from 'rollup-plugin-serve';
let barnner = `// Copyright ${(new Date).getFullYear()} ${meta.author}`;
export default {
  input: 'src/main.js',
  output: {
    file: 'bundle.js',
    name: 'nice', // umd格式必填
    format: 'umd' //  "amd", "cjs", "system", "esm", "iife" or "umd"
  },
  plugins: [
    resolve(),
    babel({
      exclude: "node_modules/**", //不编译node_modules
      runtimeHelpers: true, // 开启runtime，避免重复引包
    }),
    commonjs(),
    serve({
      open: true,
      contentBase: '',
      historyApiFallback: true,
      host: 'localhost',
      port: 8888,
      openPage: '/index.html',
    }),
    terser({
      output: {
        preamble: barnner,
      }
    })
  ]
};

// babelrc
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "modules": false,
        "useBuiltIns": "usage",
        "corejs": "3",
        "targets": {
          "browsers": "> 1%, IE 11, not op_mini all, not dead",
          "node": 8
        }
      }
    ]
  ],
  "plugins": [
    [
      "@babel/plugin-transform-runtime"
    ]
  ]
}

// package.json
{
  "name": "rollup-test",
  "version": "1.0.0",
  "description": "rollup-test",
  "scripts": {
    "dev": "cross-env NODE_ENV=dev rollup -c",
    "build": "cross-env NODE_ENV=production rollup -c"
  },
  "main": "a.js",
  "author": "YOUR NAME",
  "license": "ISC",
  "devDependencies": {
    "@babel/core": "^7.7.4",
    "@babel/runtime": "^7.7.4",
    "@babel/plugin-transform-runtime": "^7.7.4",
    "@babel/preset-env": "^7.7.4",
    "cross-env": "^6.0.3",
    "rollup": "^1.27.8",
    "rollup-plugin-babel": "^4.3.3",
    "rollup-plugin-commonjs": "^10.1.0",
    "rollup-plugin-node-resolve": "^5.2.0",
    "rollup-plugin-serve": "^1.0.1",
    "rollup-plugin-terser": "^5.1.2"
  },
  "dependencies": {
    "@babel/polyfill": "^7.7.0",
    "core-js": "^3.4.7"
  }
}

```



  

    
 
