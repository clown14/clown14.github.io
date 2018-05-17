---
title: 图解HTTP学习笔记（二）
date: 2018-05-02 20:08:10
tags: HTTP
categories: HTTP
---

图解HTTP（二）
<!-- more -->

# 返回结果的HTTP状态码

## 状态码告知从服务器端返回的请求结果

状态码的职责是当客户端向2服务器端发送请求时，描述返回的请求结果。
<img src="/img/HTTP23.png" alt="图片名称" title="">

## 2XX成功

2XX的响应结果表明请求被正常处理了。

### 200 OK

<img src="/img/HTTP24.png" alt="图片名称" title="">
表示从客户端发来的请求在服务器端被正常处理了。

### 204 No Content

<img src="/img/HTTP25.png" alt="图片名称" title="">
代表服务器接收的请求已成功处理，但在返回的响应报文中不含实体的主体部分。

### 206 Partial Content

<img src="/img/HTTP26.png" alt="图片名称" title="">
表示客户端进行了范围请求，而服务器成功执行了这部分的GET请求。

## 3XX 重定向

3XX响应结果表明浏览器需要执行某些特殊的处理以正确处理请求。

### 301 Moved Permanently

<img src="/img/HTTP27.png" alt="图片名称" title="">
永久性重定向。表示请求的资源已被分配了新的URI，以后应使用资源现在所指的URI。

### 302 Found

<img src="/img/HTTP28.png" alt="图片名称" title="">
表示请求的资源已被分配的新的URI，希望用户能使用新的URI访问。和301相比，302只是临时性质的改变，可能还会改变。

### 303 See Other

<img src="/img/HTTP29.png" alt="图片名称" title="">
表示由于请求对应的资源存在另一个URI。

### 304 Not Modified

<img src="/img/HTTP30.png" alt="图片名称" title="">
表示客户端发送附带条件的请求时，服务器端允许请求访问资源，但为满足条件的情况。

### 307 Temporary Redirect

临时重定向。该状态码与302Found有着相同的含义。

## 4XX客户端错误

### 400 Bad Request

<img src="/img/HTTP31.png" alt="图片名称" title="">
表示请求报文中存在语法错误。

### 401 Unauthorized

<img src="/img/HTTP32.png" alt="图片名称" title="">
表示发送的请求需要有通过HTTP认证（BASIC认证、DIGEST认证）的认证信息。若之前已进行过一次请求，则表示用户认证失败。

### 403 Frobidden

<img src="/img/HTTP33.png" alt="图片名称" title="">
表明对请求资源的访问被服务器拒绝了。

### 404 Not Found

<img src="/img/HTTP34.png" alt="图片名称" title="">
表明服务器上无法找到请求的资源，除此之外，也可以在服务器端拒绝请求且不想说明理由时使用。

## 5XX服务器错误

### 500 Internal Server Error

<img src="/img/HTTP35.png" alt="图片名称" title="">
表明服务器在执行请求时发生了错误。也可能是Web应用存在的bug或某些临时的故障。

### 503 Service Unavailable

<img src="/img/HTTP36.png" alt="图片名称" title="">
表明服务器暂时处于超负载或正在进行停机维护，无法处理请求。

# 与HTTP协作的Web服务器

## 通信数据转发程序：代理、网关、隧道

代理是一种有转发功能的应用程序，扮演了服务器和客户端“中间人”的角色，接收2由客户端发送的请求并转发给服务器，同时也接收服务器返回的响应并转发给客户端。

网关是转发其他服务器通信数据的服务器，接收从客户端发送来的请求时，它就像自己拥有资源的源服务器一样对请求进行处理。

隧道是在相隔甚远的客户端和服务器两者之间进行中转，并保持双方通信连接的应用程序。

### 代理

<img src="/img/HTTP37.png" alt="图片名称" title="">
<img src="/img/HTTP38.png" alt="图片名称" title="">
<img src="/img/HTTP39.png" alt="图片名称" title="">

使用代理服务器的理由：利用缓存技术减少网络带宽的流量，组织内部针对特定网站的访问控制，以获取访问日志为主要目的，等等。

缓存代理：
代理转发响应时，缓存代理预先将资源的副本（缓存）保存在代理服务器上，当再次请求相同资源，可以返回之前缓存的资源。

透明代理：
转发请求或响应时，不对报文进行修改，反之称为非透明代理。

### 网关

<img src="/img/HTTP40.png" alt="图片名称" title="">
网关的工作机制和代理十分相似，而网关能够使通信线路上的服务器提提供非HTTP协议服务，利用网关嗯那个提高通信的安全性。

### 隧道
 
<img src="/img/HTTP41.png" alt="图片名称" title="">
隧道的目的时确保客户端与服务器进行安全的通信。

## 保存资源的缓存

缓存是指代理服务器或客户端本地磁盘内保存的资源副本，利用缓存可减少对源服务器的访问，因此也就节省了通信流量和通信时间。
<img src="/img/HTTP42.png" alt="图片名称" title="">