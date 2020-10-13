---
title: 部署Coding SSL/TLS证书
date: 2020-10-13 10:34:13
tags:
    - Hexo
categories: Hexo
img: 
---

## 问题

站点Coding Pages SSL/TLS证书到期，访问为不安全链接。

![Firefox访问为不安全链接](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/wulinzengblog/PageIsNotSecurity.png)

## SSL与TLS

SSL：（Secure Socket Layer，安全套接字层），位于可靠的面向连接的网络层协议和应用层协议之间的一种协议层。SSL通过互相认证、使用数字签名确保完整性、使用加密确保私密性，以实现客户端和服务器之间的安全通讯。该协议由两层组成：SSL记录协议和SSL握手协议。

TLS：(Transport Layer Security，传输层安全协议)，用于两个应用程序之间提供保密性和数据完整性。该协议由两层组成：TLS记录协议和TLS握手协议。

## 步骤

进入coding找到部署的项目（不得不说新改版的看着有点别扭），进入到**持续部署**的**静态网站**，这里不用新建，因为我们之前已经部署过了，点击**旧版网站列表**进入到以前部署的项目。进入后直接到**设置**下拉到自定义域名，点击**申请证书**（注意这里需要DNS停掉GitHub Pages 的解析）。直至证书状态变为**正常**即可。

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/wulinzengblog/CodingPages.png)

绑定域名成功后，系统将会自动为其申请 SSL/TLS 安全证书。（ SSL/TLS 安全证书到期前一周，系统将自动续签。）

启用强制 HTTPS 访问后，任何尝试用 HTTP 协议访问你网站的请求都会被强制跳转到使用 HTTPS 加密协议访问，为保护数据安全。

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/wulinzengblog/CodingPagesSSL.png)

## 完成

不安全链接已经消失，直接转到主站。

![FireFox访问安全](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/wulinzengblog/PageIsSecurity.png)

## 错误类型

```
acme:error:unauthorized: During secondary validation: Invalid response from http://xxxxxxx/.well-known/acme-challenge/u0F-eNkNn958JbxhnH0lyhGxS6d_FMLHWmIiiwF8P5k [185.199.108.153]: "<!DOCTYPE html>\n<html>\n <head>\n <meta http-equiv=\"Content-type\" content=\"text/html; charset=utf-8\">\n <meta http-equiv=\"Co"
```

**原因**

这种错误一般是 hexo 博客双线部署到 GitHub Pages 和 Coding Pages 过程中出现的，并且已经在域名 DNS 配置好了 GitHub 的域名解析，这种情况下，在验证域名所有权时会定位到 Github Pages 的主机上导致 SSL 证书申请失败。

**解决**

DNS停掉GitHub的解析再重新生成证书，申请成功后再开启解析。

![腾讯云DNS解析DNSPod](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/wulinzengblog/TencentCodingGitBubDNS.png)

