### homebrew
[地址](https://brew.sh/)

- 下载

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
需要翻墙
也可以浏览器打开
https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh 
保存sh文件

```bash
bash brew_install.sh
```

- 卸载
注意install.sh改为uninstall.sh

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/uninstall.sh)"
```


### nvm

brew 自带可以安装

```bash
    brew install nvm
```

也可
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
touch ~/.bash_profile 
将
export NVM_DIR=~/.nvm
source ~/.nvm/nvm.sh
放入bash_profile

```



- 常用指令
  
nvm ls-remote  查看 Node 远程版本库
nvm install node  将安装最新版本的 Node
nvm install v12.7.0  将安装 12.7.0 版本的 Node
nvm uninstall v12.7.0  卸载 12.7.0 版本的 Node
nvm ls  查看已经安装的 Node 版本
nvm use v12.7.0 切换 12.7.0 为当前使用的版本
nvm alias default v12.7.0 将 12.7.0 设置为 Node 的默认版本
nvm which v12.7.0 查看 12.7.0 版本的 Node 的安装目录，比如：/Users/ccp/.nvm/versions/node/v12.7.0/bin/node
nvm --help  查看更多命令用法



