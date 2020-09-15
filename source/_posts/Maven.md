---
title: Maven
date: 2020-06-30 08:15:29
cover: true
img: https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/monk-5357402_1920.jpg
summary: Maven项目对象模型(POM)，可以通过一小段描述信息来管理项目的构建，报告和文档的软件项目管理工具
tags:
   - 项目管理 
   - Maven
   - java
categories: 工具
---

## Maven

Maven项目对象模型(POM)，可以通过一小段描述信息来管理项目的构建，报告和文档的软件项目管理工具。

## 安装

下载地址：http://maven.apache.org/download.cgi

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/MavenDownload.png)

下载完，直接解压，copy到一个无中文的目录，并且创建一个新的maven-repository目录。一下便是解压后的Maven目录结构。

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/Maven.png)

## 配置

1.首先确保电脑JDK已经安装好，进入环境变量的系统变量新建变量名`MAVEN_HOME`，值为刚才解压的Maven文件的路径。

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/MAVEN_HOME.png)

2.确认后再进入到系统变量的`Path`新建`%MAVEN_HOME%\bin`

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/MAVEN_Path.png)

3.在cmd窗口输入`mvn -v`显示出版本信息则说明已经配置成功。

## 修改配置文件

**修改localRepository**

进入`conf\settings.xml`文件在<localRepository>标签内修改它的默认存储路径。

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/MavenLocalRepository.png)

**阿里云镜像**

添加<mirrors>标签下<mirror>，添加国内镜像源，这样下载jar包速度很快。默认的中央仓库有时候甚至连接不通。一般使用阿里云镜像库即可。这里我就都加上了，Maven会默认从这几个开始下载，没有的话就会去中央仓库了。

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/MavenMirror.png)

```xml
     <mirror>     
       <id>alimaven</id>     
       <name>aliyun maven</name>    
       <url>http://maven.aliyun.com/nexus/content/groups/public/</url>    
       <mirrorOf>central</mirrorOf>        
    </mirror>
```



## Intillij IDEA

在settings种配置Maven

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/MavenIDEA.png)

## 创建

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/MavenProject.png)

## 优势

- Maven的库是由开源组织维护，不需要我们再花精力去管第三方库，即使自己维护，也比较方便。
- Maven对jar包的版本管理有工具上的支持，比如将Release版本和Snapshot版本区分开，有利于SCM管理。
- Maven的库下载是即用即下，不需要实现全部down下来。Maven的插件也是自动升级，可以方便的。

- 可以很方便的与eclipse, IDEA这样的主流的IDE集成。

- 仓库管理器：它的出现有两个目的：首先它的角色是一个高度可配置的介于你的组织与公开Maven仓库之间的代理，其次它为你的组织提供了一个可部署你组织内部生成的构件(第二方库)的地方。

## 仓库

- 本地仓库--存在本地的仓库 （使用）

- 中央仓库 --存在远程网上（使用）

- 镜像 -- 在国内搭建的服务器 比较出名阿里云镜像（使用）

- 私服 -- 公司自己搭建一个容器服务(私服) 所有的jar包放入到私服上面 ，公司内部人员下载jar包 就使用私服