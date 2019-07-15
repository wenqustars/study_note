# shell编程

##  shell脚本中针对sudo的密码输入问题解决方案
* 安装expect工具：
```shell
sudo apt-get install tcl tk expect

#! /usr/bin/expect
spawn sudo apt-get update
send "zhoushuo\r"
interact
```
 * 希望手动录入密码的方法：
```shell
#! /bin/bash
sudo apt-get update<<EOF
password
EOF
```
* 简易方式：
```shell
echo "password" | sudo -S 命令，eg：
echo 'zhoushuo' | sudo -S /usr/bin/anydesk
```
