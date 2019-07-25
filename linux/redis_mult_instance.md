# ubuntu系统redis多实例
## 安装最新的redis版本
```shell
apt-get remove redis-server    # 删除旧版
apt-get autoremove
sudo apt-get install -y python-software-properties
sudo apt-get install software-properties-common
# sudo add-apt-repository ppa:chris-lea/redis-server
sudo add-apt-repository -y ppa:rwky/redis
sudo apt-get update
sudo apt-get install -y redis-server
```
## redis配置文件
```
/etc/init.d/redis-server-------------redis的可执行程序
/etc/redis/redis.conf----------------redis的配置文件
/usr/bin/redis-server---------------redis的自启动文件
```
## redis默认配置的端口号是6379，添加多配置一个6380端口
* 复制redis配置文件

```shell
cp /etc/redis/redis.conf /etc/redis/redis6380.conf
```
* 修改redis6380.conf文件，vim /etc/redis/redis6380.conf

```properties
pidfile /var/run/redis/redis-server6380.pid
port 6380
logfile /var/log/redis/redis-server6380.log
dbfilename dump6380.rdb
bind 0.0.0.0    # 允许其他服务器访问
```
## 添加redis6380执行文件
* 复制redis执行文件

```shell
sudo cp /etc/init.d/redis-server /etc/init.d/redis-server6380
#修改用户所属，否则服务无法正常启动
chown redis:redis redis-server6380
```
* 修改redis-server6380配置文件对应位置

```properties
DAEMON=/usr/bin/redis-server
DAEMON_ARGS=/etc/redis/redis_6380.conf
NAME=redis-server
DESC=redis-server6380

RUNDIR=/var/run/redis
PIDFILE=$RUNDIR/redis-server6380.pid
```
## 重启systemctl
```shell
#如果不重启systemctl，启动启动redis6380时会出现如下错误
#Failed to start .-redis-server6380.service: Unit .-redis -server6380.service not found.
sudo systemctl daemon-reload
```
## 启动redis6380
```shell
sudo service redis-server6380 (start|stop|restart)
```
## 添加redis6380开机启动设置
* 开机启动设置

```shell
sudo update-rc.d redis-server6380 defaults 20    # 开机启动
```
* 取消开启启动设置

```shell
sudo update-rc.d -f redis-server6380 remove
```
* 测试开机启动，redis6380是否运行  

```shell
sudo reboot    # 重启
ps axu | grep redis    # 查询redis进程
```
## 测试连接redis6380
```shell
#语法： redis-cli -h host -p port -a password
#参数说明： -h 服务器地址 -p 端口号 -a 密码
redis-cli -h localhost -p 6380 
```
## redis主从服务配置
* 在`/etc/redis/redis6380.conf`中添加如下配置  

```properties
# 表示6380是6379的从节点
slaveof localhost 6379
```
* 重启服务  

```
#使用sudo service redis-server6380 restart发现主从不生效，可能是环境的问题 
sudo service redis-server6380 stop
sudo service redis-server6380 start
```
--------------------- 
来源：CSDN   
原文：https://blog.csdn.net/tianjiewang/article/details/58385932 
