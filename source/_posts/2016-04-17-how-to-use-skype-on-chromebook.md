---
title: Chromebook使用Skype进行视频聊天
tags: [skype]
---


老婆和一对美国夫妇（在中国居住）聊天，得知对方有一台在美国买的ChromeBook，但却不能用Skype和家人聊天，老婆就帮我揽下这个活（程序员～电脑修理工）。

<!--more-->


## 冒险之旅

虽然早就耳闻Chromebook大名，这还是第一次见到真机（人）。拿到手的时候，不知道管理员密码（他们也忘记了）。首先需要重新注册一个Google帐号（步骤请自行参考官网）。

### Google神器
当我遇到疑问的时候，找Google帮忙，每次都能给我帮助和指引。首先尝试使用Skype的Web版，体验后发现视频功能只支持windows和Mac OS X，不支持linux，因此只能使用客户端版本，这里它给我提供了两条路。

#### 1.Run Android Apps on Chrome
开始安装Chrome插件，也下载了Skype的apk文件，按照步骤进行，插件无响应，灰心沮丧，歇了几天，放在一旁不管。

#### 2.Run ubuntu via crouton
在Chromebook上通过crouton安装Ubuntu，然后在Ubuntu里安装Skype。以下步骤参考这篇[文章](http://mytiankong.com/?p=16178)。

* 启用开发者模式，同时按下Esc＋Refresh＋Power键，当看到屏幕上出现一个黄色感叹号（操作系统验证），按Ctrl+D键。
* 从Github下载crouton（[https://github.com/dnschneid/crouton](https://github.com/dnschneid/crouton)）,到Downloads目录
* 同时按住Ctrl+Alt+T键进入Crosh终端，运行命令,耐心等待：

    crosh> shell
    
    crosh> sudo sh -e ~/Downloads/crouton -r trusty -t unity

* 在Crosh终端，输入命令启动Ubuntu：
    
    crosh> sudo startunity
* 安装Ubuntu相关软件
	+ 安装synaptic（sudo apt-get install synaptic ）
	+ 安装多语言支持，sudo apt-get install language-selector-gnome
	+ 添加repository，sudo add-apt-repository “deb http://archive.canonical.com/ $(lab_release -sc) partner"
	+ 安装Skype，sudo apt-get update && sudo apt-get install skype


## 后记
历经各种挫折，修成正果！在此，记录步骤，以备不时之需，如果能帮到有同样需求的朋友就更欣慰了。
