# Docker使用

## 镜像

```
docker search nginx // 查找镜像

docker pull nginx:latest // 拉去nginx最新版本镜像

docker images // 查看安装的所有镜像
```



### 容器

```
// 启动容器
docker run 容器名称 -p 8080:80 -d 

// 查看容器
docker ps -a 

// 删除容器
docker rm id

// 停止容器
docker stop id

// 启动/重启
docker start/restart id

// 进入容器
docker exec -it CONTAINER_ID /bin/bash

```

### 文件操作

```shell
// 将主机/www/runoob目录拷贝到容器96f7f14e99ab的/www目录下。
docker cp /www/runoob CONTAINER:/www/
```

## 挂载卷

```shell
// 删除无用的volume
docker volume prune
```



