# apache2网络配置
## ubuntu apache2 前端静态资源映射配置

1.增加端口映射目录配置，这里在`/var/www/html`目录下增加`dist`文件夹作为前端静态资源存放目录，并在`/etc/apache2`目录下创建端口映射配置文件 `custom.conf`
```shell
sudo mkdir /var/www/html/dist
sudo vim /etc/apache2/custom.conf
```
2.static.conf 设置映射端口，例如8888  
```properties
<VirtualHost *:8888>
    ServerAdmin server
    DocumentRoot /var/www/html/dist
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
3.在`/etc/apache2/apache2.conf`中添加`custom.conf`配置文件的引用  
```shell
sudo vim /etc/apache2/apache2.conf
```
```properties
Include custom.conf
```
4.`/etc/apache2/ports.conf`增加`Listen`端口
```shell
sudo vim /etc/apache2/ports.conf
```
```properties
# If you just change the port or add more ports here, you will likely also
# have to change the VirtualHost statement in
# /etc/apache2/sites-enabled/000-default.conf
Listen 80
Listen 8888  #增加8888端口监听
<IfModule ssl_module>
        Listen 443
</IfModule>

<IfModule mod_gnutls.c>
        Listen 443
</IfModule>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```
5.修改完成，重启apache2服务
```shell
sudo service apache2 reload
```

## Apache2滚动日志配置及参数说明
**apache log格式化参数:**  
```properties
* %a: 远程IP地址
* %A: 本地IP地址
* %B: 已发送的字节数，不包含HTTP头
* %b: CLF格式的已发送字节数量，不包含HTTP头。例如当没有发送数据时，写入‘-’而不是0。
* %{FOOBAR}e: 环境变量FOOBAR的内容
* %f: 文件名字
* %h: 远程主机
* %H 请求的协议
* %Foobar}i: Foobar的内容，发送给服务器的请求的标头行。
* %l: 远程登录名字（来自identd，如提供的话）
* %m: 请求的方法
* %{Foobar}n: 来自另外一个模块的注解“Foobar”的内容
* %{Foobar}o: Foobar的内容，应答的标头行
* %p: 服务器响应请求时使用的端口
* %P: 响应请求的子进程ID。
* %q: 查询字符串（如果存在查询字符串，则包含“?”后面的部分；否则，它是一个空字符串。）
* %r: 请求的第一行
* %s: 状态。对于进行内部重定向的请求，这是指原来请求的状态。如果用%…>s，则是指后来的请求。
* %t: 以公共日志时间格式表示的时间（或称为标准英文格式）
* %{format}t: 以指定格式format表示的时间
* %T: 为响应请求而耗费的时间，以秒计
* %u: 远程用户（来自auth；如果返回状态（%s）是401则可能是伪造的）
* %U: 用户所请求的URL路径
* %v: 响应请求的服务器的ServerName
* %V: 依照UseCanonicalName设置得到的服务器名字
```

**access log**  
```properties
LogFormat “%h %t |%r| %>s %b %T” custom  
CustomLog “|bin/rotatelogs.exe D:/server/apache2/logs/access_%Y-%m-%d.log 86400” custom
```
**error log** 
```properties 
ErrorLog “|bin/rotatelogs.exe D:/server/apache2/logs/error_%Y-%m-%d.log 86400”
```
