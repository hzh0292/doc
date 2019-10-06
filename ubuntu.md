## 一、Ubuntu安装VMware提示未找到GCC8.2.0

### 查看系统GCC版本

```bash
gcc --version
```

### 安装gcc8.2.0

```bash
sudo apt install gcc-8
```

### 设置系统默认GCC版本为gcc-8

```bash
sudo rm /usr/bin/gcc
sudo ln -s /usr/bin/gcc-8 /usr/bin/gcc
```

### 再次查看系统gcc版本

```bash
gcc --version
```

## 二、Ubuntu18.04下安装Sublime Text3，并解决不能输入中文的问题

### 安装

```bash
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
sudo apt-get install apt-transport-https
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
sudo apt-get update
sudo apt-get install sublime-text
```

### 解决在Ubuntu下sublime不能输入中文的问题

```bash
git clone https://github.com/lyfeyaj/sublime-text-imfix.git
cd sublime-text-imfix && ./sublime-imfix
```

### 中文本地化

```bash
git clone -b st3 https://github.com/rexdf/ChineseLocalization.git ~/.config/sublime-text-3/Packages/ChineseLocalization
```
## 三、查看Ubuntu版本

### 方法一

```bash
cat /etc/issue
```

### 方法二

```bash
sudo lsb_release -a
```

## 四、查看系统CPU核心数

```
grep -c ^processor /proc/cpuinfo
```

## 五、查看和修改ubuntu服务器时区

```bash
date -R  # 查看
sudo dpkg-reconfigure tzdata  # 修改，选择Asia/Shanghai
```

## 六、修改当前用户密码

```bash
passwd 用户名
```


## 七、Wkhtmltopdf 失败 (错误代码: -8). 消息: b''

```bash
sudo apt install ttf-mscorefonts-installer
sudo fc-cache -f -v
```

## 八、pycharm打开脚本报错Gtk-Message: Failed to load module "canberra-gtk-module"
解决方法

```bash
sudo apt-get install libcanberra-gtk-module
```

## 九、Skype不支持中文输入

### 添加Skype Linux客户端存储库

```bash
echo "deb [arch=amd64] https://repo.skype.com/deb stable main" | sudo tee /etc/apt/sources.list.d/skype-stable.list
```

### 获取并安装Skype公钥

```bash
wget https://repo.skype.com/data/SKYPE-GPG-KEY 
sudo apt-key add SKYPE-GPG-KEY
```

### 安装apt-transport-https

```bash
sudo apt install apt-transport-https
```

### 安装Skype

```bash
sudo apt update
sudo apt install skypeforlinux
```

### 请使用ibus输入法

## 十、安装Typora

```bash
# or run:
# sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE
wget -qO - https://typora.io/linux/public-key.asc | sudo apt-key add -
# add Typora's repository
sudo add-apt-repository 'deb https://typora.io/linux ./'
sudo apt-get update
# install typora
sudo apt-get install typora
```

## 十一、实用的scp命令

### 下载文件或文件夹

```bash
scp -P 22 -r user@host:/www/wwwroot/ c:/user/aa/
# -P 指定端口，默认为22；-r 目录操作；-i 验证文件登录。
# 示例1，下载rma文件夹到/mnt/d
scp -r user@192.168.100.1:/home/user/rma /mnt/d/
# 示例2，下载README.rst到当前路径(省略下载目的路径则默认为当前工作路径)
scp user@192.168.100.1:/home/user/rma/README.rst .
# 示例3，验证文件登录下载(jeanphy.online代表主机IP或域名)
scp -r -i aws.pem ubuntu@jeanphy.online:/home/ubuntu/online/myaddons/recard
```

### 上传文件及文件夹(类似下载，仅调整文件/目录在主机之前)

```bash
scp -P 22 -r directory user@host:/www/wwwroot/
# 例如(上传默认目录为用户根目录)
scp README.rst user@192.168.100.1:/home/user/rma/  # 上传文件
scp -r src/img user@192.168.100.1:  # 上传目录
```

## 十二、Ubuntu 18.04安装postgresql-11/12

```bash
sudo vim /etc/apt/sources.list.d/pgdg.list
# 添加如下一行内容保存
deb http://apt.postgresql.org/pub/repos/apt/ bionic-pgdg main
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo apt-get update
sudo apt-get install postgresql-11/12
```

## 十三、Ubuntu 18.04更换阿里源

### 备份/etc/apt/sources.list

```bash
cp /etc/apt/sources.list /etc/apt/sources.list.bak
# 在/etc/apt/sources.list文件前面添加如下条目
sudo vim /etc/apt/sources.list
```

```ini
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```

### 最后执行如下命令更新源

```bash
sudo apt-get update
sudo apt-get upgrade
```
