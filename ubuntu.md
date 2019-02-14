# 一、Ubuntu安装VMware提示未找到GCC8.2.0

## 查看系统GCC版本

```bash
gcc --version
```

## 安装gcc8.2.0

```bash
sudo apt install gcc-8
```

## 设置系统默认GCC版本为gcc-8

```bash
sudo rm /usr/bin/gcc
sudo ln -s /usr/bin/gcc-8 /usr/bin/gcc
```

## 再次查看系统gcc版本

```bash
gcc --version
```

# 二、Ubuntu18.04下安装Sublime Text3，并解决不能输入中文的问题

## 安装

```bash
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
sudo apt-get install apt-transport-https
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
sudo apt-get update
sudo apt-get install sublime-text
```

## 解决在Ubuntu下sublime不能输入中文的问题

```bash
git clone https://github.com/lyfeyaj/sublime-text-imfix.git
cd sublime-text-imfix && ./sublime-imfix
```

# 三、查看Ubuntu版本

## 方法一

```bash
cat /etc/issue
```

## 方法二

```bash
sudo lsb_release -a
```

# 四、查看系统CPU核心数

```
grep -c ^processor /proc/cpuinfo
```

# 五、查看和修改ubuntu服务器时区

```bash
date -R  # 查看
sudo dpkg-reconfigure tzdata  # 修改，选择Asia/Shanghai
```

# 六、修改当前用户密码

```bash
passwd 用户名
```
