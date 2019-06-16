---
title: Vagrant学习笔记
tags: [Vagrant]
---

Vagrant是一个Virutal Machine管理工具，通过文件定制一个开发环境后，在任意一台安装了Vagrant的机器上就可以复用这套开发环境。

<!--more-->

### 在Mac里使用Vagrant

#### （一）让它跑起来先

##### 1) 下载安装VirtualBox
Vagrant支持VirtualBox、Hyper-V、Docker、VMware等Provider，这里以VirtualBox为例。需要先下载安装[VirtualBox](https://www.virtualbox.org/wiki/Downloads)。

##### 2) 下载安装Vagrant
Vagrant支持Mac OS X、Windows、Debian、Centos等操作系统，更新资料及下载地址，可到[Vagrant官网](https://www.vagrantup.com/downloads.html)查看。

##### 3) 手动下载添加Box
鉴于国内的网络环境，通过命令行不能直接下载vagrant box。可以通过先通过下面的方法手动添加Box。

1. 搜索要使用的基础Box，这里选ubuntu/trusty64这个Box，然后选择一个版本，这里选当前最新的一个版本（v20160406.0.0），看到的URL是这样的：<br>[https://atlas.hashicorp.com/ubuntu/boxes/trusty64/versions/20160406.0.0](https://atlas.hashicorp.com/ubuntu/boxes/trusty64/versions/20160406.0.0)
2. 把URL中的域名换成：vagrantcloud.com，这样得到的URL是：<br>[https://vagrantcloud.com/ubuntu/boxes/trusty64/versions/20160406.0.0](https://vagrantcloud.com/ubuntu/boxes/trusty64/versions/20160406.0.0)
3. 最后在URL后加上/providers/virtualbox.box，得到的URL是这样的：<br>[https://vagrantcloud.com/ubuntu/boxes/trusty64/versions/20160406.0.0/providers/virtualbox.box](https://vagrantcloud.com/ubuntu/boxes/trusty64/versions/20160406.0.0/providers/virtualbox.box)
4. 访问这个URL，下载文件到/Users/RobinShuai/Downloads目录。
5. 在命令行中输入下面的命令手动添加Box：<br>
	vagrant box add --name ubuntu/trusty64/Users/RobinShuai/	Downloads/trusty-server-cloudimg-amd64-vagrant-disk1.box
6. 在命令行输入下面命令确认成功添加Box：<br>
	vagrant box list
	
##### 4) 运行Vagrant
在命令行界面，输入下面的命令创建目录:

	mkdir -p /data/dev/vagrant/web_dev
	cd /data/dev/vagrant/web_dev

运行下面命令，会自动在当前目录创建Vagrantfile:

	vagrant init ubuntu/trusty64
执行下面命令，让它运行起来：

	vagrant up

执行下面命令，通过ssh进入虚拟机：

	vagrant ssh
	
在虚拟机命令行界面，执行命令回到Mac：

	exit

到这里已简单地让Vagrant跑起来了，可是这有什么作用呢（这是笔者的第一感觉）？为了证实它确实好用，下面将结合笔者在工作中遇到的问题，尝试用Vagrant来解决。

#### （二）让它好用起来

目前工作中使用开发环境是Linux＋Apache＋Nginx＋PHP＋MongoDB＋Redis＋Node。

##### 1) 注册绑定Atlas帐号
访问[https://atlas.hashicorp.com](https://atlas.hashicorp.com)，使用Email注册帐号，完成后进入“Account Settings” => “Tokens”界面，然后点击“Generate token”，将生成的Token复制保存起来（页面刷新后就再也看不到了）。

将这个Token绑定到Mac上，执行下面的命令：

	cd /data/dev/vagrant/web_dev
	vagrant login -t YOUR_TOKEN

##### 2) 编辑Vagrantfile，设置目录映射和端口映射。

	cd /data/dev/vagrant/web_dev
	vim Vagrantfile

使用下面的命令去除注释和多余的空行：

	cat Vagrantfile | grep -v "#" | grep -vE "^$"

配置内容如下：

	Vagrant.configure(2) do |config|
	  config.vm.box = "ubuntu/trusty64"
	  
	  #是否自动检查
	  config.vm.box_check_update = false 
	  
	  #端口映射，将Mac上的端口8080映射到虚拟机里的80端
	  config.vm.network "forwarded_port", guest: 80, host: 8080
	  
	  #默认会将Vagrantfile所在目录映射到虚拟机里/vagrant目录
	  #目录映射，将Vagrantfile所在目录下的data目录映射到虚拟机里的/data目录
	  config.vm.synced_folder "./data", "/data"
	  
	  # 执行Shell脚本安装apache，这里提供Shell、Docker等方式
	  config.vm.provision "shell", inline: <<-SHELL
	     sudo apt-get update
	     sudo apt-get install -y apache2
   	   SHELL
	end
	
执行下面的命令按最新配置文件重启：

	vagrant reload


重启成功后，打开本地电脑，访问http://127.0.0.1:8080/即可看到apache的默认页（It works!）。

此时在本机映射的目录（/data/dev/vagrant/web_dev/data）下的文件相关的操作会同步到虚拟机里的目录里（/data）,如下代码：

	cd /data/dev/vagrant/web_dev/data
	touch helloworld.sh
	
	# 进入虚拟机
	vagrant ssh
	# 以下命令是在虚拟机里执行
	ls /data # 执行后会看到hellworld.sh
	
至此，介绍了常用的功能，目录映射和端口映射，后续再写一篇文章介绍安装Nginx+Apache+PHP。

#### (三）用它来结对编程吧

##### 1)分享虚拟机环境
vagrant还提供了一个share的功能，参考官方文档[https://www.vagrantup.com/docs/share/](https://www.vagrantup.com/docs/share/)。这里简单介绍下，在本机执行以下命令：

	cd /data/dev/vagrant/web_dev
	vagrant share --ssh myshare
	
这个时候访问http://myshare.vagrantshare.com即可看到Apache的“It works”页面。

##### 2)windows连接分享的虚拟机环境

windows也需要安装VirtualBox和Vagrant，步骤参考上面的。此外这里安装Cygwin（为了使用ssh），步骤如下：

1. 下载[Cygwin](http://www.cygwin.com/),根据操作系统是32位还是64位，选择对应的版本。
2. 安装Cygwin，默认是不安装openSSH的，需要手动选择。输入openssh，然后选择。
3. 安装完毕后，把C:/cygwin/bin;加入系统环境变量的Path中。

接下来打开Cgywin，并在命令行中输入以下命令：

	cd /cygdrive/c/HashiCorp/Vagrant/bin
	vagrant.exe connect --ssh myshare
	
进入分享的虚拟机（这里要等待一会，看网络情况）后，执行下面的命令：

	sudo vim /var/wwww/html/index.php # 修改“It works!”为“It works by share!”
	
保存修改后，访问http://myshare.vagrantshare.com，可看到修改后的页面。