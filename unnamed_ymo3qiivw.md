---
note:
    id: ""
    tags: [backend/git]
    modifiedAt: 2020-06-01T14:14:12.947Z
    createdAt: 2020-06-01T14:12:28.636Z
---
# Git 代理的问题
> https://www.v2ex.com/t/641227#r_8529977

由于在国内 clone，fetch 等速度很慢，需要挂上代理：

挂代理，如果是 HTTP 协议，那么像楼上说的那样
`git config --global http.proxy 'socks5h://localhost:port'`
或者指定单给某个域名的仓库设置：
`git config --global 'http.https://github.com.proxy' 'socks5h://localhost:port'`

如果是 SSH 协议，那么就要编辑 `~/.ssh/config` 文件（没有的话新建一个，权限 600 ），增加这个配置：
```
Host github.com
ProxyCommand /usr/bin/nc -X 5 -x localhost:port %h %p
```
其中 `%h` 和 `%p` 要原样保留，`-X 5` 表示使用 socks5 代理。
需要 BSD 的 netcat （ nc ）工具。
例如 Ubuntu 可使用 `sudo apt install -y netcat` 来进行安装。

