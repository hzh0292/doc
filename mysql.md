# MySQL相关

## 1.MySQL修改默认字符集为UTF-8

+ 用vim打开MySQL配置文件

```bash
sudo vim /etc/mysql/my.cnf
```

+ 在文件中增加如下内容：

```bash
[client]
default-character-set = utf8

[mysqld]
default-storage-engine = INNODB
character-set-server = utf8
collation-server = utf8_general_ci
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
+ 库.表： *.* 表示所有库的所有表

3.  刷新权限

```sql
flush privileges;
```

4. 修改MySQL配置文件允许数据库远程连接

```bash
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

```bash
#bind-address		= 127.0.0.1		#注释此行
```

5.重启MySQL服务

```bash
sudo service mysql restart
```

## 3.待更新

DROP USER 'root'@'localhost';  
create user 'root'@'%' indentified by '123456';  
grant all on *.* to "root"@"%" with grant option;  
flush privileges;  
