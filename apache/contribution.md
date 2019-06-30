# 开源贡献

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
