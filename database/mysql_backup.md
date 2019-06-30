# mysql备份
## 数据库备份脚本
script_env.sh
```shell
#!/usr/bin/env bash
export JAVA_HOME=/opt/jdk1.8.0_192
export JRE_HOME=${JAVA_HOME}/jre
```
## 数据库备份脚本
dbName_backup.sh
```shell
#!/bin/sh
# Database info
DB_USER="username"
DB_PASS="password"
DB_HOST="localhost"
DB_PORT="3306"
DB_NAME="dbName"

# Others vars
BIN_DIR="/usr/bin"            #the mysql bin path
BCK_DIR="/home/sqlbak/dumps"    #the backup file directory
DATE=`date +%F`

# TODO
# /usr/bin/mysqldump --opt -ubatsing -pbatsingpw -hlocalhost timepusher > /mnt/mysqlBackup/db_`date +%F`.sql
${BIN_DIR}/mysqldump --opt -u${DB_USER} -p${DB_PASS} -h${DB_HOST} -P${DB_PORT} ${DB_NAME} > ${BCK_DIR}/db_${DB_NAME}_${DATE}.sql
```
## 备份文件清理脚本
del_dumps.sh
```
#!/bin/sh
find /home/acme_program/selfbin/sqlbak/dumps -mtime +5 -name "db_*.*" -exec rm -rf {} \;
```

## 配置定时执行
执行命令打开crontab编辑页面
```shell
crontab -e
```
将以下配置写入文件
```shell
* 0 * * * sh /home/sqlbak/bin/dbName_backup.sh
* 0 * * * sh /home/sqlbak/bin/del_dumps.sh
```
查看配置是否生效
```shell
crontab -e
```
