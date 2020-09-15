---
title: 应用层
date: 2020-05-12 22:58:09
img: https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/twitter-292994_1920.jpg
tags: 
   - 计算机网络
categories: 计算机网络
summary: 位于计算机网络体系结构的最上层，前面四层做的所有事情就是为了他服务，他也是设计和建立计算机网络的最终目的，通俗的讲，就是我们开发的应用软件，就处于这一层，
---

## 概述

应用层的目的是要为人们提供具体的网络应用（如网页浏览，文件下载，电子邮件等）。通信两端的应用层遵循特定的应用层协议，交换特定的应用层报文，实现特定的网络应用。

在实际的网络通信中，应用层报文的传输需要使用下面的运输层、网络层、数据链路层、物理层的服务，要被层层向下传递、封装，经过网络到达目的地后再层层向上传递、去封装，最终到达目的地的应用层。

网络应用程序通过交换应用层报文实现应用的功能

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/应用层报文.png)

应用层实体实现具体的应用层协议，提供网络应用。网络通信是在两个正在运行着的程序之间进行的，应用层实体是实现并遵循特定网络协议的应用进程。两个进程通过交换应用层报文，为用户提供特定的网络应用。

## 应用层工作模型

**应用进程**

应用层之间的通信是在两个正在运行着的程序之间进行的。正在运行的程序称为进程，应用进程是应用层的通信主体，实现了网络应用。

**应用层协议**

网络应用需要通过应用层协议来实现，应用进程内包含了应用层协议的实现代码，应用进程按照协议的要求解读和生成应用报文，按照协议的要求进行报文交换，实现网络应用。

应用层的工作模型主要有三种：客户-服务器模型、P2P模型和混合模型。客户-服务器模型是最主要、最基本的模型。

---

**1.客户-服务器模型**

服务器要先运行并处于等待状态，时刻准备好接收客户的请求。可会只在需要时向服务器发出请求，服务器收到请求后给客户做出应答，然后客户再次发出请求，服务器再次给与应答。目前客户-服务器模型是网络中多数应用的工作模型（如WWW，FTP，DNS等）。

**服务器如何处理多个客户端请求的？**

1. **重复型服务程序** 在处理完成一个客户请求之前，不会为其他客户提供服务，重复型服务器包含一个请求队列，客户请求到达后，首先进入队列中，服务器按先进先出的原则对请求做出逐一响应。
2. **并发型服务器**，在启动一个主机服务器，主服务器任务实时等待客户请求，一旦客户请求到达，主服务器立即产生一个子进程（又称重服务器），由从服务器来响应客户请求，主服务器再次等待客户端请求状态，如果还有客户请求，主服务器再次产生一个从服务器来响应新的客户请求，就是说每个客户都有自己的服务器。

-------

**2.P2P模型**

在P2P（Peer TO Peer）模型中，网络中没有一个中心服务器，网络中的每个结点是对等的。网络中的结点能够自动发现别的节点，每个结点既是客户端，也是服务器（如Gnutella）。

----

**3.混合模型**

许多应用采用的客户-服务器模型和P2P模型的混合方式。从服务器获取必要信息，客户端之间P2P方式通信（如QQ，Skype）.

----

## 应用进程的地址

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/应用进程寻址.png)

在TCP/IP体系中，应用进程的地址是通过<IP地址，运输端口号>来确定的，一个IP地址可以标识网络中的一台主机，为了区分一台主机内的不同的通信进程，每个通信进程会对应一个端口号，端口号只在本地主机有效，不具有全局性。<IP地址1，端口号1>------<IP地址2，端口号2>能确定通信的两个应用进程。

对于一些常用的服务（如web服务、FTP服务），其服务器进程对应的端口号是固定的，这些端口称为**熟知端口（well-know port）**其值的范围一般为0~1023，而客户进程的端口号是由操作系统随机分配的它的值一般大于1024.

查看端口号的命令（windows）:

```bash
netstat -an
```

**常用的服务及其默认端口号**

|  服务  | 运输层协议 | 默认端口号 |
| :----: | :--------: | :--------: |
|  Web   |    TCP     |     80     |
|  FTP   |    TCP     |     21     |
|  DNS   |    UDP     |     53     |
|  SMTP  |    TCP     |     25     |
|  POP3  |    TCP     |    110     |
| Telnet |    TCP     |     23     |

## URL

统一资源定位符（Unified Resource Location,URL）表示服务器上可以访问的资源。URL是对Internet上资源的位置和访问方法的一种表示形式，其中资源可以是多种类型（文件，目录，图像，声音等）

**URL的表示形式：<访问方式>：//<主机>：<端口>/<路径>**

访问方式是指所采用的应用层协议，主机给出服务器的主机地址，端口给出服务器内服务进程的端口号，路径给出要访问的具体资源。

**例**

- http://220.108.9.38:80/music/song.html
- ftp://ftp.cdut.edu.cn/pub/

## DNS域名系统

Internet中标识一台主机的两种方式

- 域名（Domain Name）:www.baidu.com
- IP:220.181.111.188

**域名和IP地址存在对应关系，一个域名对应一个或多个IP地址**

域名容易书写，记忆，人们喜欢使用域名,但网络通信最终还是要使用IP地址。向具有域名的主机发送数据包时，需要先得到其域名对应的IP地址，域名系统（Domain Name System,DNS）提供了域名与IP之间的映射服务。

域名系统（DNS）是用分布式数据库来管理全球域名的系统，将计算机名字解析成IP地址，主机通过DNS转换成主机或路由器能够识别的IP地址。

**域名系统的特点**

允许区域自治在域名系统的设计过程中运行每个域的管理单位设计和定义本域下的子域、子域名、主机名，不必通知上级管理结构，由本域的DNS服务器进行管理和解析。

**层次域名空间**

域名采用分层次的命名方式

**.......三级域名.二级域名.顶级域名**   例（www.baidu.com）

三类顶级域名

- 地理顶级域名：`.CN(中国`，` .UK(英国)`，`.JP(日本)`
- 类别顶级域名: `.COM(公司)`，`.NET(网络机构)`，`.ORG(组织机构)`
- 新增顶级域名：`.BIZ(商业)`，`.COOP(合作公司)`，`.NAME(个人)`

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/域名.png)

## 域名服务器

网络通信不能通过域名来进行，必须将域名转换为对应的IP地址。将域名转换为对应IP地址的过程称为域名解析，域名解析由域名服务器来完成，域名服务器的默认端口号为53，使用UDP协议作为运输层协议。一台服务器无法提供Internet上所有的域名和IP地址的映射，Internet上的域名服务器采用层次化分布式的模型，构成一个分布式的数据库系统，共同提供域名解析服务，这个系统称为域名系统。

域名系统的三个层级：根域名服务器、顶级域名服务器、授权域名服务器。（上一级维护下一级）

- 根域名服务器维护顶级域名服务器（.com，.cn)的地址信息，接收顶级域名服务器和本地域名服务器的查询请求。
- 顶级域名服务器（.com，.cn）维护下级域名服务器（.edu.cn，.163.cn）的信息，接受下级域名服务器的查询请求。
- 授权域名服务器负责解析某个区域内所有的域名（baid.com授权域名服务器负责解析所有以baidu.com结尾的域名如www.baidu.com，music.baidu.com等 ）

全球有十三台根逻辑域名服务器。名字分别为“A”~“M”

**本地域名服务器**

本地域名服务器是用户访问域名系统的接入点，将用户的DNS请求转发到DNS系统中，将DNS应答发送给客户。

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/本地域名服务器.png)

如何查看本地域名服务器(windows)

```bash
ipconfig/all
```

## 域名解析过程

**迭代方式与递归方式**

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/迭代方式.png)



整过程中，本地域名服务器对于用户而言是服务器，而对于其他的域名服务器来说，则是客户。因此，本地域名服务器有着双重的身份，每收到进一步的信息后，本地域名服务器就会向新的服务器发出查询请求，直到最终得到结果，返回给用户。

---



![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/递归方式.png)在这种方式中，主机向本地的域名服务器发出DNS请求，如果在本地域名服务中有其IP地址，就做出正确的回答，如果服务器不知道该域名的IP地址，就以DNS客户的方式向其他服务器发出请求，其他域名服务器要么知道该域名的IP地址给出正确回答，要么再以客户身份发出DNS请求，重复(递归).上述过程，直到找到正确的服务器为止，将域名对应的IP地址再反向回传给开始发出DNS请求的主机。请求一级一级地传递，结果一级一级地返回。根域名服务器要负责返回结果，因此负担比较重。实践中，域名查询常采取递归方式。

## DNS缓存

为了减少网络上DNS报文的数量和查询所花费的时间，DNS系统广泛使用缓存技术。在用户端操作系统通常会缓存曾经查询过的DNS域名和其对应的IP。在DNS服务器端，每当收到来自其他服务器的DNS应答时，DNS服务器就会将其缓存起来。

**客户端缓存**

在用户端操作系统通常会缓存曾经查询过的DNS域名和其对应的IP地址，当下一次要访问同一个域名时，查询缓存就可以得到结果，不必再向本地域名服务器发出请求了。

在Windows XP系统下

查看DNS缓存

```bash
ipconfig/displaydns
```

清空DNS缓存

```bash
ipconfig/flushdns
```

**服务器缓存**

在DNS服务器端，每当收到来自其他服务器的DNS应答时，DNS服务器就会将其缓存起来，当有同样的请求到来时，本地域名服务器就能直接给出应答了。但是，因为主机名和IP地址的映射关系可能会发生变化，因此DNS缓存通常会在一段时间内（通常为两天）被丢弃，这样能保证适应情况的变化而不至于做出错误的域名解析。

## DNS报文

DNS报文分为DNS**请求报文**和DNS**应答报文**

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/DNS请求报文.png)![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/DNS应答报文.png)

**常用命令**

查看本地域名服务器

```bash
ipconfig/all
```

查询域名与IP地址的对应关系

 正向解析

```bash
nslookup www.163.com
```

逆向解析

```bash
nslookup -qt=ptr 192.168.1.1
```

## WWW

万维网(World Wide Web，WWW)是最广泛的网络应用，是一个大规模的、联机的信息存藏所，是Time Berners-Lee于20世纪90年代初在欧洲粒子物理实验室发明的。WWW用链接的方法能非常方便地从因特网上的一个站点访问另一个站点，从而主动地按需获取丰富的信息。

WWW的主要功能组件：HTML语言，HTTP协议，Web浏览器和Web服务器。

**HTML语言**

称为超文本标记语言（Hyper Text Markup Language ,HTML),写出来的文档称为超文本文档，俗称网页。

**Web浏览器和Web服务器**

Web服务器是运行在服务端应用的应用进程，默认的端口号是80，它接收Web浏览器的请求，向其传输网页和其他文件。

Web浏览器是运行在客户端的应用进程，用于访问Web服务器上的HTML文档，接收服务器传来的HTML文档并显示其内容。

**HTTP协议**

Web浏览器和Web服务器程序在进行数据交换，发送请求和应答时，要遵循一定的协议，这个协议就是超文本传输协议（Hyper Text Transfer Protocol, HTTP）。

## HTTP协议

HTTP（Hyper Text Transfer Protocol）是Web浏览器和Web服务器交互时要遵循的协议，最新版本是HTTP1.1(RFC2616)。HTTP遵循客户-服务器模式，服务器默认端口号为80。HTTP协议需要传输TCP协议支持，进行可靠数据传输。

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/HTTP工作过程.png)

1. 浏览器分析超链接指向页面URL
2. 浏览器向DNS请求解析URL的IP地址
3. 域名系统DNS解析出URL服务器的IP地址
4. 浏览器与服务器建立TCP连接
5. 浏览器发出取文件的命令
6. 服务器给出响应
7. TCP连接释放
8. 浏览器显示文件中的所有文本

**HTTP的无状态特性**

Web服务器不去记忆客户端的访问记录，同一个客户端向Web服务器发出两个相同的请求，Web服务器会认为这是两个单独的请求，都会给予应答。

**HTTP的持久连接和非持久连接**

非持久连接：一个TCP连接只传送一个文件，文件传输完毕，连接关闭。（早期HTTP Sever采用）

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/非持久连接.png)

持久连接：一个TCP连接可以传送多个文件。（目前采用）

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/持久连接.png)

**解释HTTP协议中GET,POST,HEAD,PUT和DELETE的含有**

- GET

当浏览器请求获取某个文档时，方法字段的值就使用GET，文档的地址由URL字段给出。当使用GET方法时，请求报文的实体主体部分为空。

- POST

在多数情况下，浏览器是从服务器上获取数据的。单在某些情况下，浏览器也会向服务器提交网页上表单内的一些数据（如用户信息），这时方法字段的值设定为POST,实体主体中要放入提交的内容。

- HEAD

HEAD字段与GET字段很类似。如果请求报文中方法字段的值为HEAD，那么服务也会给浏览器发送应答报文，这点同GET一样。不同的是，对GET的应答报文中会包含浏览器所请求的文档；而对HEAD的应答报文中不包含文档的内容。HEAD字段常用来进行测试和故障跟踪，用来判断某一个连接是否有效，能否被访问。

- PUT

PUT用来将一个文档上传到Web服务器上，文档的名字和位置由URL字段指明。文档的内容存储在实体字段里。如果文档已经存在于服务器上，则服务器会覆盖旧的文档；如果文档不存在，则服务器会根据URL建立一个新文档，将实体字段的内容存入新文档。如果执行成功，则服务器会返回对应的应答报文；如果执行过程中出错（如没有写权限），则服务器会给浏览器报错。

- DELETE

DELETE用来删除Web服务器上的文档，文档的名字和位置由URL字段指定。请求行后面是首部行，可以有多个首部行，每个首部行都有特定的含有，用来告诉服务器一些特定的信息。常用的首部有：Accept，Accept-Language，Accept-Encoding，User-Agent，Host和Connection。

## HTTP报文

Web浏览器和Web服务通过交换HTTP报文来实现HTTP协议，HTTP协议由两种报文，请求报文和应答报文，请求报文是浏览器发送给服务器的，指明所需文档的名字和位置。应答报文是服务器发送给浏览器的，里面包含服务器的应答和浏览器所需的文档。

**请求报文**

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/请求报文结构.png)

---

方法字段：

- GET：请求获取某个文档
- POST：向服务器提交网上表单的数据
- HEAD：类似GET，但其应答报文中不包含文档的具体内容
- PUT：将文档上传到Web服务器上
- DELETE：删除Web服务器上的文档
- URL字段：指明浏览器需要文档位置与名称

版本：浏览器目前使用的是HTTP协议的版本，一般为HTTP1.1。

首部字段：

- Accept：表示浏览器所接受的文档类型
- Accept-Language：表示浏览器优先接受的语言类型
- Accept-Encoding：表示浏览器能够理解的编码方式
- User-Agent：告诉服务器浏览器的类型
- Host：表示所访问的主机
- Connection：告诉服务器在对浏览器做了应答后，是否继续保持和浏览器的连接

实体：存放浏览器向服务器发送的数据。对于HTTP请求报文实体部分多数情况下为空。

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/请求报文例子.png)

---

**应答报文**

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/应答报文结构.png)

---

应答报文的开始行是状态行，状态行包括三项内容，即HTTP的版本，状态码，以及解释状态码的简单短语。

- 版本：Web服务器使用HTTP协议版本。

HTTP协议使用一些特定的三位数字表明对请求应答结果称为状态码，常用状态码有200（OK），301（Moved Permanently），300（Bad Request），403(Forbidden)，404（Not Found），500（Internal Server Error）

- 短语：对状态码的文字解释

首部字段：

- Connection：服务器告诉浏览器，发送完文当后是关闭还是保持连接
- Data：是服务器产生响应报文的时间
- Server：表明服务器的类型
- Last-Modified：文档的最后修改时间
- Content-Length：发送文档的字节数
- Contend-Type：文档的格式

实体：存放服务器向浏览器返回的文件内容

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/应答报文例子.png)

---

## Web代理

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/Web代理.png)

**优点**

1. 访问速度快
2. 可节省带宽，降低开销

**缺点**

代理服务器所缓存的网页可能不是最新的

**解决**

条件GET机制

- 服务器的应答中添加Last-Modified字段
- 客户端的请求中添加If-modified-since字段

## Cookie

Cookie实际上是Web网站用来记录用户行为的一种技术，Cookie最早由Netscape公司发明。HTTP协议本身是一种无状态的协议，Web服务器不会去记录访问情况。有时候网站需要了解和记录用户访问网站的过程（如购物喜好、购物记录等）并进行一些统计。Cookie技术便用来完成这项任务。

**原理**

对于第一次访问网站的用户，服务器 的应答中添加Set-cookie，分配cookieId给用户。浏览器将用户cookieId保存在磁盘中，下次访问时，浏览器的请求报文中包含Cookie字段，告诉服务器用户的cookieId，这样服务器就能记录一些用户的访问信息。



**Cookie技术由四个部分组成**

1. 在HTTP的响应报文中有一个SetCookie的首部行
2. 在HTTP请求报文中有一个Cookie的首部行
3. 在用户端主机中保留一个Cookie文件，由用户的浏览器管理
4. 在Web站点后台有一个数据库来维护用户信息

## 多点下载和断点续传

多点下载和断点续传都是利用了HTTP1.1协议所支持的部分下载功能。HTTP请求报文的Range可以指明想要下载文档的部分内容（例如Range: bytes=0-499）,从应答报文中的Content-Length首部获取文档大小后，可以建立若干个连接，每个连接下载一部分内容，实现多点下载。

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/多点下载和断点续传.png)

## FTP协议

文件传输协议（File Transfer Protocol,FTP）是专门用来传输各类文件的协议。FTP协议的历史要比HTTP协议时间长。FTP使用客户-服务器方式（与HTTP协议一样），21端口是FTP服务器的默认服务端口使用21端口等待客户端的请求，因为要保证数据正确无误，所以FTP协议在运输层上使用了TCP协议。

FTP客户端：浏览器、FlashFTP等

FTP服务器：vsftp(Linux)、IIS(Windows)

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/FTP协议工作原理.png)

FTP协议是交互式的命令运行的。FTP客户端连接到FTP服务器后，用户输入FTP命令（如下载文件，显示服务器文件列表等），命令通过FTP客户端传递给FTP服务器，FTP服务器执行命令，并将结果返回FTP客户端，然后用户再输入下一个命令。

**FTP命令**

- USER:客户端向服务器发送用户名
- PASS:客户端向服务器发送用户口令
- LIST:列出FTP服务器当前目录下的文件和子目录
- CWD:改变当前的目录
- CDUP:回到上一级
- PWD:显示当前所在目录
- RETR:从服务器上下载文件
- STOR:将本地硬盘的文件上传到FTP服务器上
- DELE:删除FTP服务器上的文件。DELE命令的参数是服务器上要删除的文件名
- MKD:在FTP服务器上新建目录，目录名作为MKD命令的参数
- RMD:删除FTP服务器上的目录，要删除的目录名作为RMD命令的参数
- PORT:用来传递数据连接的端口号
- PASV:客户端通知服务器采用被动模式
- HELP:帮助
- QUIT:退出

**FTP应答**

对于FTP客户发出的命令，FTP服务器需要给出应答，应答包括三位整数的状态码和它后面的一个文本字符串（如226 File send OK），FTP应答通过控制连接发送。

- 150  File status okay; about to open data connection（打开数据连接）
- 200  Command okay  （命令成功）
- 220  Service ready for new user （新用户服务准备好了）
- 221  Service closing control connection Logged out if appropriate （服务关闭控制连接，可以退出登录）
- 226  Closing data connection. Requested file action successful (for example, file transfer or file abort) （关闭数据连接，请求的文件操作成功） 
- 227  Entering Passive Mode（进入被动模式）
- 230  User logged in, proceed （用户登录）
- 331  User name okay, need password （用户名正确，需要口令）
- 425  Can't open data connection （不能打开数据连接）
- 500  Syntax error, command unrecognized （格式错误，命令不可识别）

## 控制连接和数据连接

FTP会话种，客户端和服务器会使用两个连接：控制连接（21端口）和数据连接（20端口）。客户端主动连接服务器的21端口建立控制连接，用来传递用户命令和服务器响应，整个会话期间一直存在。数据连接只在传递数据时（如发送目录内容列表、上传文件、下载文件）临时建立，数据传递完毕后关闭数据连接。

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/控制连接和数据连接.png)

1. FTP客户端和服务器建立控制连接，会话开始
2. 客户端通过控制连接发送LIST命令
3. 建立数据连接
4. 发送文件、目录列表
5. 关闭数据连接
6. 客户端发出QUIT命令
7. 控制连接关闭，会话结束

**主动模式和被动模式**

传输数据前，需要建立数据连接，如果数据连接是由服务器发起的，连接客户端的，称为主动模式；如果数据连接是由客户端发起的，连接服务器的，称为被动模式。主动模式和被动模式都是向对于服务器来说的。

----

为社么会有主动模式和被动模式？

- 客户端防火墙问题
- 主动模式中，数据连接由服务器发起，可能会被客户端的防火墙拒绝；被动模式则无此问题

服务器处于主动模式还是被动模式如何确定？

由客户端发出的命令确定

- 如客户端发送Port命令（包含客户端的数据端口号），服务器会处于主动模式，从20端口发起到客户端的连接
- 如客户端发送PASV命令，服务器会处于被动模式，并通过控制连接将数据连接端口号（随机值）发送给客户端（回应状态码227），客户端发起和服务器的数据连接

---

## 电子邮件

电子邮件（Email)是Internet上广泛使用的一种应用，电子邮件是异步的，方便，快捷，还可以携带各种形式的附件。使用电子邮件有两种方式**使用客户端软件**和**使用浏览器**。

**系统构成**

- 用户代理（邮件客户端）

1. Outlook,foxmail等
2. 撰写、回复、转发、保存邮件
3. 发送、接受邮件

- 电子邮件服务器

1. 提供邮件服务器
2. 向其他邮件服务器发送邮件
3. 与邮件客户端交互

- 电子邮件协议（SMTP，POP3）

1. 发送邮件：SMTP
2. 接收邮件：POP3

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/电子邮件系统构成.png)

**电子邮件报文格式**

一个邮件报文分为首部和报文主体两部分，首部和报文主体用空行间隔开。

首部由多个首部行组成。首部有特定的格式要求，报文主体由用户自己撰写

常见首部有

- From:发送方
- To:接收方
- Cc:抄送
- Bcc:暗送
- Subject:邮件的主题
- Data:邮件发送的时间

## SMTP协议

SMTP协议是专门用来发送邮件的协议，采用客户-服务器模型，使用TCP协议作为运输层协议，SMTP服务器在25号端口侦听。SMTP客户端和服务器使用命令/应答的方式交互。

**SMTP命令**

SMTP命令由客户端发给服务器，每个命令都是一个字符串，以换行、回车符号结尾。

|  SMTP命令  |                含义                |
| :--------: | :--------------------------------: |
| HELO和EHLO |      向服务器表明客户端的身份      |
| MAIL FROM  |      向服务器表明邮件的发送者      |
|  RCPT TO   |      向服务器表明邮件的接受者      |
|    DATA    | 告诉服务器下面紧接着的是邮件的内容 |
|    REST    |     通知终止当前的邮件传输活动     |
|    VRFY    |     验证服务器上是否有某个用户     |

**SMTP应答**

SMTP应答是由SMTP服务器收到客户端的命令后给出的响应，向客户端表明命令执行的结果，应答由一个三位数字的状态码和后面的一些说明文本组成，状态码和说明文本之间要有一个空格

**常见应答**

200 < domain > Service ready

221 < domain >Service closing transmission channel

| SMTP应答 | 含义                                                         |
| -------- | ------------------------------------------------------------ |
| 200      | Service ready （服务就绪）                                   |
| 221      | Service closing transmission channel （服务关闭）            |
| 250      | Requested mail action okay, completed （要求的邮件操作完成） |
| 354      | Start mail input，end with < CRLF >.< CRLF > （开始邮件输入，以"."结束） |
| 421      | Service not available，closing transmission channel （服务未就绪，关闭传输信道） |
| 501      | Syntax error, command unrecognized （参数格式错误）          |
| 502      | Command not implemented （命令不可实现）                     |

**SMTP工作过程**

SMTP要经过建立连接、传送邮件和释放连接三个阶段具体为：

1. 建立TCP连接，服务器的端口号为25
2. 客户端向服务器发送HELLO命令以标识发件人自己的身份，然后客户端发送MAIL命令
3. 服务器端以OK作为响应，表示准备接收
4. 客户端发送RCPT命令
5. 服务器端表示是否愿意为收件人接收邮件
6. 协商结束，发送邮件，用命令DATA发送输入内容
7. 结束发送，用QUIT命令退出
8. 断开TCP的连接

## SMTP协议扩展

**SMTP协议的局限性**

- SMTP协议限制所有邮件的首部和报文的主体只能用七比特的ASCLL码表示
- 每一行包括的字符数不能超过一千个（以一对换行符和回车符标识一行）

SMTP协议不支持多国语言，不支持除了文本以外的附件形式

**SMTP协议扩展**

RFC 1869：规定了对SMTP协议进行扩展所要遵循的框架

RFC 2045-RFC 2049：多用途因特网邮件扩展（Multipurpose Internet Mail Extensions, MIMT）

有了MIME，邮件可以使用世界各国文字的字符集而不仅仅限于ASCLL码，同时还能传输文字以为的其他内容（如音频，图片等）。

**MIME**

MIME给出了如何描述数据中所含有的文件类型（如text/html,img/jpeg）。MIME给出了如何把非ASCLL码字符转换成ASCLL字符。quoted-printable编码常用于邮件正文的编码，将正文中的非ASCLL字符（如汉字）编码成ASCLL字符，base64编码适合于任意的二进制文件，多用于对字符进行编码，将附件编码成ASCLL码。

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/MIME.png)

**常见的MIME类型**

- 超文本标记语言文本 .html,.html text/html
- 普通文本 .txt text/plain
- RTF文本 .rtf application/rtf
- GIF图形 .gif image/gif
- JPEG图形 .ipeg,.jpg image/jpeg
- au声音文件 .au audio/basic
- MIDI音乐文件 mid,.midi audio/midi,audio/x-midi
- RealAudio音乐文件 .ra, .ram audio/x-pn-realaudio
- MPEG文件 .mpg,.mpeg video/mpeg
- AVI文件 .avi video/x-msvideo
- GZIP文件 .gz application/x-gzip
- TAR文件 .tar application/x-tar

## SMTP认证

SMTP协议原本不具有认证机制，存在安全隐患

- 任何人都可以连接邮件服务器发邮件
- 垃圾邮件

RFC2554对SMTP协议做了扩展，给出SMTP服务器如何认证合法用户的机制和方法

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/SMTP认证.png)

**邮件接收协议**

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/邮件接收协议.png)

## POP3

POP3（Post Office Protocol,POP3）协议是专门用来从邮件服务器上获取邮件的协议，有了POP3协议，人们能够将邮件下载到本地的硬盘中进行离线阅读和处理。与SMTP协议不同，POP3执行的是“拉”操作，是将远程服务器上的邮件下载到本地。而SMTP执行的是“推”操作，是将本地邮件发送给远端的服务器，POP3协议在RFC1939中定义。

POP3协议的工作模型为客户-服务器模型，POP3协议使用TCP协议作为运输层协议，POP3服务器的默认服务端口是110，POP3协议以命令-应答的方式工作。

**POP3主要命令**

| 命令名 | 命令格式              | 命令用途                                       |
| ------ | --------------------- | ---------------------------------------------- |
| USER   | USER < username >     | 系统登录需要的用户名                           |
| PASS   | PASS < password >     | 系统登录需要的密码                             |
| APOP   | APOP < name, digest > | 系统登录采用经过认证的密码信息                 |
| STAT   | STAT                  | 返回邮箱统计信息，包括邮件数、总字节数         |
| LIST   | LIST < message >      | 返回指定邮件的大小                             |
| RETR   | RETR < message >      | 返回指定邮件的内容，包括邮件头与正文           |
| DELE   | DELE < message >      | 为指定邮件做删除标记，在退出系统时删除指定邮件 |
| RSET   | RSET                  | 撤销所有的DELE命令                             |
| UIDL   | UIDL < message >      | 返回指定邮件的唯一标识                         |
| TOP    | TOP < message,n >     | 返回指定邮件的内容和前n行                      |
| NOOP   | NOOP                  | 没有动作，只是等待服务器返回OK                 |
| QUIT   | QUIT                  | 退出系统登录                                   |

POP3应答，肯定应答为 +OK ，否定应答为 -ERR。

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/POP3命令交互.png)

## IMAP协议

IMAP协议支持离线方式，能够将邮件下载到本地，进行离线阅读和管理，同时IMAP协议还能进行在线邮件管理，IMAP协议支持用户在邮件服务器上建立任意层次的远程文件夹，对邮件进行分类管理，同时IMAP还提供摘要浏览功能，对于有多个附件的邮件，IMAP协议允许用户只下载其中的一个附件或几个附件。

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/IMAP.png)

## DHCP

---

静态IP地址方案

- 手工设定
- 容易出错，工作量大

动态IP地址方案（DHCP）

- 自动从服务端获取IP地址信息
- 方便，大大减轻工作量

**动态IP地址方案优缺点**

优点

- 减少管理员的工作量（不必手动更改或配置IP地址、子网掩码、DNS等）
- 避免IP地址冲突问题
- 自动更新功能

缺点

- 主机获得的IP地址不固定，对于提供网络服务的主机不适合
- 需要配置专门的DHCP服务器

----

动态主机配置协议（Dynamic Host Configuration Protocol，DHCP）能够自动地为网络中没有IP地址的主机分配IP地址、子网掩码等信息，不在需要手动进行配置。DHCP协议是在BOOTP（Bootstrap Protocol）协议的基础上发展起来的，在RFC 2131和RFC2132中定义。

**DHCP工作原理**

DHCP协议使用UDP作为运输层协议，DHCP服务器的端口号是67，客户端的端口号是68。DHCP服务器管理一个或多个IP地址域，每个域称为地址池。当收到客户端的请求后，就从地址池中取出一个未用的地址分配给客户端，称为出租。

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/DHCP工作原理.png)

1. DHCP客户端在网络中广播DHCP Discover报文，寻找DHCP服务器
2. DHCP服务器从地址池中选取地址，想客户端发送DHCP Offer报文，提供IP地址，掩码等信息
3. DHCP客户端广播DHCP request报文，想服务器申请IP地址等信息
4. DHCP服务器向客户端发送DHCP Ack报文，分配IP地址等信息

**DHCP报文**

| 编号 |   消息类型    | 说明                                                |
| :--: | :-----------: | :-------------------------------------------------- |
|  01  | DHCP Discover | 客户端广播，用来发现DHCP服务器                      |
|  02  |  DHCP Offer   | 服务器对DHCP Discover的响应，提供IP地址、掩码等信息 |
|  03  | DHCP Request  | 客户端向服务器发出配置请求或请求延长租期            |
|  04  | DHCP Decline  | 客户端向服务器发出，指明无效的参数信息              |
|  05  |   DHCP Ack    | DHCP服务器确认客户端配置参数                        |
|  06  |   DHCP Nak    | 服务器向客户端发出，通告IP地址错误或租期到期        |
|  07  | DHCP Release  | 客户端通知服务器放弃租用的IP地址                    |
|  08  |  DHCP Inform  | 客户端向服务器发送要求配置部分参数                  |

**DHCP报文格式**

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/DHCP报文格式.png)

**DHCP Discover报文**

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/DHCPDiscover报文.png)

**DHCP Offer报文**

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/DHCPOffer报文.png)

**DHCP Request报文**

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/DHCPRequest报文.png)

**DHCP Ack报文**

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/DHCPack报文.png)

**DHCP租期**

DHCP服务器分配给DHCP客户的IP地址是临时的，DHCP客户端只在一段时间内能够使用这个IP地址，这段时间称为租用期（Lease Time），租用期的大小由服务器决定，在DHCP Ack报文中给出。租期快到时，客户端需要向服务器申请续租。

**DHCP中继**

DHCP报文的广播特性要求客户端和服务器要在同一个物理网络内（广播报文不能跨越路由器），否则DHCP Discover报文将无法到达DHCP服务器。可以在路由器配置DHCP中继（DHCP Relay），DHCP中继可以跨越网络在DHCP服务器和客户端之间转发DHCP报文。

![](https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/DHCP中继.png)

## 总结

| 应用层服务名称                                              | 作用                                                         | 端口                   | 运输层协议 |
| ----------------------------------------------------------- | ------------------------------------------------------------ | ---------------------- | ---------- |
| 超文本传输协议HTTP（Hyper Text Transfer Protocol）          | HTTP是Web浏览器和Web服务器之间应用层协议，是无状态协议       | 80                     | TCP        |
| 文件传送协议FTP（File Transfer Protocol）                   | FTP提供交互式的访问，允许客户指明文件的类型与格式，并允许文件具有存取权限 | 21控制端口；20数据端口 | TCP        |
| 简单文件传送协议TFTP                                        | 是一个小且易于实现的文件传送协议                             | 69                     | UDP        |
| 简单邮件传送协议SMTP（Simple Mail Transfer Protocol）       | SMTP协议发送邮件                                             | 25                     | TCP        |
| 邮件协议POP（Post Office System）                           | POP3协议接收邮件                                             | 110                    | TCP        |
| 域名系统DNS（Domain Name System）                           | 把域名转换成为网络可以识别的IP地址                           | 53                     | UDP        |
| 动态主机配置协议DHCP（Dynamic Host Configuration Protocol） | 提供一种自动为工作站分配IP地址并配置IP相关信息的方法         | 服务器端口67；客户端68 | UDP        |

