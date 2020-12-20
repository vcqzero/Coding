```shell
# install
sudo apt install -y zsh
# use
chsh -s /bin/zsh
```

安装

| Method    | Command                                                      |
| --------- | ------------------------------------------------------------ |
| **curl**  | `sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"` |
| **wget**  | `sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"` |
| **fetch** | `sh -c "$(fetch -o - https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"` |

.zshrc配置参考

```shell
# Android path eg:adb
export ANDROID_HOME=$HOME/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
export PATH=$PATH:$ANDROID_HOME/emulator

# Java
export JAVA_HOME=$(/usr/libexec/java_home)
export PATH=$JAVA_HOME/bin:$PATH
export CLASS_PATH=$JAVA_HOME/lib

# Composer path
export PATH=$PATH:/Users/qinchong/.composer/vendor/bin

#Php 7.4
export PATH=$PATH:/usr/local/Cellar/php/7.4.10/bin

# 配置zsh
ZSH_THEME="steeef"
plugins=(git z)
...
alias gpr="git pull --rebase"
alias cl="clear"
```

