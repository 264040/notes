# nginx常用命令（windows）

```nginx


// 启动nginx服务
start nginx 或 start nginx.exe

// 正常关闭nginx服务
nginx -s quit

// 快速关闭nginx服务
nginx -s stop	或 nginx.exe -s stop

// 重启(在开启的状态下才能重启)
nginx.exe -s reload


// 查看正在运行的nginx
tasklist /fi "imagename eq nginx.exe"

// 停止全部正在运行的nginx
taskkill /f /t /im nginx.exe


```

