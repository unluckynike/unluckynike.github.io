---
title: Hexo-backup备份恢复
date: 2020-10-19 16:17:09
tags:
    - Hexo
categories: Hexo
img: https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/pictures/snow-5369752_1920.jpg
summary: 就在昨天，博主的电脑莫名其妙就崩了，蓝屏，开不了机。通过某技术手段（U盘）打开发现，里面也没有，连C盘都直接空了😱😱😱😱。
---

> 记录一次电脑重置Hexo还原恢复

## 前言

就在昨天，博主的电脑莫名其妙就崩了，蓝屏，开不了机。通过某技术手段（U盘）打开发现，里面也没有，连C盘都直接空了😱😱😱😱。这可真是神奇，啥也没给我留下。还好hexo之前用backup利用branch做了备份，要不然可真是😨😨😨😨😨“连个毛都不剩了”。（嗯！[未雨绸缪](https://baike.baidu.com/item/%E6%9C%AA%E9%9B%A8%E7%BB%B8%E7%BC%AA/839102)总是好事😝😝😝😝）。

<div>
<img 
     src="https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/wulinzengblog/doyouhavemao.gif">
</div>



<div>
<img 
     src="https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/wulinzengblog/computersystemcrash.png">
</div>

## 恢复

### 准备工作

之前已经用hexo分支备份，我们首先需要电脑上下载git，node.js，配好ssh.

重置电脑删除了配置文件的环境变量，需要重新设置name和email。

```bash
git config --global user.name="你的名字"
git config --global user.email="你的邮箱"
```

私钥，生产后在github配置SSH,同样在`.ssh`文件下，将公钥粘到`key`l里面。

```bash
ssh-keygen -t rsa -C "你的邮箱"
```

- -t 指定密钥类型，默认是rsa
- -C 设置注释文字，比如用户名

这里输入后弹出的信息，连按回车就可以。

测试是否连通，这里看自己个人有哪些部署，GitHub，Coding，Gitee

```bash
ssh -T git@github.com
ssh -T git@git.coding.net
ssh -T git@gitee.com
```

### 克隆到本地

首先找一个空的文件夹将之前托管的项目拉下来，这里使用的GitHub Desktop工具(推荐)，也可以直接`clone`

```bash
git clone git@github.com:xxxxx/xxxxx.github.io.git
git clone https://github.com/xxxxx/xxxxx.github.io.git
```

<div>
<img 
  src="https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/wulinzengblog/20201019170706.png">
</div>

拉取下来后切换到分支`backup`,因为这里才是我们的源文件，`master`我已经渲染生成好的文件。

<div>
<img 
  src="https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/wulinzengblog/20201019170707.png">
</div>

### 恢复

在克隆的那个文件夹下输入如下命令恢复博客，这里我们不再需要`hexo init`了，因为我们不再是从零开始搭建的，要进入有`public`、`source`、`themes`文件的目录下执行。

```bash
$ npm install hexo-cli
$ npm install
$ npm install hexo-deployer-git
```

<div>
<img 
  src="https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/wulinzengblog/20201019170708.png">
</div>

完成之后直接三连本地访问部署😆😆😆😆😆。

## 踩坑

#### 指令无效

```bash
$ hexo s 
$ bash: hexo:command not foud
```

这里我们将hexo的文件写入系统环境变量即可解决，找到`node_modules`目录。

<div>
<img 
  src="https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/wulinzengblog/20201019170709.png">
    <img 
  src="https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/wulinzengblog/20201019170710.png">
</div>

完成之后当前终端可能依然会出现指令找不见的现象，另开一个终端执行hexo即可（或者重启）。

### Coding推送失败

如图（中间部分Error），这里只是GitHub推送上去了，Coding推送失败，检查自己的Coding SSH配置，重新部署。

<div>
<img 
  src="https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/wulinzengblog/20201019170711.png">
</div>

