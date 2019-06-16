---
title: Jenkins实现自动化构建
tags: [jenkins]
---

最早接触Jenkins是在2012年，那时候笔者在一家银行做.NET开发，当时组内使用Jenkins构建.NET应用，但笔者几乎没有用它，只是用来看构建结果，如果构建失败则联系组长，让他帮忙处理。就这样Jenkins在我的技术田野里撒下了种子。

<!--more-->

### 背景

时光苒苒，笔者来到现在的公司，用着世界上最好的编程语言（之一），没错，就是PHP。现在的迭代开发节奏，就和深圳的生活节奏似的，以周为单位，周一需求评审，周五提测，下周三发布，周而复始。从开发环境、测试环境、预发布环境，到生产环境。每个环境都需要自己提供代码包,非生产环境还有自己部署代码，手上又有好几个project需要部署，虽然有现成的发布系统，但是部署一次要花10多分钟，而且中间很多步骤需要人工准备和输入信息。很多时候修复紧急Bug然后部署，多几个来回心都折腾累了。

目前几个项目中，有些需要前端构建，一个做前端的同事，结合他之前开发的前端构建工具，自研了一个从svn发布代码的发布系统，实现机制就是通过ssh＋shell＋rsync，这个系统已在组内分享并宣传使用。

为什么我还要去研究Jenkins呢？对外的说辞是：我比较懒，希望可以提svn代码后自动部署；还希望后台也可以使用发布系统。此外，我认为它功能更强大，扩展性更好，而且是开源的，不需要自己维护代码。

### 安装Jenkins

详细的安装指南参考这里:[https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins)。

这里选择在Ubuntu下安装运行Jenkins,在命令行中输入：

	wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | 	sudo apt-key add -
	sudo sh -c 'echo deb http://pkg.jenkins-ci.org/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
	sudo apt-get update
	sudo apt-get install jenkins
	
如果需要更新最新版，在命令行中输入：

	sudo apt-get update
	sudo apt-get install jenkins
	
这样jenkins会加入系统的启动脚本中，默认的端口是8080，如果想通过80端口访问它，有下面几种方式：

1. 如果服务器上80端口未被占用，可直接通过修改配置文件(/etc/default/jenkins)。
2. 使用[iptables](http://linux.die.net/man/8/iptables)做端口转发。
3. 使用[Apache](http://httpd.apache.org/docs/current/mod/mod_proxy.html)做代理。
4. 使用[Nginx](http://nginx.org/en/docs/http/ngx_http_proxy_module.html)做代理。

### 使用Jenkins

在浏览器输入http://127.0.0.1，访问刚刚安装好的Jenkins，按照指引完成设置。

### 后续

后续有时间再另写一篇blog详细介绍Jenkins使用心得。