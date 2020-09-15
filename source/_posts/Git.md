---
title: Git
date: 2020-04-28 09:15:36
top: true
img: https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/scubatocat.png
tags: 
    - 版本控制
    - git
categories: 工具
summary: Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.
---

## Git

Git是目前世界上最先进的分布式版本控制系统，在处理各种项目时都十分高效，而且非常的高大上。

SVN是集中式版本控制系统，版本库是集中放在中央服务器的，而干活的时候，用的都是自己的电脑，所以首先要从中央服务器哪里得到最新的版本，然后干活，干完后，需要把自己做完的活推送到中央服务器。而且集中式版本控制系统是必须联网才能工作。

Git是分布式版本控制系统，它就没有中央服务器的，每个人的电脑就是一个完整的版本库，这样，工作的时候就不需要联网了，因为版本都是在自己的电脑上。

Git关心的是：文件整体是否发生变化，而SVN关心的是：文件内容的具体差异。

SVN每次提交记录的是：哪些文件进行了修改，以及修改了哪些行的哪些内容

## 账户

### 配置账户

```bash
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

如果用了 --global 选项，那么更改的配置文件就是位于你用户主目录下的那个，以后你所有的项目都会默认使用这里配置的用户信息。

如果要在某个特定的项目中使用其他名字或者电邮，只要去掉 --global 选项重新配置即可，新的设定保存在当前项目的 .git/config 文件里。

### 查看账户

```bash
$ git config user.name
$ git config user.email
```

### SSH

```bash
 ssh-keygen -t rsa -C "email@example.com"
```

- -t = 密钥的类型(The type of the key to generate) ，这里参数值是 rsa

- -C = 用于识别这个密钥的注释 ，可以是任何内容(comment to identify the key)

id_rsa（私钥）和id_rsa.pub（公钥）默认路径为：/c/Users/iskyl/.ssh/

将id_rsa.pub内容同步到github的SSH keys中(github中可以新建多个SSH keys)

#### 查看公钥链接

```bash
ssh -T git@github.com
```

注意“T”大写,连接成功，会生成known_hosts文件(.ssh文件夹下)，并且将github.com连接的ip，通信公钥等内容加入到文件中。作为信任列表。

![](SSH.png)

### 查看配置

检查自己的配置信息

```bash
$ git config --list
```

查看git版本

```bash
$ git --version
```

注意"-"

## 概念

### 四个组成部分

![](GitStructure.png)

- Working Directory:工作区，即本地电脑能看到的目录

- Stage(Index):暂存区，一般放在”.git目录下的“文件，暂存区有时也叫做索引（Index）

- Repository:本地历史仓库
- Remote:远程仓库,github或者gitee

工作区有一个隐藏目录.git,这是git的版本库不算工作区

### 文件状态

依据于是否已经加入版本控制

```bash
$ git status 
```

- Tracked:已跟踪
- Untracked:未跟踪

例

1. 新建一个文件则处于Untracked状态
2. git add进入缓存文件处于Tracked状态并且是Staged暂存状态
3. git commit暂存区到Repository本地仓库文件处于Unmodified未修改状态
4. 再次编写此文件则文件又变成Modified修改状态

### Commit

每次commit时仓库的数据结构分为四个对象

- blob对象：存放文件数据； 
- tree对象：目录，内容为blob对象的指针或其他tree对象的指针
-  commit对象：快照，包含指向前一次提交对象的指针，commit相关的信 
  通过索引找到文件快照。
-  tag对象：一种特殊的commit对象，一般对某次重要的commit加TAG，以示重要(方便找)

## 基本命令

- 初始化仓库：`git init`
- 把文件添加到仓库：`git add a.txt`添加到暂存区（state）
- 把文件提交到仓库：`git commit -m '注释信息'`
- 仓库状态：`git status`
- 查看修改内容：`git diff`
- 显示最近到最有远的提交日志：`git log` （`git log --pretty=oneline`）
- 克隆到本地：`git clone`
- 添加到缓存：`git add`  

## 版本回退

- `git reset --hard HEAD^` 回退到上一版本
- `git reset --hard HEAD^` 回退到上上版本
- `git reset --hard HEAD~100` 回退到上100个版本
- `git reset --hard 具体版本号` 回退到具体版本号

记录每一次命令 ：`git reflog`

`git checkout -- readme.txt`：

命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：

一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。

## 删除

删除文件后，Git知道你删除了文件，因此，工作区和版本库就不一致了，`git status`命令会立刻告诉你哪些文件被删除了：

现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令`git rm filename` 删掉，并且`git commit`

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本： `git checkout -- filename`

## 远程仓库

- `git remote add origin 远程仓库地址` ：关联远程仓库
- `git remote rm origin` ：删除关联
- `git push origin master`: 推送 （第一次 加上-u 就会一直关联这个地址，就不需要再写origin master）
- `git pull origin master --allow-unrelated-histories`：如果本地仓库和远程库有冲突，比如GitHub上有markdown文件，则加上 --allow-unrelatered..
- `git clone git@github.com:rottengeek/test.git`：克隆远程库到本地

## 分支

- `git branch 分支名`：创建分支
- `git checkout 分支名`：切换分支
- `git checkout -b 分支名` ：创建与切换同时进行
- `git branch`：列出所有分支
- `git merge dev`：把dev分支的工作成果合并到master分支上
- `git branch -d 分支名` : 删除分支