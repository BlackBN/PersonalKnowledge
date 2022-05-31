# git 命令

```bash
#github更新fork的代码:
#1.在本地装好github客户端，或者git客户端
#2.clone自己的fork分支到本地,可以直接使用github客户端,clone到本地,如果使用命令行,命令为:
$ git clone git@github.com:BlackBN/customers.git
#3.增加源分支地址到你项目远程分支列表中(此处是关键)，先将原来的仓库指定为upstream，命令为:
$ git remote add upstream git@github.com:caicloud/customers.git
$ git remote -v #查看远程分支列表
#4.fetch源分支的新版本到本地
$ [master]> git fetch upstream
#5.合并两个版本的代码
$ [master]> git merge upstream/master
#6. 将合并后的代码push到github上去
$ [master]> git push origin master
$ git push origin cps-2.8.5/best --force-with-lease
```

```bash
#删除之前的一次提交，重新commit
$ git reset HEAD^（退回上一次 commit 前的修改）（HEAD~2 ： 回退两次）
$ git add .
$ git commit -m "xxxx"
$ git push -f （强制提交）
```

```bash
git config --global --add url."git@github.com:".insteadOf "https://github.com/"  # bn@bytedance 这个用户已经用过
```
