# Git使用笔记
## Git配置
[TOC]
### 1.	配置姓名
`git config global user.name "afanit"`
### 2.	配置邮箱
`git config global user.email "afanti@ncepu.edu.cn"`
### 3.	本地初始化仓库
`mkdir www;
cd www; 
git init` 
>会生成.git目录

### 4.	将文件添加到暂存区里面且提交
`git add readme.txt;
git commit readme.txt m "readme.txt提交"`
> 提交部分文件 
git commit <文件名1 文件名2 文件名n> m <评论> 
提交全部文件
git commit a m <评论>
### 5. 当readme.txt文件被修改时查看修改的变化
`git diff readme.txt;
git status;
git add readme.txt;
git commit m "提交修改的变化"`

### 6. 查看历史版本并回退
`git log;
git reset hard 3df24605fea5dc8151325761723769a2c25d2878;`
>回退上一个版本：`git reset hard HEAD^` 

### 7. 查看提交版本历史记录并撤销回退操作到最新版本
`git reflog;
git reset hard 99a6a92`
### 8. 创建+切换分支
`git checkout b dev`
> 在dev分支进行开发;git add;git commit 后不影响master分支。git merge dev合并分支。主分支就会与dev合并

### 9. 查看当前分支
`git branch`
### 10.切换分支
`git checkout dev`
### 11.合并某分支到当前分支
`git merge dev`
### 12.删除分支
`git branch d dev`
### 13.查看文件修改
`git diff master` 查看与master分支的内容
### 14.删除远程仓库的文件或目录

    git rm -r --cached a/2.txt //删除a目录下的2.txt文件 
    git rm -r --cached a  删除a目录
	git commit -m "删除a目录下的2.txt文件" 
	git push

> **Note:**
用r参数删除目录, git rm cached a.txt 删除的是本地仓库中的文件，且本地工作区的文件会保留且不再与远程仓库发生跟踪关系，如果本地仓库中的文件也要删除则用git rm a.txt
### 15.每次开发时更新代码
`git pull`
### 16.解决冲突
> 什么时候能够产生冲突？一般在一个分支下增加1.txt，在主分支也在1.txt文件更新了代码。当主分支合并这个分支的时候就会产生冲突。

    git checkout b fenzhi1 //建立一个分支
    echo "1111" >>readme.txt
    git add readme.txt
    git commit m "在fenzhi1添加了111"
    git checkout master //切换到主分支
    echo "2222" >>readme.txt
    git add readme.txt 
    git commit m "在master分支添加了222"
    git merge fenzhi1 //这样就会产生冲突
	解决办法：
	查看状态：git status
	修改冲突文件，看保留哪个分支的代码：
	cat readme.txt 
	<<<<<<<HEAD
	2222         //是指主分支修改的内容
	========
	1111         //是指fenzhi1上修改的内容
	>>>>>>>fenzhi1
	git add readme.txt 
	git commit m "conflict fixed" //解决冲突
### 多人协作开发
> 当你从远程库克隆时候，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且远程库的默认名称是origin。

    git remote v //查看远程库的详细信息
    origin  https://github.com/kang rui/gitrepo.git (fetch) //从什么地方获取的
	origin  https://github.com/kangrui/gitrepo.git (push)  //推送到哪里
	
	本地开发完提交代码到github
	git remote add origin https://github.com/kangrui/gitrepo.git //关联一个远程库
	git remote -v //查看关联远程库的详细信息
	git push -u origin master //(第一次使用-u以后不用)把当前master推送到远程，要求输入账号密码.
	在本地建立test分支，提交test分支到github上
	git branch test //建立test分支
	git checkout test //切换分支
	coding...
	git push origin test //将test分支提交到github 
	默认git clone下来的是master分支，如果要在test分支下开发，把远程的origin的test分支到本地来,提交到本地后可以将test分支和master合并
	git checkout b test origin/test
	git push origin :test //删除test分支
