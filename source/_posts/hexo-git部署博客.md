---
title: hexo+git部署博客
date: 2016-10-08 22:13:57
tags:
- hexo
- git
categories:
- 个人博客
---

简单介绍一下我搭建这个博客的心得。

<!--more-->
### 安装Hexo

首先，我是在windows上搭建的博客(才不是没钱买mac)。
安装Git,Node.js具体安装百度即可。
安装上面两个之后就可以利用npm命令来安装hexo，执行下面这条命令：

``` bash
$ npm install hexo -g
```

### 初始化生成博客

初始化Hexo博客只需进入博客目录，执行下面这条命令：

``` bash
$ hexo init
```

初始化博客之后博客目录下会产生一些配置文件和模板文件，现在就要利用这些模板文件来生成我们的静态博客文件，通过下面这条命令就可以生成我们的博客。

``` bash
$ hexo generate
```

### 本地部署

生成博客之后我们就可以开始在本地部署我们的博客，执行下面这条命令。

``` bash
$ hexo server
```

现在打开浏览器访问http://localhost:4000 就可以看到我们的博客了。

### 部署到GitHub
通过上面这些步骤我们只是在本地搭建起了博客，要让别人看到还需要把它部署到GitHub上。
首先，当然是需要申请一个GitHub账号，然后新建一个Repository，接着在命名的时候需要注意，GitHubPage的名字必须用户名.github.com或者用户名.github.io。
创建完Repository之后，我们要配置Hexo的github地址然后才可以把我们的博客部署到GitHub上面。在博客目录底下打开_congig.yml，这就是Hexo的配置文件。找到下面这段代码：

``` bash
$   # Deployment
    ## Docs: http://hexo.io/docs/deployment.html
    deploy:
    type:
```

把上面的内容替换成你刚才创建的Repository地址，如下:

``` bash
$   # Deployment
    ## Docs: http://hexo.io/docs/deployment.html
    deploy:
    type: github
    repository: https://github.com/用户名/用户名.github.io.git
    branch: master
```

创建Repository并且修改配置文件之后我们就可以把我们的博客部署到GitHub上面了，通过下面这个命令，过程中需要输入Git账号和密码:

``` bash
$  hexo deploy
```

现在打开浏览器访问http://用户名.github.com 或者 http://用户名.github.io ，部署之后大约十分钟就可以看到博客效果了。

关于Git,具体可以看廖雪峰的[教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)。这里有一点说一下，安装完成后要配置username，usermail，注意设置完是没有任何显示的，我一开始以为是自己的问题，其实并不是。

一开始我是大体是照着这个[百度经验](http://jingyan.baidu.com/article/d8072ac47aca0fec95cefd2d.html)搭建的。

安装完博客之后开始考虑优化，可以参考[这里](https://www.zhihu.com/question/24422335)，只要到Hexo目录下从Git克隆代码，具体操作参照N[ext使用手册](http://theme-next.iissnan.com/)