### 外部访问ssl

```
# 添加代理
netsh  interface portproxy add v4tov4 listenaddress=192.168.0.116 listenport=3000 connectaddress=127.0.0.1 connectport=3000

# 查看代理
netsh interface portproxy show all

# 查看端口
ipconfig
```



