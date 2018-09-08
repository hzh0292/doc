# odoo11百度云ubuntu16.04安装步骤

## 更新系统

```bash
sudo apt update | sudo apt upgrade
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
sudo apt install -y python3-venv
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
Enter password for new role: *****
Enter it again:*****
exit
```

## 安装字体等依赖项

```bash
sudo apt-get install -y fontconfig libfontconfig1 libxrender1 libjpeg-turbo8 libfontenc1 libxfont1 x11-common xfonts-75dpi xfonts-base xfonts-encodings xfonts-utils libldap2-dev libsasl2-dev
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
