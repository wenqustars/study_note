# jenkins使用教程
## jenkins安装
* 必备条件：java环境
### Debian/Ubuntu
* 安装
```shell
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install jenkins
```
* 配置jenkins服务端口
```shell
vim  /etc/default/jenkins
HTTP_PORT=8070
```
* 启动jenkis
```shell
sudo  /etc/init.d/jenkins [start|restart|stop]
```
* 查看日志
```shell
cat  /var/log/jenkins/jenkins.log
```
 * 注意事项
   * 需要创建`jenkins`用户来运行此服务  

### Centos
* 安装
```shell
wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
yum install -y jenkins
```
* 配置jenkins服务端口
```shell
vim  /etc/sysconfig/jenkins
JENKINS_PORT="8070"
```
* 启动jenkins服务
```
service jenkins start/stop/restart
```
 * 注意事项
   * 安装成功后Jenkins将作为一个守护进程随系统启动
   * 系统会创建一个“jenkins”用户来允许这个服务，如果改变服务所有者，同时需要修改/var/log/jenkins, /var/lib/jenkins, 和/var/cache/jenkins的所有者
   * 启动的时候将从/etc/sysconfig/jenkins获取配置参数
   * 默认情况下，Jenkins运行在8080端口，在浏览器中直接访问该端进行服务配置
   * Jenkins的RPM仓库配置被加到/etc/yum.repos.d/jenkins.repo

## 配置jenkins
在浏览器中访问 :http://jenkinshost:8070/  
首次进入会要求输入初始密码如下图， 
初始密码在：/var/lib/jenkins/secrets/initialAdminPassword 
![jenkins_1](/pictures/develop_tools/jenkins/jenkins_1.png)  
选择“Install suggested plugins”安装默认的插件，下面Jenkins就会自己去下载相关的插件进行安装。  
![jenkins_2](/pictures/develop_tools/jenkins/jenkins_2.png)   
![jenkins_3](/pictures/develop_tools/jenkins/jenkins_3.png)    
创建超级管理员账号   
![jenkins_4](/pictures/develop_tools/jenkins/jenkins_4.png)  
![jenkins_5](/pictures/develop_tools/jenkins/jenkins_5.png)
## 新建任务
![jenkins_6](/pictures/develop_tools/jenkins/jenkins_6.png)  
![jenkins_7](/pictures/develop_tools/jenkins/jenkins_7.png)  
![jenkins_8](/pictures/develop_tools/jenkins/jenkins_8.png)  
![jenkins_9](/pictures/develop_tools/jenkins/jenkins_9.png)  
![jenkins_10](/pictures/develop_tools/jenkins/jenkins_10.png)  

## jenkins问题
### 自动杀掉衍生进程
现象：在使用jenkins进行自动化部署服务的过程中，发现调用服务器的shell命令无法正常启动tomcat，但是构建日志显示是成功执行的，而手动在服务器却是可以正常启动tomcat。    
原因：jenkins默认在build结束后会kill掉所有的衍生进程  
```txt
Jenkins提供了hudson.util.ProcessTree.disable和hudson.util.ProcessTreeKiller.disable两个属性来控制些特性，值为true将禁用此特性。hudson.util.ProcessTree.disable从Jenkins 1.260开始使用，而使用1.315之前的Hudson时只能使用hudson.util.ProcessTreeKiller.disable，为了版本兼容，在Jenkins 1.260后这两个属性都可能使用，建议使用1.260之的Jenkins用户使用hudson.util.ProcessTree.disable属性。  
```
* 启动jenkins时添加配置
  *  java -Dhudson.util.ProcessTree.disable=true -jar jenkins.war
  *  修改/etc/sysconfig/jenkins配置，在JENKINS_JAVA_OPTIONS中加入-Dhudson.util.ProcessTree.disable=true。需要重启jenkins生效
* 后台进程前添加配置
  * BUILD_ID=dontKillMe nohup npm start >/var/app/nohup.out 2>&1 &
  * 在 execute shell输入框中加入BUILD_ID=DONTKILLME,即可防止jenkins杀死启动的进程  

![jenkins_11](/pictures/develop_tools/jenkins/jenkins_11.png)  
```shell
#!/bin/bash
BUILD_ID=dontKillMe nohup java -jar test.war

```
## 参考文档
[安装Jenkins](https://jenkins.io/zh/doc/book/installing/#debianubuntu)  
[ubuntu 下搭建 Jenkins 并配置部署环境](https://www.cnblogs.com/shuoer/p/9471839.html)  
[centos下搭建Jenkins持续集成环境(安装jenkins)](https://www.cnblogs.com/loveyouyou616/p/8714544.html)  
[jenkins自动杀掉衍生进程怎么解决](https://blog.csdn.net/baidu_35140444/article/details/82981531)  
