# 1 系统安装



# 2 软件安装



## 2.1 安装Redis

- 安装

```
- 更新安装源 
	sudo apt update
	
- 安装
	sudo apt install redis-server
```

- 常见配置

```
- 1 管理Redis为服务
    sudo gedit /etc/redis/redis.conf

    搜索supervised
    原来是：supervised no
    改成：supervised systemd

- 2 支持远程连接
	sudo gedit /etc/redis/redis.conf
	将 bind 127.0.0.1 ::1 
	改为 bind 0.0.0.0

- 3 给Redis设置密码
	sudo gedit /etc/redis/redis.conf
	搜索 requirepass
	默认是被注释的：# requirepass foobared
	改成：requirepass 123456
	后面的就是密码
	
说明：
	1. 改过配置后都得重启一些服务，才能使配置生效。
```

- 基本操作

1. 启动服务

```
sudo service redis start
```

2. 关闭服务

```
udo service redis stop
```

3. 重启服务

```
sudo service redis restart
```

4. 客户端连接

```
redis-cli
```

5. 查看redis服务状态

```
sudo systemctl status redis

说明：
	按ctrl + c退出
```



## 2.2 安装MySQL5.7



### 2.2.1 安装

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install mysql-server
```



### 2.2.2  登录

有的系统第一次安装就会提示你，给root用户设置一个密码。

啥也没提示，直接就安装结束了，则可以到：**/etc/mysql/debian.cnf** 查看用户账号和密码。

user和password都写里面了。

```
mysql -u[用户名] -p[密码]
```



### 2.2.3 修改root密码

- 登录MySQL

```shell
mysql -uroot -p123456

说明：
	1. 修改root的密码就这流程，要是第一次，不知道root的密码，就用/etc/mysql/debian.cnf里面的用户信息去登录。
```

- 修改

```mysql
use mysql;
update mysql.user set authentication_string=password('你的密码') where user='root' and Host='localhost';

说明：
	1. 如果设置的密码太弱了，会提示一些信息，重新设置个强的。
	2. 要是就是像要设置弱的密码，你可以：
		- 看看mysql的密码规则
			show variables like 'validate_password%';
		- 把密码规则改成低安全的
			set global validate_password_policy=LOW;
		- 修改密码的长度
			set global validate_password_length=5;
		- 修改密码
			update mysql.user set authentication_string=password('11111') where user='root' and Host='localhost';
		- 依次执行一下，使设置的密码生效
			update user set plugin="mysql_native_password"; 
			flush privileges;
			quit;
		- 再登录就成了。
	3. 但一般都不建议改人家的规则，咱遵循就好了。
```



## 2.3 远程登录



- 设置用户账号可以访问的主机

```mysql
UPDATE mysql.user SET host="%" WHERE user="root";

说明：
	1. 即root用户可以在任意的host上面登录。
```

- 修改mysql配置文件

```shell
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

在文件里面搜索：bind-address

把：bind-addrees   = 127.0.0.1

改成：bind-addrees   = 0.0.0.0



- 允许端口通过防火墙

如果防火墙状态是未开启的，那就不用这一步了，跳过。

不过建议把防火墙开起来，多学点好。

```shell
- 查看防火墙状态
	sudo ufw status

- 允许端口通过防火墙
	sudo ufw allow 3306/tcp

- 重启防火墙
    sudo ufw disable
    sudo ufw enable
    或
    sudo ufw restart
```

走到这里，root的用户就可以在其他的主机远程登录msyql了。

当让主机和服务器之间网络得通。



## 2.4 安装中文输入法



https://blog.csdn.net/Chamico/article/details/89788324





# 3 库&插件安装



## 3.1 python3

### 3.1.1 python3：安装mysqlclient

```shell
sudo apt-get install libmysqlclient-dev
pip3 install mysqlclient
```



### 3.1.2 python3:安装pip3

```shell
sudo apt install python3-pip
```

