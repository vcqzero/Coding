```shell
# 备份
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

# 设置国内源,新增一个国内源的source.list
sudo vim /etc/apt/sources.list.cn
# 粘贴以下内容
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse

# 替换原来的文件
sudo cp /etc/apt/sources.list.cn /etc/apt/sources.list

# update
sudo apt update
```

## 系统版本

不同的系统版本，对应的国内源地址是不一样的

```shell
# 查看Ubuntu名称
lsb_release -a
```

