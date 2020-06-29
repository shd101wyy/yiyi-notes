---
tags:
  - backend/nginx
created: 2020-03-21T11:11:13.327Z
modified: 2020-03-21T11:38:38.078Z
---

# 关于 nginx 的路径配置问题（新手）

刚开始学习 nginx，想要让我的 `http://ip.com/hcy` 目录指向我机子上面的 `/home/yiyiwang/hcy` 目录。

一开始编写的 `/etc/nginx/sites-available` 中的配置如下

```
location ^~ /hcy/ {
  root /home/yiyiwang/hcy;
}

location / {
        # First attempt to serve request as file, then
        # as directory, then fall back to displaying a 404.
        try_files $uri $uri/ =404;
}

```

重启 nginx 后发现 `http://ip.com/hcy` 总是 404。  
接着读了[这篇文章](https://blog.csdn.net/zhaoyangjian724/article/details/48524091) 才知道配置应该这么写

```
location ^~ /hcy/ {
  root /home/yiyiwang;
}

location / {
        # First attempt to serve request as file, then
        # as directory, then fall back to displaying a 404.
        try_files $uri $uri/ =404;
}

```

得确保 `/home/yiyiwang` 下面有 `./hcy` 目录。

感觉要学的东西很多。。
