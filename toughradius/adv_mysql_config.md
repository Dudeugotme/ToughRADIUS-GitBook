
## ToughRADIUS使用MySQL存储数据

ToughRADIUS默认采用了SqLite存储数据，通常这足够运营上千的用户量了，不过当系统对数据的可管理性，系统的性能有更高的要求时，我们建议采用MySQL数据库来替换。

在ToughRADIUS发布的Docker镜像中，已经内置了MySQL的驱动，因此我们要做的仅仅是对数据库的配置。

首先安装配置我们的MySQL数据库，关于独立主机MySQL服务的安装配置，有很多资料可以参考，这里不在赘述，但我们推荐使用Docker镜像来部署Mysql，更加简单方便。

-  创建一个Mysql容器实例并自动创建一个数据库

	$ mkdir -p /home/var/lib/mysql  #创建一个文件夹来存放mysql数据文件
	$ docker run --name mysql -d --privileged -p 3306:3306 \
		-v /home/var/lib/mysql:/var/lib/mysql \
		-e MYSQL_USER=tradmin \
		-e MYSQL_PASSWORD=trradiusd \
		-e MYSQL_DATABASE=toughradius \
		-e MYSQL_ROOT_PASSWORD=myroot talkincode/docker-mysql

>   `MYSQL_ROOT_PASSWORD`: root用户密码
>   `MYSQL_DATABASE`: 自动创建一个数据库，非必选
>   `MYSQL_USER`: 随数据库创建一个用户
>   `MYSQL_PASSWORD`: 新建用户的密码


### 备份数据库

如果是首次安装，可略过，如果是迁移数据库，则务必进行备份。

### 创建数据库

通过MySQL客户端执行以下语句，请自行修改相关用户密码信息。如果已经使用Docker镜像自动创建数据库，则忽略。

	create database toughradius DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci; 
	GRANT ALL ON toughradius.* TO tradmin@‘%' IDENTIFIED BY ‘trradiusd' WITH GRANT OPTION; 
	FLUSH PRIVILEGES; 

### 配置ToughRADIUS

进入ToughRADIUS的系统管理控制台，打开数据库配置界面。设置MySQl数据库类型，并填写mysql数据库连接描述。注意数据库名称，用户名，密码对应。

	mysql://tradmin:trradiusd@192.168.1.10/toughradius?charset=utf8

![][image-1]

### 初始化MySQL数据库

完成创建和配置后，进入下一步的初始化过程，首先进入控制面板，重启所有服务，以保证所有服务都能加载最新配置。

点击初始化数据库按钮，完成数据库的初始化。

### 恢复数据

进入数据备份管理界面，恢复之前备份的数据，如果是首次安装则可忽略。

[image-1]:	../imgs/adv_mysql_config.jpg