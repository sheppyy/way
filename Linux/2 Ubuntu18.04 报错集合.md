# 1 虚拟机网络错误



## 1.1 网络不通

- 问题描述：

```
virtualbox 创建的Ubuntu虚拟机，宿主机ping不通虚拟机，但虚拟机确可以ping得通宿主机。
```

- 解决方法

  ```
  说明：
  	1. 下面的4个方法基本都可以解决这个问题，因为机器不一样，所以得一个一个方法试。
  	2. 设置后都重启一下虚拟机，让它生效
  	3. 下面的方法都是独立的
  ```

  

  1 关闭防火墙

```
Ubuntu操作防火墙：
	- 查看状态 sudo ufw status
	- 关闭    sudo ufw disable
	- 开启    sudo ufw enable
```

​		Windows关闭防火墙直接搜索点击关闭就行了

​		2  使用virtualbox创建虚拟机时默默只开一个网卡，所以，你把所有的网卡都开了，并且都选择桥接模式。

​		3  进入`控制面板\系统和安全\Windows Defender 防火墙` ，选择高级设置，在入站规则中找到 **虚拟机监控（回显请求 - ICMPv6-In）**和 **虚拟机监控（回显请求 - ICMPv4-In）**两兄弟，右键，启用规则即可。

​		4 进入**控制面板\网络和 Internet\网络和共享中心**，进入更改适配器设置，找到虚拟网络网卡（大概长这样：VirtualBox Host-Only Ethernet Adapter），右键，属性，Internet协议版本4（TCP/IPv4）,把ip地址设置跟宿主机一个网段。

# 2 安装库报错



## 2.1 执行pip时报dpkg





## 2.2 sudo apt-get 或 apt时，报错

- 描述：

```
提示有东西再后台跑着，获取不了锁🔒
	Could not get lock /var/lib/dpkg/lock-frontend ....
```



- 解决

```
因为是用apt-get或apt时说占用了，用下面的命令看看
  ps -A | grep apt
  ps -A | grep apt-get

要是真有东西出来，就照准进程ID，kill掉，再关掉终端，再试，就成了。
  kill 2020
  
  
说明：
	1.要是有其他类似的命令也是报这要的错，就这么搞。
	2. 
```



2.3 直接pip3 install mysqlclient时报错

- 描述

```
报 MySQLdb/_mysql.c:29:10: fatal error: mysql.h: No such file or directory`...错，即依赖缺陷问题。
```



- 解决

```shell
sudo apt install libmysqlclient-dev

说明：
	1. 安装一下依赖
```



# 3 运行错误



## 3.1 Python：程序运行时，报locale.Error: unsupported locale setting错

- 描述

```
在启动Python程序时，报了个locale.Error: unsupported locale setting。
大概原因就是不支持语言环境。
有些机子pip3安装库时也报这个错。
```

- 解决

```shell
export LC_ALL="en_US.UTF-8"
export LC_CTYPE="en_US.UTF-8"
sudo dpkg-reconfigure locales

说明：
	1. 在终端下依次执行即可
	2. 到第三条命令时，有个弹框，在里面找到en_US.UTF-8，回车即可。
```



## 3.2 MySQL：启动时报错

- 描述

```
service mysql start|stop|restart时报Failed to start mysql.service: Unit mysql.service is masked.错
大概意思就是MySQL服务被标记了，动不了了
```



- 解决

```shell
sudo systemctl unmask mysql.service
```



## 3.3 MySQL：使用http或socket登录时报错



- 描述

```shell
使用http登录MySQL：mysql -h 127.0.0.1 -uroot -p123456
或socket登录：mysql -uroot -p123456
时报Can 't connect to local MySQL server through socket '/tmp/mysql.sock '(2) ";错
报错的路径一定是：'/tmp/mysql.sock '，也可能是'/run/.../mysql.sock '之类的。

总的说，就是没有或找不到mysql.sock文件的错。
```



- 解决

```shell
1. 先看看有没有mysql.sock文件。
	find / -name '*.sock'

2. 有这文件，复制文件的路径，创建一个软连接给它报错那个路径
	ln -s /var/lib/mysql.sock /tmp/mysql.sock
```

要是没有mysql.sock文件，则两条路：

路1

```
重启MySQL服务，让它再生一个mysql.sock文件。
sudo service mysql restart

重启了还没有，那走路2
```



路2

```
重写安装MySQL。

卷铺盖跑路....
```





























