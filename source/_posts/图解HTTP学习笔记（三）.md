---
title: 图解HTTP学习笔记（三）
date: 2018-05-14 19:16:24
tags: HTTP
categories: HTTP
---

图解HTTP（三）
<!-- more -->

# HTTP首部

## HTTP报文首部

<img src="/img/HTTP43.png" alt="图片名称" title="">
<img src="/img/HTTP44.png" alt="图片名称" title="">

## HTTP首部字段

使用首部字段是为了给浏览器和服务器提供报文主体大小、所使用的语言、认证信息等内容。HTTP首部字段由首部字段名和字段值构成，中间用冒号分隔。

<img src="/img/HTTP45.png" alt="图片名称" title="">

## HTTP/1.1通用首部字段

通用首部字段是指，请求报文和响应报文双方都会使用的首部。

### Cashe-Control

通过指定Caseh-Control的指令，就能操作缓存的工作机制。
<img src="/img/HTTP46.png" alt="图片名称" title="">
<img src="/img/HTTP47.png" alt="图片名称" title="">
例如：
<img src="/img/HTTP48.png" alt="图片名称" title="">
当指定使用public指令，则明确表明其他用户也可以利用缓存。

<img src="/img/HTTP49.png" alt="图片名称" title="">
使用no-cashe是为了防止从缓存中返回过期的资源。
<img src="/img/HTTP50.png" alt="图片名称" title="">
该指令规定缓存不能再本地存储请求或响应的任一部分。

### Connection

作用：控制不再转发给代理的首部字段，管理持久连接
<img src="/img/HTTP51.png" alt="图片名称" title="">
HTTP/1.1的默认连接都是持久连接。1.1之前的默认是非持久连接，要想在旧版的HTTP协议上维持持续连接，需指定Connection值为Keep-Alive。

### Date

Date表明创建HTTP报文的日期和时间。
<img src="/img/HTTP52.png" alt="图片名称" title="">

### Trailer

Trailer会事先说明在报文主体后记录了哪些首部字段。
<img src="/img/HTTP53.png" alt="图片名称" title="">

### Transfer-Encoding

<img src="/img/HTTP54.png" alt="图片名称" title="">
规定了传输报文主体时采用的编码方式。

### Upgrade
 
<img src="/img/HTTP55.png" alt="图片名称" title="">
用于检测HTTP协议及其他协议是否可使用更高的版本进行通信。

### Via

<img src="/img/HTTP56.png" alt="图片名称" title="">
追踪客户端与服务器之间的请求和响应报文的传输路径。

## 请求首部字段

### Accept

<img src="/img/HTTP57.png" alt="图片名称" title="">
通知服务器，用户代理能够处理的媒体类型及媒体类型的相对优先级。

### Accept-Charset

<img src="/img/HTTP58.png" alt="图片名称" title="">
通知服务器用户代理支持的字符集及字符集的相对优先顺序。

### Accept-Encoding

<img src="/img/HTTP59.png" alt="图片名称" title="">
告知服务器用户代理支持的内容编码及内容编码的优先级顺序。

### Accept-Language

<img src="/img/HTTP60.png" alt="图片名称" title="">
告知服务器用户代理能够处理的自然语言以及自然语言集的相对优先级。

### Authorization

<img src="/img/HTTP61.png" alt="图片名称" title="">
告知服务器，用户代理的认证信息。

### Except

<img src="/img/HTTP62.png" alt="图片名称" title="">
告知服务器，期待出现的某种特定行为。

### From

<img src="/img/HTTP63.png" alt="图片名称" title="">
告知服务器使用用户代理的用户的电子邮件地址。

### Host

<img src="/img/HTTP64.png" alt="图片名称" title="">
告知服务器，请求的资源所处的互联网主机名和端口号。

### Referer

<img src="/img/HTTP65.png" alt="图片名称" title="">
告知服务器请求的原始资源URI。

### User-Agent

<img src="/img/HTTP66.png" alt="图片名称" title="">
会将创建请求的浏览器和用户代理名称等信息传达给服务器。