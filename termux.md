# Termux - 安卓终端模拟器资料整理

## 一、设置存储让终端可以读取手机文件

```bash
$ termux-setup-storage
```

## 二、升级0.66版本后快捷键如何恢复两排

```bash
mkdir $HOME/.termux/ ;echo "extra-keys = [['ESC','/','-','HOME','UP','END','PGUP'],['TAB','CTRL','ALT','LEFT','DOWN','RIGHT','PGDN']]" >> $HOME/.termux/termux.properties
exit
```

## 三、设置Termux欢迎语

```bash
vim $PREFIX/etc/motd
```

## 四、vim中文字符乱码

```bash
vim .vimrc
set encoding=utf-8
set fileencodings=utf-8
```
