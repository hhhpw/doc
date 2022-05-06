
/Users/bj00112ml/.nvm/versions/node/v16.14.2/bin
vue-cli、npm等指令都会在这这里


```js

mkdir wind-cli
touch cli.js
scripts里bin: cli.js
// 本地使用而不发布
yarn link 或npm link
wind-cli 
输出：wind-cli working~

```

- inquirer 交互命令
- commander 指令 -v 查看版本等
- chalk 格式化输出 颜色、样式等
- log-symbols 为各种日志级别提供着色的符号（类似下载成功的√）
- ora 显示下载中，可以设置样式等；有start，fail，succeed方法等。
- download-git-repo 用于下载git撒过的模版项目


https://juejin.cn/post/6966119324478079007#heading-2
https://juejin.cn/post/6911987404039520270