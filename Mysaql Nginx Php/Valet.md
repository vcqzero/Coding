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

# 从links中查看url
```



## 启动 

```shell
# 启动mysql
brew services start mysql@5.7

# 启动valet
valet start
```


