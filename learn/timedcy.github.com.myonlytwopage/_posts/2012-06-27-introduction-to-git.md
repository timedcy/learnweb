---
published: false
layout: post
title: "git速查手册"
description: ""
category: 振导社会
date: 2011-09-27 13:37:00
tags: [程序, git, 速查, 简介, 工具]
---
{% include JB/setup %}


##git 代码库的结构
![git代码库结构]({{ site.img_url }}/2012-06-27-git-transport.png)

上图展示了git代码仓库的结构以及执行相关命令后数据的变迁流程。git 维护的代码分成三部分：current working directory（当前工作目录）、index file、git repository（git仓库）。

- git add 会将“当前工作目录”的改变写到“index file”；
- git commit 会将“index file”中的改变写到git仓库；
- git commit -a 则会直接将“当前工作目录”的改动同时写到“index file”和“git仓库”。

将“当前工作目录”记为 (1)，将 index file 记为 (2)，将“git仓库”记为 (3)。他们之间的提交层次关系是 (1) -> (2) -> (3)，从时间上看，可以认为(1)是最新的代码，(2)比较旧，(3)更旧。三者之间的数据变化：
<!--more-->
- git add 完成的是(1) -> (2)；
- git commit 完成的是(2) -> (3)；
- git commit -a 两者的直接结合。  

查看三者之间的变化：

- git diff 得到的是从(2)到(1)的变化
- git diff --cached 得到的是从(3)到(2)的变化
- git diff HEAD 得到的是从(3)到(1)的变化

github给出了如下图所示的[简明git可视化教程](http://marklodato.github.com/visual-git-guide/index-en.html)。
![git basic usage](http://marklodato.github.com/visual-git-guide/basic-usage.svg)

##git 中的常量

常量名 | 含义 
:---------- | :-----------
HEAD&#94; | 最近一次的commit        
HEAD&#94;&#94; | HEAD父母的信息      
HEAD~4 | HEAD父母的父母的信息
HEAD&#94;1 | HEAD的第一个父母的信息
HEAD&#94;2 | HEAD的第二个父母的信息
COMMIT_EDITMSG | 最后一次commit时的提交信息
MERGE_HEAD | 如果是merge产生的commit，那么它表示除HEAD之外的另一个父母分支。
FETCH_HEAD | 使用git-fetch获得的object和ref的信息都存储在这里，这些信息是为日后git-merge准备的。 

##git 的基本命令


基本命令 | 含义
:-------------- | :------------
git help command | 查看帮助
git command --help |查看帮助
git config --global user.name \<username\> | 设置用户名
git config --global user.email \<email\>  | 设置用户邮箱
git config --list | 查看用户信息
git init | 初始化
git clone /path/to/repository | 创建一个本地仓库的克隆
<span style="white-space:nowrap">git clone username@host:/path/to/repository</span> | 创建一个远端服务器仓库的克隆
git add \<filename\> | 将改动添加到缓存区
git add * | 将改动添加到缓存区
git commit -m "代码提交信息" | 提交改动
git commit --amend -m "新的提交信息" | 提交改动（若有改动） + 修补最近一次提交
git push origin master | 这些改动提交到远端仓库master分支
git remote add origin \<server\> | 修改推送到远程的服务器
git checkout -b feature_x | 创建一个叫做“feature_x”的分支，并切换过去
git checkout master | 切换回主分支
git branch -d feature_x | 删除分支
git pull | 更新本地仓库至最新
git merge \<branch\> | 合并其他分支到当前分支
git diff \<source_branch\> \<target_branch\> | 查看分支间差异
git log | 获取提交 ID
git tag 1.0.0 \<ID\> | 创建一个叫做 1.0.0 的标签
git checkout -- \<filename\> | 用git仓库的HEAD替换当前工作目录的改动   

___

git add 命令 | 含义
:-------------- | :------------
git add file_name       |加单个文件
git add .          |加当前目录的所有文件
git add -i |进入交互式add
git add -p |直接进入补丁模式，可以暂存修改的一部分。

<span style="white-space:nowrap">git commit 命令</span> | 含义
:-------------- | :------------
<span style="white-space:nowrap">git commit -a</span> | 第一步：自动地add所有改动的代码，使得所有的开发代码都列于index file中；第二步：自动地删除那些在index file中但不在工作树中的文件；第三步：执行commit命令来提交。
  
git push 命令 | 含义
:-------------- | :------------
git push origin test:test2 |提交本地test分支作为远程test2分支
git push origin test |提交本地test分支作为远程test分支
git push origin :test |删除远程test分支
git push origin :refs/tags/t4 |删除远程tag标签t4

git pull 命令 | 含义
:-------------- | :------------
git pull origin test:test2 |取回远程的test分支作为本地的test2分支

git checkout 命令 | 含义
:-------------- | :------------
git checkout test |如果本地无test分支，先需要从远程pull回test分支

git branch 命令 | 含义
:-------------- | :------------
git branch b4 t4 |以tag t4建立新的分支b4

git merge 命令 | 含义
:-------------- | :------------
git merge AAA      |将分支AAA与当前分枝合并

git diff&nbsp;命令 | 含义
:-------------- | :------------
git diff&nbsp; |查看working tree与index file的差别的（这个命令只在git add之前使用有效。如果已经add了，那么此命令输出为空）。
<span style="white-space:nowrap">git diff --cached</span> |查看index file与commit的差别的（这个命令在git add之后在git commit之前有效。）。
git&nbsp;diff&nbsp;HEAD |查看working tree和commit的差别的。（你一定没有忘记，HEAD代表的是最近的一次commit的信息）

git reset 命令 | 含义
:-------------- | :------------
git reset --soft |只撤销commit，保留working tree和index file。
<span style="white-space:nowrap">git reset --mixed</span> |撤销commit和index file，保留working tree
git reset --hard |撤销commit、index file和working tree，即撤销销毁最近一次的commit
git reset |和git reset --mixed完全一样
git reset -- |用于删除登记在index file里的某个文件。例如：git reset -- roc.c

git revert 命令 | 含义 
:-------------- | :------------
git revert |用于回滚（撤销）一些commit。对于一个或者多个已经存在的commit，去除由这些commit引入的改变，并且用一个新的commit来记录这个回滚操作。这个命令要求working tree必须是干净的。 
git revert HEAD~3 |丢弃最近的三个commit，把状态恢复到最近的第四个commit，并且提交一个新的commit来记录这次改变。 
<span style="white-space:nowrap">git revert -n master~5..master~2</span> |丢弃从最近的第五个commit（包含）到第二个（不包含）,但是不自动生成commit，这个revert仅仅修改working tree和index。 

 其它命令 | 含义
:-------------- | :------------
git rebase |
git submodule | （1、2）

## 应用实例

###git中修改commit的message的方法
- git rebase -i master~5 
- 这个是找出master分支最近5次的commit，看见那个写错的了吧？把pick改成reword

###丢弃你所有的本地改动与提交
- 到服务器上获取最新的版本并将你本地主分支指向到它
  1. git fetch origin
  2. git reset --hard origin/master
 
##参考文献
[A Visual Git Reference](http://marklodato.github.com/visual-git-guide/index-en.html)   
[git - 简易指南](http://rogerdudler.GitHub.com/git-guide/index.zh.html)  
[Linux大棚的Git教程](http://roclinux.cn/?tag=git)       
[GIT基本概念和用法总结](http://guibin.iteye.com/blog/1014369)  
[Git分支管理策略](http://www.ruanyifeng.com/blog/2012/07/git.html)   
[超级有用的git reset --hard和git revert命令](http://blog.sina.com.cn/s/blog_68af3f090100rp5r.html)   

[id]: http://example.com/  "Title"
[id2]: http://i3.sinaimg.cn/IT/2012/0627/U7989P2DT20120627145618.jpg "picture"