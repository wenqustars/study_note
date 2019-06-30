# ElasticSearch Restfull接口
## 集群操作
### 查询elastic服务健康状态
```txt
GET http://host:9200/_cat/health?v
```
### 查询elastic集群节点信息列表
```txt
GET http://host:9200/_cat/nodes?v
```
## 索引操作
### 查询elastic集群的索引列表
```txt
GET http://host:9200/_cat/indices?v
```
### 创建customer索引
```txt
PUT http://host:9200/customer?pretty
```
### 删除elastic索引
```txt
DELETE http://host:9200/customer?pretty
```
## 数据操作
### 向customer索引中添加数据或修改数据
**带主键添加或修改数据**
```txt
PUT http://host:9200/customer/external/1?pretty
{
  "name": "John Doe"
}

customer 表示索引（index）
external 表示索引类型（type）
1 表示添加数据的主键（primary key）
```
**不带主键添加数据**
```txt
POST http://host:9200/customer/external?pretty
{
  "name": "Jane Doe"
}
```
## 查询elastic数据
```txt
GET http://host:9200/customer/external/1?pretty
```

## 文档操作
### 通过修改`name`属性更新文档
```txt
POST http://host:9200/customer/external/1/_update?pretty
{
  "doc": { "name": "Jane Doe" }
}
```
### 通过修改`name`属性更新文档并增加`age`属性
```txt
POST http://host:9200/customer/external/1/_update?pretty
{
  "doc": { "name": "Jane Doe", "age": 20 }
}
```
### 通过脚本的方式修改文档
将年龄添加5
```txt
POST http://host:9200/customer/external/1/_update?pretty
{
  "script" : "ctx._source.age += 5"
}
```
### 删除文档
```txt
DELETE http://host:9200/customer/external/2?pretty
```
## 批量操作elastic
### 同时添加两条数据
```txt
POST http://host:9200/customer/external/_bulk?pretty
{"index":{"_id":"1"}}
{"name": "John Doe" }
{"index":{"_id":"2"}}
{"name": "Jane Doe" }
```
### 同时修改一条数据并删除一条数据
```txt
POST http://host:9200/customer/external/_bulk?pretty
{"update":{"_id":"1"}}
{"doc": { "name": "John Doe becomes Jane Doe" } }
{"delete":{"_id":"2"}}
```
### 引入数据文件
```txt
curl -XPOST 'http://host:9200/bank/account/_bulk?pretty&refresh' --data-binary "@accounts.json"
```
### accounts.json文件样例数据
```json
{"index":{"_id":"1"}}
{"account_number":1,"balance":39225,"firstname":"Amber","lastname":"Duke","age":32,"gender":"M","address":"880 Holmes Lane","employer":"Pyrami","email":"amberduke@pyrami.com","city":"Brogan","state":"IL"}
{"index":{"_id":"6"}}
{"account_number":6,"balance":5686,"firstname":"Hattie","lastname":"Bond","age":36,"gender":"M","address":"671 Bristol Street","employer":"Netagy","email":"hattiebond@netagy.com","city":"Dante","state":"TN"}
{"index":{"_id":"13"}}
{"account_number":13,"balance":32838,"firstname":"Nanette","lastname":"Bates","age":28,"gender":"F","address":"789 Madison Street","employer":"Quility","email":"nanettebates@quility.com","city":"Nogal","state":"VA"}
{"index":{"_id":"18"}}
{"account_number":18,"balance":4180,"firstname":"Dale","lastname":"Adams","age":33,"gender":"M","address":"467 Hutchinson Court","employer":"Boink","email":"daleadams@boink.com","city":"Orick","state":"MD"}
{"index":{"_id":"20"}}
{"account_number":20,"balance":16418,"firstname":"Elinor","lastname":"Ratliff","age":36,"gender":"M","address":"282 Kings Place","employer":"Scentric","email":"elinorratliff@scentric.com","city":"Ribera","state":"WA"}
```

## 数据查询接口
```txt
GET http://host:9200/customer/_search?q=*&sort=account_number:asc&pretty
```
```txt
返回的响应内容：

took - Elasticsearch执行搜索的时间（以毫秒为单位）
timed_out - 告诉我们搜索是否超时
_shards - 告诉我们搜索了多少个分片，以及搜索成功/失败分片的计数
hits - 搜索结果
hits.total  - 符合我们搜索条件的文档总数
hits.hits  - 搜索结果的实际数组（默认为前10个文档）
sort  - 对结果进行排序（如果按分数排序则丢失）
_score和max_score - 暂时忽略这些字段
```
```txt
GET http://host:9200/customer/_search
{
  "query": { "match_all": {} },
  "sort": [
    { "account_number": "asc" }
  ]
}
```
