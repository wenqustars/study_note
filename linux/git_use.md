# git使用
## git操作
### 克隆代码
```shell
git clone git@github.com:apache/repositories.git
```
### 添加远程库
```shell
#添加远程仓库，并给远程仓库取别名为 origin
git remote add origin git@github.com:apache/repositories.git
#将本地代码添加到远程仓库
git push -u origin master
```
### 切换分支
```shell 
#切换到远程分支
git checkout -b localbranch origin/remotebranch
#切换到本地分支
git checkout dev
```
### 分支代码合并
```
#将branch上的修改合并到当前分支，branch分支的所有修改在当前分支只会形成一个提交记录
git merge --no-ff branch
#将branch上的修改合并到当前分支，使用--no-ff参数可让当前分支保留branch分支修改的提交记录
git merge --no-ff branch
```

### 查看远程仓库信息
```
git remote
git remote -v
```

### 修改远程仓库地址
```
git remote set-url origin git@wenqustars@163.com/study_note.git

git branch --set-upstream-to=origin/master master  
git branch --set-upstream-to=origin/topic topic  
方法二 remove && add  
git remote rm origin git@git@wenqustars@163.com/  study_note.git  

git remote add origin git@wenqustars@163.com/  study_note.git  
方法三 修改配置文件,直接修改配置文件  
进入study_note/.git  
vim config  
```

### git删除未跟踪文件

```
#删除 untracked files
git clean -f
 
#连 untracked 的目录也一起删掉
git clean -fd
 
#连 gitignore 的untrack 文件/目录也一起删掉 （慎用，一般这个是用来删掉编译出来的 .o之类的文件用的）
git clean -xfd
 
#在用上述 git clean 前，墙裂建议加上 -n 参数来先看看会删掉哪些文件，防止重要文件被误删
git clean -nxfd
git clean -nf
git clean -nfd
```