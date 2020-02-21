## 一、odoo访问远程pg数据库

### 数据库服务器设置：

#### 1.修改```postgresql.conf```文件

```bash
sudo vim /etc/postgresql/10/main/postgresql.conf
```

##### 修改```listen_addresses```为```*```或者特定IP地址

```ini
#listen_addresses = 'localhost'         # what IP address(es) to listen on;
listen_addresses = '*'                  # what IP address(es) to listen on;
```

#### 2.修改```pg_hba.conf```文件

```bash
sudo vim /etc/postgresql/10/main/pg_hba.conf
```

##### 添加IPv4连接```0.0.0.0/0```或者特定IP地址

```ini
# IPv4 local connections:
host    all             all             127.0.0.1/32            md5
host    all             all             0.0.0.0/0               md5
```

#### 3.重启pg服务生效

```bash
sudo service postgresql restart
```

### odoo服务器设置：

#### 修改```odoo.conf```或```.odoorc```配置文件

```ini
db_host = False　　    # 这里填写数据库ip地址
db_port = False　　    # 这是数据库的端口
db_user = odoo　　     # 这里填写数据库登陆账号
db_password = False　　# 这里填写数据库登陆密码
```

## 二、Ubuntu 18.04安装Odoo 12/13步骤

+ 更换阿里源（可选）
	
  + 备份/etc/apt/sources.list
  
  ```bash
  cp /etc/apt/sources.list /etc/apt/sources.list.bak
  ```
  
  + 替换阿里源
  
  ```bash
  sudo sed -i "s/archive.ubuntu.com/mirrors.aliyun.com/g;s/security.ubuntu.com/mirrors.aliyun.com/g" /etc/apt/sources.list
  ```
  
+ 更新系统（可选）

```bash
sudo apt update && sudo apt upgrade -y
```

+ 更换Python的pip源（可选）

  + 在主目录下创建.pip文件夹,然后在该目录下创建pip.conf文件
  
  ```bash
  mkdir ~/.pip
  vim ~/.pip/pip.conf
  ```
  
  + pip.conf文件编写如下内容保存（更换为阿里源）：
  
  ```ini
  [global]
  index-url = https://mirrors.aliyun.com/pypi/simple/
  
  [install]
  trusted-host=mirrors.aliyun.com
  ```
	
+ 添加libpng12-0存储库，wkhtmltopdf将使用此依赖

```bash
sudo add-apt-repository universe
sudo add-apt-repository "deb http://mirrors.aliyun.com/ubuntu/ xenial main"
sudo apt update && sudo apt upgrade -y
```

+ 安装PostgreSQL-11/12数据库（可选，直接sudo apt install postgresql安装的是10版本）

```bash
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo add-apt-repository "deb http://apt.postgresql.org/pub/repos/apt/ bionic-pgdg main"
sudo apt update
sudo apt install postgresql-11 -y
# sudo apt install postgresql-12 -y
```

+ 创建用户（可选，不建议用root用户创建服务）

  + 切换到root用户

  ```bash
  sudo -i
  ```

  + 新建用户（此处以odoo用户名为例）

  ```bash
  adduser odoo
  ```

  + 设置权限

  ```bash
  visudo
  ```
  
  > 添加 odoo ALL=(ALL:ALL) ALL
  > Ctrl + O保存，Ctrl + X关闭

  + 切换用户

  ```bash
  su odoo
  cd
  ```

+ 创建数据库角色（此处以odoo用户名为例）

```bash
sudo su - postgres
createuser -d -U postgres -R -S -P odoo
exit
```

+ 安装 Python 3 （odoo13需3.6及以上版本）+ pip3

```bash
sudo apt install git python3 python3-pip build-essential wget python3-dev python3-wheel libxslt-dev libzip-dev libldap2-dev libsasl2-dev python3-setuptools -y
```

+ 安装依赖包
```bash
sudo pip3 install -r https://github.com/odoo/odoo/raw/13.0/requirements.txt
# sudo pip3 install -r https://github.com/odoo/odoo/raw/12.0/requirements.txt
```
+ 下载安装wkhtmltopdf（建议提前下载好）

```bash
wget https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.5/wkhtmltox_0.12.5-1.trusty_amd64.deb
sudo dpkg -i wkhtmltox_0.12.5-1.trusty_amd64.deb
sudo apt --fix-broken install
sudo ln -s /usr/local/bin/wkhtmltopdf /usr/bin
sudo ln -s /usr/local/bin/wkhtmltoimage /usr/bin
```

+ 安装中文字体

```bash
sudo apt install fonts-wqy-zenhei fonts-wqy-microhei -y
```

+ 克隆代码（可选，也可在[Odoo Nightly builds](http://nightly.odoo.com/13.0/nightly/src/)下载源码包或使用已有源码包解压，假设解压路径为/opt）

```bash
git clone https://www.github.com/odoo/odoo --depth 1 -b 13.0
# git clone https://www.github.com/odoo/odoo --depth 1 -b 12.0
sudo pip3 install num2words ofxparse phonenumbers
```

+ 创建odoo配置文件（假设放在/etc）

```bash
sudo wget -P /etc https://github.com/odoo/odoo/raw/13.0/debian/odoo.conf
# sudo wget -P /etc https://github.com/odoo/odoo/raw/12.0/debian/odoo.conf
```

+ 打开配置文件新增或编辑各项参数：

```ini
[options]
; This is the password that allows database operations:
; admin_passwd = admin
db_host = False
db_port = False
db_user = odoo
db_password = False
addons_path = /opt/odoo/odoo/addons,/opt/odoo/myaddons
```

> addons_path是必须设置的，设置为odoo源码包addons目录以及附加模块（如企业版模块）和第三方模块及自定义开发模块myaddons，逗号分隔。
> 常用其他参数有http_port指定端口，db_name或dbfilter指定数据库。

+ 创建systemd启动单元

	+ 创建服务文件
  
  ```bash
  sudo vim /etc/systemd/system/odoo.service
  ```
  
	+ 文件内容如下：
  
	```ini
	[Unit]
	Description=Odoo
	Requires=postgresql.service
	After=network.target postgresql.service
	
	[Service]
	Type=simple
	SyslogIdentifier=odoo
	PermissionsStartOnly=true
	User=odoo
	Group=odoo
	ExecStart=/usr/bin/python3 /opt/odoo/odoo-bin -c /etc/odoo.conf
	StandardOutput=journal+console
	Restart=always
	RestartSec=5
	StartLimitInterval=0
	
	[Install]
	WantedBy=multi-user.target
	```
  
	+ 重新加载systemd守护程序，然后启动odoo服务
  
  ```bash
  sudo systemctl daemon-reload
  sudo systemctl start odoo
  ```
  
	+ 设置系统启动时启动odoo
  
  ```bash
  sudo systemctl enable odoo
  ```

## 三、创建Odoo数据库时的“new encoding (UTF8) is incompatible with the encoding of the template database (SQL_ASCII)“问题

### Odoo创建数据库时，显示如下错误信息:

> Database creation error: new encoding (UTF8) is incompatible with the encoding of the template database (SQL_ASCII)  
HINT: Use the same encoding as in the template database, or use template0 as template.

### 解决方法：

1. First, we need to drop template1. Templates can’t be dropped, so we first modify it so t’s an ordinary database:

```sql
UPDATE pg_database SET datistemplate = FALSE WHERE datname = 'template1';
```

2. Now we can drop it:

```sql
DROP DATABASE template1;
```

3. Now its time to create database from template0, with a new default encoding:

```sql
CREATE DATABASE template1 WITH TEMPLATE = template0 ENCODING = 'UNICODE';
```

4. Now modify template1 so it’s actually a template:

```sql
UPDATE pg_database SET datistemplate = TRUE WHERE datname = 'template1';
```

5. Now switch to template1 and VACUUM FREEZE the template:

```sql
\c template1
VACUUM FREEZE;
```

6. Problem should be resolved.

## 四、windows安装odoo11/12/13，默认打不开。

> 从[Odoo Nightly builds](http://nightly.odoo.com)下载的exe安装包，安装后浏览器无法打开127.0.0.1:8069。

### 解决办法：

#### 找到odoo安装目录下python\\Lib\\_strptime.py文件，在文件导入模块之后添加一行```locale.setlocale(locale.LC_ALL,'en')```保存，然后再重启odoo服务或重启电脑。

## 五、Wkhtmltopdf 失败 (错误代码: -8). 消息: b''

```bash
sudo apt install ttf-mscorefonts-installer
sudo fc-cache -f -v
```

## 六、登记付款提示“会计凭证不属同一会计科目”

> Odoo12科目表中默认的 “222100 Tax payable” 类型是“应付”，我们需要把它修改成“流动负债”。

## 七、odoo数据库备份和恢复

> odoo的数据库包括数据文件和filestore文件存储两部分，因此备份时两部分都需要备份。

### 备份

```bash
cd && pg_dump -Fc -f dbname.dump dbname
#比如demo数据库
#cd && pg_dump -Fc -f demo.dump demo
tar cjf dbname.tgz dbname.dump .local/share/Odoo/filestore/dbname
#tar cjf demo.tgz demo.dump .local/share/Odoo/filestore/demo
```

### 恢复

```bash
tar xf dbname.tgz
#tar xf demo.tgz
pg_restore -C -d dbname dbname.dump
#pg_restore -C -d demo demo.dump
```
