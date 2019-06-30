# npm使用

## 环境配置
### 设置仓库
#### 添加仓库
**通过config命令**
```shell
npm config set registry https://registry.npm.taobao.org
npm config list #查看npm当前配置
```
**命令行指定,每次执行命令前加入–registry指定仓库路径**
```shell
npm --registry https://registry.npm.taobao.org install
```
**编辑 ~/.npmrc 加入下面内容**
```shell
registry = https://registry.npm.taobao.org
```
#### 删除仓库
**通过config命令**
```shell
npm config delete registry
npm config list #查看npm当前配置
```
**编辑 ~/.npmrc 删除下面内容**
```shell
registry = https://registry.npm.taobao.org
```
