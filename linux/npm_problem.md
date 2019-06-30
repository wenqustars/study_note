# npm问题集
### gitbook install失败
现象：gitbook安装插件时网络连接超时，且访问的IP与当前网络可能没有关系。
```shell
acme@acme-ubuntu:~/Documents/mybooks/flink-book$ gitbook install
info: installing 2 plugins using npm@3.9.2
info:
info: installing plugin "katex"

Error: connect ETIMEDOUT 172.17.1.197:2018
```
**原因：** npm中设置了仓库，而仓库指向的地址当前网络环境不可访问。  
**解决办法：** 删除仓库配置，或替换成可用的仓库环境。  
**操具体操作：** 编辑~/.npmrc删除一下内容  
```shell
registry = http://172.17.1.197:2018
```
