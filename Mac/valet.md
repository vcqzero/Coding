# valet

valet是mac上php开发环境管理工具

命令	描述
valet forget	从一个「驻留」目录运行该命令将会将其从主流目录列表中移除。
valet log	查看 Valet 服务记录的日志列表。
valet paths	查看所有「驻留」的路径。
valet restart	重启 Valet 守护进程。
valet start	启动 Valet 守护进程。
valet stop	停止 Valet 守护进程。
valet trust	为 Brew 和 Valet 添加文件修改权限使 Valet 输入命令的时候不需要输入密码。
valet uninstall	卸载 Valet 守护进程：显示手动卸载的向导或使用 --force 命令强制卸载之。

https://learnku.com/docs/laravel/8.x/valet/9358#installation

## 安装valet

https://learnku.com/docs/laravel/8.x/valet/9358#installation

Valet 仅支持 Mac，并且需要你直接在本地计算机上安装 PHP 和数据库服务器。这很容易通过使用 Homebrew 命令来实现，

 ```shell
brew install php 

# mysql安装之后root默认密码为空
brew install mysql@5.7
# 访问mysql
mysql -uroot 
 ```

Valet 以最少的资源消耗提供了一个极快的本地开发环境，非常适合只需要 PHP 和 MySQL 且不需要完全虚拟化开发环境的开发者。



## 配置站点

安装valet之后，valet服务会自动代理所有*.test域名；

对于具体项目需要指定域名， 如：

```shell
# link
cd laravel项目目录
valet link api.jjhy.test 

# 查看所有links
valet links
```

## 修改端口

默认情况下，valet会代理所有*.test域名的80端口，同时限制流量必须是127.0.0.1

现在修改为:

```shell
server {
 ...
 # 监听所有接口的1002端口
 listen 1002 default_server;
 ...
}
```



## 启动 

```shell
# 启动mysql
brew services start mysql@5.7

# 启动valet
valet start
```



## 分享

在项目目录执行

```shell
# 执行下面命令，会打印出可访问的域名
valet share 
```

php artisan down --secret="163o09k1"  