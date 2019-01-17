# MySQL相关

## 1.MySQL修改默认字符集为UTF-8

+ 用vim打开MySQL配置文件

```bash
sudo vim /etc/mysql/my.cnf
```

+ 在文件中增加如下内容：

```ini
[client]
default-character-set = utf8

[mysqld]
default-storage-engine = INNODB
character-set-server = utf8
collation-server = utf8_general_ci
default-time-zone = '+8:00'
```

+ 保存并退出，重启MySQL服务

```bash
service mysql restart
```

## 2.MySQL创建用户

1. 新建一个用户

```sql
create user "用户名"@"IP地址" identified by "密码";
```

IP地址的表示方式：

+ %表示用户可以从任何地址连接到服务器
+ localhost 用户只能从本地连接
+ 指定一个IP 表示用户只能从此IP连接到服务器

2. 给新建的用户授权

```sql
grant 权限列表 on 库.表 to "用户名"@"IP地址" with grant option;
```

+ 权限列表：```select```,```update```,```delete```,```insert```,```alter```,```drop```,```create```,...如果要授予所的权限则使用ALL
+ 库.表： \*.* 表示所有库的所有表

3. 刷新权限

```sql
flush privileges;
```

4. 修改MySQL配置文件允许数据库远程连接

```bash
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

```ini
#bind-address		= 127.0.0.1		#注释此行
```

5.重启MySQL服务

```bash
sudo service mysql restart
```

## 3.ubuntu18.04上安装mysql后设置root用户密码

+ sudo i登录后修改初始密码

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
```


+ 查看默认debian-sys-maint用户的信息（以下几步是补充方法）

```bash
sudo cat /etc/mysql/debian.cnf
```

+ 复制debian-sys-maint用户密码，登陆MySQL

```bash
mysql -udebian-sys-maint -p刚才复制的密码
```

+ 删除本地用户root,再重新新建一个用户root

```sql
DROP USER 'root'@'localhost';  
create user 'root'@'%' identified by '123456';  
grant all on *.* to "root"@"%" with grant option;
flush privileges;  
quit;
```

+ 重启MySQL服务之后，就可以用root用户和密码登陆了

```bash
sudo service mysql restart
mysql -uroot -p
```

## 4.Windows安装zip格式mysql 8.0.13

+ 下载zip包，解压到指定目录

```
mysql-8.0.13-winx64.zip
D:\Program Files\mysql-8.0.13-winx64
```

+ 添加系统环境变量

```
D:\Program Files\mysql-8.0.13-winx64\bin
```

+ 在安装目录下建立配置文件，内容如下：

```ini
#D:\Program Files\mysql-8.0.13-winx64\my.cnf
[mysql]
default-character-set=utf8
[mysqld]
basedir=D:\Program Files\mysql-8.0.13-winx64
datadir=D:\Program Files\mysql-8.0.13-winx64\data
character-set-server=utf8
```

+ 以管理员身份运行cmd.exe，初始化数据库

```
mysqld --initialize --user=mysql --console
```

> A temporary password is generated for root@localhost:初始密码

+ 安装服务

```
mysqld --install MySQL
```

+ 启动服务

```
net start MySQL
```

+ 以初始密码登陆数据库

```
mysql -u root -p
Enter password:初始密码
```

+ 修改初始密码

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
```

+ 停止服务

```
net stop MySQL
```
