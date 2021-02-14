# rebase的使用

## --rebase

```shell
git pull --rebase
```

## 合并多个commit

```shell
git rebase -i COMMIT_ID #  将COMMIT_ID之后的commits进行合并

# 执行上一步之后，会进入vim
# 在这一步需要选择将哪些提交合并到哪些提交上
# 合并之后，需要修改commit message
```

