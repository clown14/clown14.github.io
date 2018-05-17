---
title: 图解HTTP学习笔记（一）
date: 2018-05-02 19:18:34
tags: HTTP
categories: HTTP
---

图解HTTP（一）
<!-- more -->

# 了解Web与网络基础

## 网络基础TCP/IP
通常使用的网络实在TCP/IP协议族的基础上运作的，HTTP属于它内部的子集。

### TCP/IP协议族
不同硬件、操作系统之间的通信，所有的这一切都需要一种规则。这种规则称为协议（protocal），TCP/IP是互联网相关的各类协议族的总称。

### TCP/IP的分层管理
TCP/IP协议族按层次分别分为：应用、传输、网络、数据链路层。
应用层决定了向用户提供了应用服务时通信的活动。
传输层对上层应用层，提供处于网络连接中的两台计算机之间的数据传输。
网络层用来处理在网络上流动的数据包。
链路层用来处理联结网络的硬件部分。

### TCP/IP通信传输流
<img src="/img/HTTP1.png" alt="图片名称" title="">
<img src="/img/HTTP2.png" alt="图片名称" title="">

## 与HTTP关系密切的协议：IP、TCP和DNS

### 负责传输的IP协议（网络层）

IP协议的作用是把各种数据包传送给对方，而要保证确实传送到对方那里，两个重要条件是IP地址和MAC地址。IP地址指明了节点被分配到的地址，MAC地址是指网卡所属的固定地址。IP地址考研和MAC地址进行配对，IP地址可变换，MAC地址基本上不会更改。

ARP是一种用以解析地址的协议，根据IP地址考研反查出MAC地址。

无论哪台计算机、网络设备，都无法全面掌握互联网中的细节。

### 确保可靠性的TCP协议（传输层）

TCP提供可靠的字节流服务，即为了方便传输，把大块数据分割成以报文段为单位的数据包进行管理。

TCP协议采用三次握手将数据准确地送达目标处。发送端发送带SYN的数据包，接收端回传带SYN/ACK的数据包，发送端再回传带ACK的是数据包。
<img src="/img/HTTP3.png" alt="图片名称" title="">

### 负责域名解析的DNS服务（应用层）

DNS协议提供通过域名查找IP地址，或逆向从IP地址反查域名的服务。
<img src="/img/HTTP4.png" alt="图片名称" title="">

## URI和URL

URI：Uniform Resource Identifier，统一资源标识符，
URL：Uniform Resource Locator，统一资源定位符。

URI用字符串标识某一互联网资源，而URL表示资源的地点。可见URL是URI的子集。
<img src="/img/HTTP5.png" alt="图片名称" title="">

# 简单的HTTP协议

## HTTP协议用于客户端和服务器端之间的通信

应用HTTP协议时，必定一端担任客户端，另一端担任服务器
<img src="/img/HTTP6.png" alt="图片名称" title="">

## 通过请求和响应的交换达成通信

请求必由客户端发出，服务端回复响应
<img src="/img/HTTP7.png" alt="图片名称" title="">
<img src="/img/HTTP8.png" alt="图片名称" title="">
<img src="/img/HTTP9.png" alt="图片名称" title="">

## HTTP是不保存状态的协议

不保存状态即无状态，HTTP协议自身不对请求和响应之间的通信状态进行保存。
<img src="/img/HTTP10.png" alt="图片名称" title="">

## 告知服务器意图的HTTP方法

### GET:获取资源

GET方法请求访问已被URI识别的资源。指定的资源经服务器解析后返回响应内容。
<img src="/img/HTTP11.png" alt="图片名称" title="">

### POST：传输实体主体
<img src="/img/HTTP12.png" alt="图片名称" title="">

### PUT：传输文件

PUT用来传输文件，像FTP一样，要求在请求报文的主题中包含文件内容，然后保存到请求URI指定的位置。但自身不带验证机制，存在安全问题，所有一般不使用。
<img src="/img/HTTP13.png" alt="图片名称" title="">

### HEAD：获得报文首部

HEAD跟GET一样，指示不返回报文主体部分。
<img src="/img/HTTP14.png" alt="图片名称" title="">

### DELETE：删除文件

DELDETE用来删除文件，与PUT相反，同样不安全，不常使用。
<img src="/img/HTTP15.png" alt="图片名称" title="">
<img src="/img/HTTP16.png" alt="图片名称" title="">

## 持久连接节省通信量
<img src="/img/HTTP17.png" alt="图片名称" title="">

### 持久连接

为解决上述TCP连接的问题，HTTP/1.1和部分HTTP/1.0提出了持久连接，只要任意一端没有明确提出断开连接，则保持TCP连接状态。
<img src="/img/HTTP18.png" alt="图片名称" title="">

### 管线化

持久连接使得多数请求以管线化方式发送成为可能。即同时并行发送多个请求。
<img src="/img/HTTP19.png" alt="图片名称" title="">

## 使用Cookie的状态管理

Cookie技术通过在请求和响应报文中写入Cookie信息来控制客户端的状态。Cookie会根据从服务器发送的响应报文内的Set-Cookie首部字段信息，通知客户端保存Cookie。
<img src="/img/HTTP20.png" alt="图片名称" title="">

# HTTP报文内的HTTP信息

## HTTP报文

用于HTTP协议交互的信息称为HTTP报文。客户端的HTTP报文叫做请求报文，服务器端的叫做响应报文。
<img src="/img/HTTP21.png" alt="图片名称" title="">
<img src="/img/HTTP22.png" alt="图片名称" title="">


