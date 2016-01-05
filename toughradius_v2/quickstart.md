# ToughRADIUS快速指南

## 准备

一台完整的服务器，或者远程VPS，给服务器安装Linux系统，CentOS6以上，ubuntu13以上，或者其他你自己熟悉的Linux发行版。

你要懂一点技术，比如安装操作系统，会在终端敲命令。

![][image-1]

ToughRADIUS 是Docker技术的拥抱者，如果想更好的使用ToughRADIUS，你也需要去学习关于Docker的知识。

## 安装部署

ToughRADIUS主要采用了Docker镜像部署的模式，ToughRADIUS的镜像基础是ubuntu 14。

> 我们可以把Docker看作一个软件集装箱，半世纪之前，集装箱发挥了巨大的力量，改变了整个运输产业，也改变了人们的生活。而Docker就类似这样一个集装箱工具，只不过他封装的是软件。

> 还记得linux安装lamp的经历吗？现在可以对各种安装配置apache，php等繁琐的工作说再见了。

> 我们把ToughRADIUS相关的配置，运行依赖环境等全部打包在一个“Docker集装箱”里，我们只需要在我们的服务器上简单的安装一个支持运行“Docker集装箱”的环境，那么我们不用去折腾各种运行环境搭建就能简单的让ToughRADIUS跑起来。

> 通常我们把封装了软件应用的“Docker集装箱”叫做镜像，有点类似你可能了解的ISO文件。

### 下载Linux专用脚本工具

	wget https://github.com/talkincode/ToughRADIUS/raw/master/trshell  -O /usr/local/bin/trshell
	chmod +x /usr/local/bin/trshell

看看这个工具为我们提供了那些功能

	trshell help

	Usage: /usr/local/bin/trshell [OPTIONS] instance
	
	docker_setup       install docker, docker-compose
	pull               toughradius docker images pull
	standalone         install toughradius with already exists mysql
	with_mysql         install toughradius with new docker mysql instance
	remove             uninstall toughradius and database
	status             toughradius instance status
	restart            toughradius instance restart
	stop               toughradius instance stop
	upgrade            toughradius instance upgrade
	logs               toughradius instance logs
	dsh                toughwlan instance bash term
	
	All other options are passed to the trshell program.

### Docker环境安装

我们首先应该安装配置服务器的Docker运行环境，以下指令会自动根据当前linux版本下载对应的docker版本进行自动安装。

	trshell docker_setup


### ToughRADIUS 应用实例创建

> 注意，trshell创建容器指令需要交互式完成，请根据提示进行输入操作

	trshell with_mysql t1     # t1表示实例名，可自定义

以上指令会部署一个名称为t1的实例，同时为这个实例部署一个专用的MySQL数据库，如果已经有存在的MySQL数据库服务器，可使用以下指令创建一个独立的实例

	trshell standalone t1     ＃ t1表示实例名，可自定义

### 应用管理

这样我们的服务就已经运行了。我们可以通过浏览器来访问我们的应用了。

营业管理：http://ipaddr:1816   管理权限 admin/root


#### 防火墙设置

注意：如果访问不了web，可能是防火墙禁止了相关端口，如果不打算用内置防火墙，可以关闭防火墙。

	systemctl stop firewalld.service

禁止firewall开机启动，防火墙就永久性关闭了。

	systemctl disable firewalld.service



[image-1]:	../imgs/docker.png