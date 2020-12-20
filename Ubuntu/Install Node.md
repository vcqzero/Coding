```shell
# 安装nodejs
# 添加14的源
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
# 安装
sudo apt install -y nodejs
```

```shell
# 安装yarn
curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt update && sudo apt install yarn

# 配置淘宝镜像
yarn config set registry https://registry.npm.taobao.org/

# 查看当前镜像
yarn config get registry
```

