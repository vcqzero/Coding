```shell
# 第一步：执行 git reflog
git reflog 
d185b35 HEAD@{0}: rebase finished: returning to refs/heads/passport
d185b35 HEAD@{1}: rebase: checkout origin/passport
4ab8e52 HEAD@{2}: checkout: moving from passport_email to passport
d185b35 HEAD@{3}: checkout: moving from passport to passport_email
4ab8e52 HEAD@{4}: checkout: moving from passprot_bak to passport
d185b35 HEAD@{5}: checkout: moving from passport to passprot_bak
4ab8e52 HEAD@{6}: checkout: moving from passprot_bak to passport
d185b35 HEAD@{7}: commit: 添加邮件发送处理操作
............

这里显示了commit 的sha, version, message，找到你消失的commit，然后可以使用这个‘消失的’commit重新建立一个branch.
# 第二步：选择一个commit，创建新分支
$git checkout -b branch-bak [commit-sha]
# 第三步：将代码合并即可
然后可以出来冲突，再git add, git commit, git push等操作，把修改提交。
```

