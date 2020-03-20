# python、pip相关

## 1.pyenv管理python版本和虚拟环境

1. 克隆项目

```bash
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```

2. 配置环境变量

```bash
# bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n eval "$(pyenv init -)"\nfi' >> ~/.bashrc
# zsh
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.zshrc
```

3. 重新初始化shell环境

```bash
exec $SHELL
```

4. 安装 pyenv-virtualenv

```bash
git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv
```

5. 配置环境变量

```bash
# bash
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bashrc
# zsh
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshrc
```

6. 重新初始化shell环境

```bash
exec $SHELL
```

7. 安装python 3.7.7

```bash
pyenv install 3.7.7
```

8. 如果安装失败，安装相应的依赖后重试

```bash
# Ubuntu/Debian/Mint
sudo apt-get update; sudo apt-get install --no-install-recommends make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
# CentOS/Fedora 21 and below
yum install gcc zlib-devel bzip2 bzip2-devel readline-devel sqlite sqlite-devel openssl-devel tk-devel libffi-devel
# Fedora 22 and above
dnf install make gcc zlib-devel bzip2 bzip2-devel readline-devel sqlite sqlite-devel openssl-devel tk-devel libffi-devel
```

> 更多请参考pyenv[官方文档](https://github.com/pyenv/pyenv/wiki)

9. 创建、激活、删除虚拟环境

```bash
pyenv virtualenv 3.7.7 venv
pyenv activate venv
# 取消激活
pyenv deactivate
# 删除
pyenv uninstall venv
```

10. 其他命令

```bash
# 查看所有已安装的python环境
pyenv versions
# 查看所有可安装的python环境
pyenv install [Tab键]
# 为test目录设置默认python环境
cd ~/test && pyenv local venv
# 设置全局环境
pyenv global 3.7.7 # 设置3.7.7为全局python环境
# 为当前shell指定python版本
pyenv shell 3.7.7
# 取消当前shell的python版本
unset PYENV_VERSION
```

## 2.升级pip后出现ImportError: cannot import name main

解决办法：  
用vim打开pip文件

```bash
sudo vim /usr/bin/pip
```

把如下内容： 

```python
from pip import main
if __name__ == '__main__':
    sys.exit(main())
```

修改为：

```python
from pip import __main__
if __name__ == '__main__':
    sys.exit(__main__._main())
```

## 3.豆瓣源安装pip插件

```bash
sudo pip install 插件名 -i https://pypi.douban.com/simple
```

## 4.生成和安装requirements.txt依赖

+ 生成requirements.txt

```bash
pip freeze > requirements.txt
```

+ 安装requirements.txt依赖

```bash
pip install -r requirements.txt
```

```bash
# 使用豆瓣源
pip install -r requirements.txt -i https://pypi.douban.com/simple
```

## 5.python3虚拟环境操作

1. 安装python3-venv

```bash
sudo apt-get install python3-venv
```

2. 创建虚拟环境

```bash
python3 -m venv 虚拟环境名
```

3. 激活虚拟环境

```bash
source 虚拟环境名/bin/activate
```

4. 退出虚拟环境

```bash
deactivate
```

## 6.python2虚拟环境操作

1. 安装virtualenv

```bash
sudo apt install virtualenv
```

2. 创建虚拟环境

```bash
virtualenv 虚拟环境名
```

3. 激活虚拟环境

```bash
source 虚拟环境名/bin/activate
```

4. 退出虚拟环境

```bash
deactivate
```

## 7.安装python3.7报错```No module named '_ctypes'```

1、安装libffi-dev

```bash
sudo apt install libffi-dev
```

2、安装其他所需组件

```bash
sudo apt-get install libbz2-dev libreadline-dev libsqlite3-dev
```

## 8.更换pip为豆瓣源

1、在主目录下创建.pip文件夹,然后在该目录下创建pip.conf文件

```bash
mkdir ~/.pip
vim ~/.pip/pip.conf
```

2、pip.conf文件编写如下内容，保存退出即可

```ini
[global]
index-url = https://pypi.douban.com/simple 
```
