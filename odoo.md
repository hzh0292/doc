# 一、odoo11百度云ubuntu16.04安装步骤

## 更新系统

```bash
sudo apt update && sudo apt upgrade -y
```

## 新建用户（以odoo为例）

```bash
adduser odoo
```

## 设置权限

```bash
visudo
```
- 添加 odoo ALL=(ALL:ALL) ALL
- Ctrl + O保存，Ctrl + X关闭
## 切换用户

```bash
su odoo
cd
```

## 建立python3虚拟环境（以py3为例）

```bash
sudo apt install -y python3-dev python3-venv
python3 -m venv py3
source py3/bin/activate
pip install -i https://pypi.douban.com/simple -U pip
```

## 安装nodejs和less

```bash
sudo apt-get install -y npm
sudo ln -s /usr/bin/nodejs /usr/bin/node
sudo npm install -g less
```

## 克隆源码

```bash
git clone https://github.com/odoo/odoo.git -b 11.0 --depth=1
```

## 安装PostgreSQL

```bash
sudo apt-get install -y postgresql
```

## 创建数据库角色（以odoo为例）

```bash
sudo su - postgres
createuser -d -U postgres -R -S -P odoo
Enter password for new role: *****
Enter it again:*****
exit
```

## 安装字体等依赖项

```bash
sudo apt-get install -y fontconfig libfontconfig1 libxrender1 libjpeg-turbo8 libfontenc1 libxfont1 x11-common xfonts-75dpi xfonts-base xfonts-encodings xfonts-utils libldap2-dev libsasl2-dev ttf-wqy-zenhei ttf-wqy-microhei
```

## 下载安装wkhtmltopdf

```bash
wget https://downloads.wkhtmltopdf.org/0.12/0.12.5/wkhtmltox_0.12.5-1.xenial_amd64.deb
sudo dpkg -i wkhtmltox_0.12.5-1.xenial_amd64.deb
```

> wkhtmltopdf官方下载很慢，甚至不能下载，建议提前下载好从本地上传

## 安装requiremwnts.txt

```bash
cd odoo
pip install -r requirements.txt -i https://pypi.douban.com/simple
pip install phonenumbers -i https://pypi.douban.com/simple
```

## 启动odoo

```bash
./odoo-bin -s
```

# 二、odoo11 AWS ubuntu18.04安装步骤

## 更新系统

```bash
sudo apt update && sudo apt upgrade -y
```

## 切换到root用户

```bash
sudo -i
```

## 新建用户（以odoo为例）

```bash
adduser odoo
```

## 设置权限

```bash
visudo
```
- 添加 odoo ALL=(ALL:ALL) ALL
- Ctrl + O保存，Ctrl + X关闭
## 切换用户

```bash
su odoo
cd
```

## 建立python3虚拟环境（以py3为例）

```bash
sudo apt install -y python3-dev python3-venv
python3 -m venv venv11
source venv11/bin/activate
pip install -U pip
```

## 安装nodejs和less

```bash
sudo apt-get install -y npm
sudo npm install -g less
```

## 克隆源码

```bash
git clone https://github.com/odoo/odoo.git -b 11.0 --depth=1
```

## 安装PostgreSQL

```bash
sudo apt-get install -y postgresql
```

## 创建数据库角色（以odoo为例）

```bash
sudo su - postgres
createuser -d -U postgres -R -S -P odoo
Enter password for new role: *****
Enter it again:*****
exit
```

## 安装字体等依赖项

```bash
sudo apt-get install -y libxml2-dev libxslt1-dev fontconfig libfontconfig1 libxrender1 libjpeg-turbo8 libfontenc1 libxfont2 x11-common xfonts-75dpi xfonts-base xfonts-encodings xfonts-utils libldap2-dev libsasl2-dev fonts-wqy-zenhei fonts-wqy-microhei
```

## 下载安装wkhtmltopdf

```bash
wget https://downloads.wkhtmltopdf.org/0.12/0.12.5/wkhtmltox_0.12.5-1.bionic_amd64.deb
sudo dpkg -i wkhtmltox_0.12.5-1.bionic_amd64.deb
```

> wkhtmltopdf的源好像就在AWS上，因此不用担心下载速度。

## 安装requiremwnts.txt

```bash
cd odoo
pip install -r requirements.txt
pip install phonenumbers
```

> 在AWS上pip安装就不要用豆瓣源了。

> 如果报错，再检查看缺什么插件，安装后继续。

## 启动odoo

```bash
./odoo-bin -s
```

# 三、创建Odoo数据库时的“new encoding (UTF8) is incompatible with the encoding of the template database (SQL_ASCII)“问题

## Odoo创建数据库时，显示如下错误信息:

> Database creation error: new encoding (UTF8) is incompatible with the encoding of the template database (SQL_ASCII)  
HINT: Use the same encoding as in the template database, or use template0 as template.

## 解决方法：

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

# 四、odoo12 AWS ubuntu18.04安装步骤

## 更新系统

```bash
sudo apt update && sudo apt upgrade -y
```

## 切换到root用户

```bash
sudo -i
```

## 新建用户（以odoo为例）

```bash
adduser odoo
```

## 设置权限

```bash
visudo
```
- 添加 odoo ALL=(ALL:ALL) ALL
- Ctrl + O保存，Ctrl + X关闭
## 切换用户

```bash
su odoo
cd
```

## 建立python3虚拟环境（以py3为例）

```bash
sudo apt install -y python3-dev python3-venv
python3 -m venv venv12
source venv12/bin/activate
pip install -U pip
```

## 安装nodejs和less

```bash
sudo apt-get install -y npm
sudo npm install -g less
```

## 克隆源码

```bash
git clone https://github.com/odoo/odoo.git -b 12.0 --depth=1
```

## 安装PostgreSQL

```bash
sudo apt-get install -y postgresql
```

## 创建数据库角色（以odoo为例）

```bash
sudo su - postgres
createuser -d -U postgres -R -S -P odoo
Enter password for new role: *****
Enter it again:*****
exit
```

## 安装字体等依赖项

```bash
sudo apt-get install -y libxml2-dev libxslt1-dev fontconfig libfontconfig1 libxrender1 libjpeg-turbo8 libfontenc1 libxfont2 x11-common xfonts-75dpi xfonts-base xfonts-encodings xfonts-utils libldap2-dev libsasl2-dev fonts-wqy-zenhei fonts-wqy-microhei
```

## 下载安装wkhtmltopdf

```bash
wget https://downloads.wkhtmltopdf.org/0.12/0.12.5/wkhtmltox_0.12.5-1.bionic_amd64.deb
sudo dpkg -i wkhtmltox_0.12.5-1.bionic_amd64.deb
```

## 安装requiremwnts.txt

```bash
cd odoo
pip install -r requirements.txt
pip install phonenumbers
```

> 如果报错，再检查看缺什么插件，安装后继续。

## 启动odoo

```bash
./odoo-bin -s
```

# 五、odoo12企业版百度云ubuntu16.04安装步骤

## 更新系统

```bash
sudo apt update && sudo apt upgrade -y
```

## 新建用户（以odoo为例）

```bash
adduser odoo
```

## 设置权限

```bash
visudo
```
- 添加 odoo ALL=(ALL:ALL) ALL
- Ctrl + O保存，Ctrl + X关闭
## 切换用户

```bash
su odoo
cd
```

## 建立python3虚拟环境（以py3为例）

```bash
sudo apt install -y python3-dev python3-venv
python3 -m venv py3
source py3/bin/activate
pip install -i https://pypi.douban.com/simple -U pip
```

## 安装nodejs和less

```bash
sudo apt-get install -y npm
sudo ln -s /usr/bin/nodejs /usr/bin/node
sudo npm install -g less
```

## 安装lrzsz和unzip

```bash
sudo apt install -y lrzsz unzip
```

## 上传企业版源码包（以odoo12e.zip为例）

```bash
rz
```

> 在弹出的窗口中选择企业版源码zip压缩包文件，这里是以Xshell和lrzsz为例，也可以使用其它方式将源码上传。

## 解压源码,重命名文件夹为odoo

```bash
unzip odoo12e.zip
mv odoo12e odoo
```

## 安装PostgreSQL

```bash
sudo apt-get install -y postgresql
```

## 创建数据库角色（以odoo为例）

```bash
sudo su - postgres
createuser -d -U postgres -R -S -P odoo
Enter password for new role: *****
Enter it again:*****
exit
```

## 安装字体等依赖项

```bash
sudo apt-get install -y fontconfig libfontconfig1 libxrender1 libjpeg-turbo8 libfontenc1 libxfont1 x11-common xfonts-75dpi xfonts-base xfonts-encodings xfonts-utils libldap2-dev libsasl2-dev ttf-wqy-zenhei ttf-wqy-microhei
```

## 下载安装wkhtmltopdf

```bash
wget https://downloads.wkhtmltopdf.org/0.12/0.12.5/wkhtmltox_0.12.5-1.xenial_amd64.deb
sudo dpkg -i wkhtmltox_0.12.5-1.xenial_amd64.deb
```

> wkhtmltopdf官方下载很慢，甚至不能下载，建议提前下载好从本地上传

## 安装requiremwnts.txt

```bash
cd odoo
pip install -r requirements.txt -i https://pypi.douban.com/simple
pip install phonenumbers -i https://pypi.douban.com/simple
```

## 复制可执行文件并改名为odoo-bin,添加可执行权限

```bash
cp setup/odoo ./odoo-bin
chmod a+x odoo-bin
```

## 启动odoo

```bash
./odoo-bin -s
2018-10-22 08:05:25,559 19717 INFO ? odoo: Odoo version 12.0+e-xxxxxx 
2018-10-22 08:05:25,560 19717 INFO ? odoo: Using configuration file at /home/odoo/.odoorc 
2018-10-22 08:05:25,560 19717 INFO ? odoo: addons paths: ['/home/odoo/.local/share/Odoo/addons/12.0', '/home/odoo/odoo/odoo/addons'] 
2018-10-22 08:05:25,560 19717 INFO ? odoo: database: default@default:default 
2018-10-22 08:05:25,665 19717 INFO ? odoo.addons.base.models.ir_actions_report: Will use the Wkhtmltopdf binary at /usr/local/bin/wkhtmltopdf 
2018-10-22 08:05:25,781 19717 INFO ? odoo.service.server: HTTP service (werkzeug) running on instance-fxwaig44.novalocal:8069 
......
```

## 创建数据库，安装应用......

# 六、ubuntu18.04本地用户odoo12安装步骤（适用于虚拟机、WSL等）

## 更新系统

```bash
sudo apt update && sudo apt upgrade -y
```

## 安装环境依赖

```bash
sudo apt install -y python3-dev python3-venv libxml2-dev libxslt1-dev fontconfig libfontconfig1 libxrender1 libjpeg-turbo8 libfontenc1 libxfont2 x11-common xfonts-75dpi xfonts-base xfonts-encodings xfonts-utils libldap2-dev libsasl2-dev fonts-wqy-zenhei fonts-wqy-microhei postgresql npm
```

## 安装less

```bash
sudo npm install -g less
```

## 创建数据库角色（同系统本地用户名）

```bash
sudo service postgresql start # WSL需要手动启动pg服务
sudo su - postgres
createuser -d -U postgres -R -S -P 角色名
Enter password for new role: *****
Enter it again:*****
exit
```

## 下载安装wkhtmltopdf

```bash
wget https://downloads.wkhtmltopdf.org/0.12/0.12.5/wkhtmltox_0.12.5-1.bionic_amd64.deb
sudo dpkg -i wkhtmltox_0.12.5-1.bionic_amd64.deb
```

## 克隆源码，安装依赖

```bash
git clone https://github.com/odoo/odoo.git -b 12.0 --depth=1
cd odoo
python3 -m venv venv
source venv/bin/activate
pip install -i https://pypi.douban.com/simple -U pip
pip install -r requirements.txt -i https://pypi.douban.com/simple
pip install phonenumbers -i https://pypi.douban.com/simple
```

> 如果报错，再检查看缺什么插件，安装后继续。

## 默认配置启动odoo

```bash
./odoo-bin -s
```

# 七、windows安装odoo11/12/13，默认打不开。

> 从[Odoo Nightly builds](http://nightly.odoo.com)下载的exe安装包，安装后浏览器无法打开127.0.0.1:8069。

## 解决办法：

### 找到ODOO目录下*:\Program Files (x86)\odoo*\python\Lib\_strptime.py文件，在文件导入模块之后添加一行```locale.setlocale(locale.LC_ALL,'en')```保存，然后再重启ODOO服务或重启电脑。
