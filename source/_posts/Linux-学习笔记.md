---
title: Linux 学习笔记
date: 2017-03-15 13:59:32
tags: Linux
categories: Linux
---
Linux概述
<!-- more -->

## 安装

首先安装虚拟机，可以自行搜索VM。
下载CentOS的linux系统，我的版本是CentOS-6.7-i386-bin-DVD1。

接下来，依次打开虚拟机--文件--下一步，选择安装盘镜像文件，目录为之前下载的linux系统，确保安装路径有20G的空间，接着点击下一步直至完成（可选择自定义硬件），随后开启此虚拟机，开始安装linux。

安装过程首先选择语言，然后在SOFTWARE SELECTION中选择GNOME DESKTOP，注意，GNOME为图形界面选项，如果采用默认值，安装完成后进入的就是命令行界面。
另外还要在INSTALLATION DESTINATION选项中确认一下，然后begin installtation，等待安装期间，创建用户以及设置root密码，完成后等待安装。

完成后点击reboot启动系统，第一次开机会有initial setup of CentOS Linux选项，依次
输入“1”，按Enter键
输入“2”，按Enter键
输入“q"，按Enter键	
输入“yes”，按Enter键

至此，linux安装完成。

## 常用Shell命令操作

### “|” 管道 
格式如：命令1|命令2
把左边命令1的输出作为右边命令2的输入。

### sudo 
以其他用户（默认是root）的身份去执行另一个命令。

### touch 创建文件，rm 删除文件，

	[cysun_14@promote ~]$ touch blank.txt
	[cysun_14@promote ~]$ rm blank.txt

【注】：rm有两个常用选项，-f 选项为强制删除，忽略不存在的文件。 -r 选项为递归删除目录及其内容。

### mkdir 创建目录，rmdir 删除目录

	[cysun_14@promote ~]$ mkdir firstdir
	[cysun_14@promote ~]$ redir firstdir

【注】：mkdir 可以使用选项 -p 指示如果要创建的目录的父目录不存在，一并创建。rmdir只能删除空目录。

### who whoami
who 可以查看所有正在使用系统的用户的用户名，所用终端，登录时间等，
whoami可以查看当前用户信息。

### ls
显示文件指定目录。

### whereis
根据文件名搜索二进制文件、手册页文件或源代码文件。未指定会将三类文件都显示出来。

### cat
cat有两项功能，一是显示文件内容，二是连接合并文件内容。

### vi
Visual Interface，UNIX系统中最常用的编辑器。

### cp 
将源文件复制到目标文件，或将多个源文件复制到目标目录。

	[cysun_14@promote ~]$ cp [选项] ... 源文件 目标文件

### mv 
将源文件重命名为目标文件，或将源文件移动至指定目录。

	[cysun_14@promote ~]$ mv [选项] ... 源文件 目标文件

### chown
更改文件属主

	[cysun_14@promote ~]$ chown [选项]... [所有者][:[组]] 文件...

### chgrp
更改文件属组

	[cysun_14@promote ~]$ chgrp [选项]... 用户组 文件...

### chmod
更改文件权限

	[cysun_14@promote ~]$ chmod [选项]... 模式[,模式] 文件...

## Shell编程基础知识

运行Linux Shell程序的方法有三种：
* 赋予程序文件可执行权限，直接运行。
* 调用命令解释器（Shell）解释运行。
* 使用source命令执行。

可执行命令分三种：
* 内部命令。
* Shell函数。
* 外部命令。

变量赋值方式： 变量名=变量值,
“=”两边不能有空格，引用变量时，在变量名前加“$”符号。

	[cysun_14@promote ~]$ str="Hello, world"
	[cysun_14@promote ~]$ echo $str

控制结构类似C,JAVA,略。

Shell函数

	[funciton] 函数名()
	{
		命令表
		return[n]
	}

n值是退出函数时的退出状态，未指定n默认取最后一个命令的退出状态。