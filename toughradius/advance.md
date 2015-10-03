# 高级技巧

## 通过 systemd 设置系统服务

目前很多Linux的发行版已经开始采用systemd系统，通过systemd来设置ToughRADIUS的服务是一件很容易的事情。

创建文件 /usr/lib/systemd/system/toughradius.service

内容如下: 

	[Unit] 
	Description=ToughRADIUS Service 
	After=docker.service 
	Requires=docker.service 
	
	[Service] 
	Type=forking 
	RemainAfterExit=yes 
	ExecStart=/usr/bin/docker start trserver 
	ExecStop=/usr/bin/docker stop trserver 
	ExecReload=/usr/bin/docker restart trserver 
	
	[Install] 
	WantedBy=multi-user.target 

其中trserver就是你的容器实例的名称。通过以下指令设置开机启动开关。

	systemctl enable trserver

由于我们的Docker容器都依赖于docker服务本身，因此也要保证docker服务开启。

	systemctl enable docker



