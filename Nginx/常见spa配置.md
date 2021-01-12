```bash

location /music {
  root /home/work/output/dev #文件访问的路径
  index index.html
  try_files $uri $uri/ /index.html =404; # $uri即为/music，当路由不符合时反馈页面为i
}


location ~* ^/music/.*\.(js|css|gif|jpg|jpeg|png)$ {
  root /home/work/output/dev
  access_log off
  expires 1d
  # gzpi压缩
  gzip on;
  gzip_comp_level 3;
  gzip_types text/plain application/javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg
  image/gif image/png;
}


# vue-cli3的配置

location /music {
  root /home/work;
  index index.html;
  try_files $uri $uri/ @musicRouter; # 截取404的uri，传给 @router
}

location @musicRouter {
rewrite ^.*$ /music/index.html last; # 接到截取的uri 并按一定规则重写uri和vue路由跳转
}


```