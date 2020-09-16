---
title: Eclipse便捷操作
date: 2020-09-14 17:08:15
img: https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/pictures/fantasy-4566021_1920.jpg
tags: 
    - java
categories: 工具
summary: Eclipse最初是由IBM公司开发的替代商业软件Visual Age for Java的下一代IDE开发环境，2001年11月贡献给开源社区，现在它由非营利软件供应商联盟Eclipse基金会（Eclipse Foundation）管理。
---

> Eclipse 是一个开放源代码的、基于Java的可扩展开发平台。就其本身而言，它只是一个框架和一组服务，用于通过插件组件构建开发环境。幸运的是，Eclipse 附带了一个标准的插件集，包括Java开发工具（Java Development Kit，JDK）。

🚪 传送门：[Eclipse下载](https://www.eclipse.org/)

## 快捷键

`ctrl` + `2`  快捷创建变量

`alt`  +  `/`  提示补全

`ctrl` + `/`  注释当前行

`ctrl` + `f`  查找 批量替换

`ctrl` + `d`  删除当前行

`ctrl` + `shift` + `f`  快速排版

## 自动补全

依次进入`Window` -> `preferences` -> `java` -> `Editor` -> `Content Assist`

在右侧的Auto Activation子菜单里找到Auto activation triggers for Java,可以看到现在是一个`.`。将其替换为`.abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ`。同时Auto activation delay(ms) 设置为 `0`。

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/wulinzengblog/eclipse0.png)

## Multiple annotations found at this line

解决Eclipse中XML最上边报错Multiple annotations found at this line:

```xml
- Referenced file contains errors (http://www.springframework.org/schema/aop/spring-aop-4.1.xsd). For more information, right click on the message in the Problems View and select "Show xml
 Details..."
- Referenced file contains errors (http://www.springframework.org/schema/context/spring-context.xsd). For more information, right click on the message in the Problems View and select "Show 
 Details...
```

依次进入`Window` -> `preferences` -> `XML` -> `XML Files` -> `Validation` 取消勾选:`Honour all XML schema locations`,这告诉eclipse,不再验证不同schema位置的相同命名空间的引用，仅以第一次找到的可验证的XML文件为结果。Apple若有弹窗选择是即可。

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/wulinzengblog/eclipse1.png)

## 注释作者日期

依次进入 `Window` -> `preferences`  -> `Java`  -> `CodeStyle`  -> `Code Templates` -> `types` 设置模板 user是电脑账号名，也可自定义，时间格式`${date}`固定不可改变。

```
/**
 * @author ${user}
 * @date ${date}
 * ${tags}
 */
```

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/wulinzengblog/eclipse2.png)

## 代码统计

依次进入 `Search` -> `file` Containing tex 输入 `\n`，勾选 Regular expression ,File name patterns 输入`*java`,当然也可以输入`*css,*html`（ 点击choose切换 ）等文件后缀，这里匹配的是你所要统计的文件。点击 `Search` 后，下方便有代码总数了。这是统计的工作区的所有项目，当然也可统计单个项目。

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/wulinzengblog/eclipse3.png)