# Ubuntu安装VMware提示未找到GCC8.2.0

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

