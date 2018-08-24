# python、pip相关

## 1.升级pip后出现ImportError: cannot import name main

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

## 2.豆瓣源安装pip插件

```bash
sudo pip install 插件名 -i https://pypi.douban.com/simple
```

## 3.生成和安装requirements.txt依赖

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

## 4.python3虚拟环境操作

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

## 5.python2虚拟环境操作

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

## 6.持续更新中