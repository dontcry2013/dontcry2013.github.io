---
title: Git操作命令
date: 2016-03-01 14:40:13
categories: [Tools, Git] #文章文类
tags: [Git] #文章标签，多于一项时用这种格式
---

初始化工作仓库的两种方式：目录中初始化和clone现有仓库。

### 在现有目录中初始化仓库

``` bash
$ git init
$ git add *.c
$ git add LICENSE
$ git commit -m 'initial project version'
```
<!-- more -->

### 克隆现有的仓库

``` bash
$ git clone https://github.com/libgit2/libgit2
```

### 基本操作

``` bash
$ git help <command>  # display command's help
$ git status  #check repository status
$ git status --short  #briefly
$ git status -s  # same to --short
$ git add README  #track new file
$ git rm log/\*.log  # remove from working directory,not filesys
$ git mv file_from file_to  #rename
$ git log --oneline --graph  #check commit log
$ git log --stat  #commit log lately

$//30 OCT 2016增加
$ git reset --hard HEAD  //检出最新版本
$ git checkout hello.js  //检出版本库中指定文件

### 加入到暂存区域等待提交：
$ git add README test.rb LICENSE
$ git commit -m 'The initial commit of my project'
$ git commit -a -m 'made a change'  #跳过暂存直接提交
```

### 远程仓库的使用

``` bash
$ git remote #查看远程仓库
$ git remote -v #查看远程仓库url
$ git remote add pb https://github.com/paulboone/ticgit #添加远程仓库
$ git fetch [remote-name] #
$ git push origin master #推送到远程仓库
$ git remote show origin #远程仓库更多信息
```

### 分支相关命令

``` bash
$ git branch #分支列表
$ git branch -v #最后提交信息
$ git branch testing #新建分支
$ git checkout testing #切换分支
```
切换分支的时候，Git 会重置你的工作目录，使其看起来像回到了你在那个分支上最后一次提交的样子。 Git 会自动添加、删除、修改文件以确保此时你的工作目录和这个分支最后一次提交时的样子一模一样。
所有这些工作，你需要的命令只有 branch、checkout 和 commit。

``` bash
$ git log --oneline --decorate --graph --all
* c2b9e (HEAD, master) made other changes
| * 87ab2 (testing) made a change
|/
* f30ab add feature #32 - ability to add new formats to the
* 34ac2 fixed bug #1328 - stack overflow under certain conditions
* 98ca9 initial commit of my project
```

查看分支分叉历史

``` bash
$ git checkout -b iss53 
```

新建并切换的简写

``` bash
$ git checkout master
$ git merge hotfix
Updating f42c576..3a0874c
Fast-forward
 index.html | 2 ++
 1 file changed, 2 insertions(+)

$ git branch -d hotfix
Deleted branch hotfix (3a0874c).
```

将hotfix分支合并回master，此时master和hotfix指向了同一个位置，已经不需要hotfix分支，故将其删除。


More info: [Git command](http://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%AE%80%E4%BB%8B)

### 冲突的解决

More info: [Git solve conflict](http://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6)