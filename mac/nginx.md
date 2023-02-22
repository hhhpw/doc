# mac安装Nginx


### brew install nginx

如果报错多是镜像问题

- 替换brew.git

```bash

cd "$(brew --repo)"
git remote set-url origin https://mirrors.aliyun.com/homebrew/brew.git
```

- 替换homebrew-core.git

```bash
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.aliyun.com/homebrew/homebrew-core.git

```

- 再次下载
```bash
brew update
brew install nginx
```

### 常用指令


- 查看nginx的安装等目录信息
```bash
brew info nginx
```

- 启ng服务
```bash
brew services start nginx
```

- 重启ng
```bash
brew services restart nginx
```

- 查看nginx.conf配置文件目录
```bash
ps -ef | grep nginx
```

- 查看nginx安装目录

```bash
ps -ef | grep nginx
```

- 关闭服务
```bash
 nginx -s stop
```
