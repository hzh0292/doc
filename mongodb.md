## 一、mongodb远程连接配置 

### mongodb远程连接配置如下：
#### 1.修改配置文件mongodb.conf

```bash
sudo vim /etc/mongodb.conf
```

> 把bind_ip=127.0.0.1 这一行修改成 bind_ip=0.0.0.0
 
#### 2.重启mongodb服务

```bash
sudo /etc/init.d/mongodb restart
或
sudo service mongodb restart
```

#### 3.防火墙开放27017端口

```bash
iptables -A INPUT -p tcp -m state --state NEW -m tcp --dport 27017 -j ACCEPT
```

#### 4.远程连接
要连接的IP：134.567.345.23
命令：mongo 134.567.345.23:27017
这样就可以连接到134.567.345.23的mongodb/test的数据库
 
#### 5.补充：连接到自定义的用户
##### 1.增加用户

```sql
use admin
```
switched to db admin

```sql
db.addUser('username','password')
```

##### 2.远程连接

```bash
mongo 134.567.345.23:27017/admin -uusername -p
```
