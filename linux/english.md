# 目录名转化成英文
1.安装需要的软件
```shell
  sudo apt install xdg-user-dirs-gtk
```
2.临时转换系统语言为英文，重启后会自动恢复原值的
```shell
  export LANG=en_US
```
3.执行转换命令，弹出的窗口中会询问是否将目录转化为英文路径，同意并关闭
```shell
  xdg-user-dirs-gtk-update
```
4.转换回系统语言为中文，也可以不执行下面的命令，直接重启也一样的
```shell
  export LANG=zh_CN
```
