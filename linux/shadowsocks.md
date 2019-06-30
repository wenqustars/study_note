# SS翻墙服务器的教程
## ubuntu 配置全局代理
linux下的ss+privoxy代理配置
### 安装ss
**直接用pip安装**
```shell
sudo apt-get update
sudo apt-get install python-pip
sudo apt-get install shadowsocks
```
### 服务端启动ss服务器
```shell
cat /etc/shadowsocks/config.json
{
    "server":"0.0.0.0",
    "server_port":8381,           #服务端口，修改项
    "local_address":"127.0.0.1",
    "local_port":1080,
    "password":"leigod",          #访问密码，修改项
    "timeout":300,
    "method":"aes-256-cfb",       #加密方式，修改项
    "fast_open":false
}
#启动服务
ssserver -c /etc/shadowsocks/config.json
```
### ubuntu启动ss客户端
```shell
cat /etc/shadowsocks/config.json
{
    "server":"47.256.171.251",      #ss服务地址，修改项
    "server_port":8381,             #ss服务端口，修改项
    "local_address":"127.0.0.1",
    "local_port":1080,
    "password":"leigod",            #ss服务登录密码，修改项
    "timeout":300,
    "method":"aes-256-cfb",         #ss服务加密方式，修改项
    "fast_open":false
}
sudo sslocal -c /etc/shadowsocks/config.json
```

### 使用genpac生成代理软件配置文件 【可选操作】
***genpac是基于gfwlist的多种代理软件配置文件生成工具，支持自定义规则，目前可生成的格式有pac, dnsmasq, wingy***
```shell
sudo pip install genpac
```
在Documents文件夹下创建名为shadowsocks文件夹，并进入该文件夹
```shell
mkdir Documents/shadowsocks && cd Documents/shadowsocks
```
+ 在终端执行下面指令：
```shell
sudo genpac --proxy="SOCKS5 127.0.0.1:1080" --gfwlist-proxy="SOCKS5 127.0.0.1:1080" -o autoproxy.pac --gfwlist-url="https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt"
```
+ 如果上面指令执行失败，执行下面指令，没有失败则跳过下面指令：
```shell
sudo genpac --proxy="SOCKS5 127.0.0.1:1080" -o autoproxy.pac --gfwlist-url="https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt"
```
对应的会在shadowsocks文件夹下生成一个文件“autoproxy.pac”

* 打开“系统设置” 的“网络”，点击“网络代理” 选择“自动”，在对应的URL里面填入
```txt
file:///home/xxx/Documents/shadowsocks/autoproxy.pac
```
+ 根据自身用户名修改URL输入，点击“apply system wide”，然后输入密码。


### ubuntu安装privoxy
```shell
sudo apt install privoxy
```
### 修改privoxy配置
```shell
vi /etc/privoxy/config
listen-address 127.0.0.1:8118  // 783行，去掉前面的注释符号，端口可以随便改
forward-socks5t / 127.0.0.1:1080  //1336行，去掉前面的注释符号，后面的1080端口要对应ss服务里面的配置，要一致
```
### 设置环境后启动privoxy
```shell
vim /etc/profile
#添加以下配置
export http_proxy=http://127.0.0.1:8118
export https_proxy=http://127.0.0.1:8118
export ftp_proxy=http://127.0.0.1:8118
#让/etc/profile修改的内容生效
source /etc/profile
#启动代理服务
sudo systemctl restart privoxy
```
### 测试
```shell
~$ wget www.baidu.com
--2018-07-21 17:07:56--  http://www.baidu.com/
正在连接 127.0.0.1:8118... 已连接。
已发出 Proxy 请求，正在等待回应... 200 OK
长度： 2381 (2.3K) [text/html]
正在保存至: “index.html”

index.html          100%[===================>]   2.33K  --.-KB/s    in 0s

2018-07-21 17:07:56 (318 MB/s) - 已保存 “index.html” [2381/2381])

~$
```
