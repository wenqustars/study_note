# 开源贡献
## 查找合适参与贡献的issue
在 [Flink Jira](https://issues.apache.org/jira/projects/FLINK/issues) 中添加一个bug或者查找一个已知的bug,作为新手最好找标记为`starter`的任务,这些任务比较容易解决，方便初学者熟悉项目和贡献流程。  
如果对贡献流程还不熟悉，可以查看 [starter issues](https://issues.apache.org/jira/issues/?jql=project%20%3D%20FLINK%20AND%20resolution%20%3D%20Unresolved%20AND%20labels%20%3D%20starter%20ORDER%20BY%20priority%20DESC) 列表，否则你也可以查看 [其他问题](https://issues.apache.org/jira/issues/?jql=project%20%3D%20FLINK%20AND%20resolution%20%3D%20Unresolved%20ORDER%20BY%20priority%20DESC) 列表。  
![starter](/pictures/apache/starter.jpg)
## 贡献代码


## git操作
**查看日志**
```shell
git log
```
**本地回滚到某次commit**
```shell
git reset --hard commit-id
```
**拉取官网提交到本地**
```shell
git pull apache_hbase master
```
**将本地提交到 自己的github仓库，此后github的仓库  应该就和官网的仓库一致了**
```shell
git push origin master --force
```
**使用git format-patch生成所需要的patch**
```shell
git format-patch -M master // 当前分支所有超前master的提交
git format-patch -s 4e16 // 某次提交以后的所有patch, --4e16指的是SHA1 ID
git format-patch -1 // 单次提交
git format-patch -3 // 从master往前3个提交的内容，可修改为你想要的数值
git format-patch –n 07fe // -n指patch数，07fe对应提交的名称, 某次提交（含）之前的几次提交
git format-patch -s --root origin // 从origin到指定提交的所有patch
```
**参考地址**
https://git-scm.com/docs/git-format-patch
```shell
git commit -m '[HBASE-22461] A "NullPointerException" could be thrown'
git push origin master
git format-patch -1
```
```shell
mv 0001-HBASE-22461-A-NullPointerException-could-be-thrown.patch HBASE-22461.patch
```
