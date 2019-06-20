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
