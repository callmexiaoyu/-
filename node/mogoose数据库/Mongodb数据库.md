# Mongodb数据库
***
[TOC]
***
## 简介
类似json数据结构
***
### 安装
+ 默认安装路径
`C:\Program Files\MongoDB\Server\3.6\`
+ 配置环境变量
添加变量名
`MONGODB_HOME=C:\developer\MongoDB\Server\3.6\bin`
添加路径
`Path=%MONGODB_HOME%`
***
### MongoDB Compass
Mongodb数据库的可视化工具，默认自带安装缓慢
***
## cmd数据库服务操作
***
###  数据库开启关闭
+ 开启数据库
默认使用在当前目录的所咋盘符的根目录创建/data/db用于存储数据`C:\data\db`
`C:\developer\MongoDB\Server\3.6\bin>mongod`
需要手动新建目录`C:\data\db`之后执行
+ 关闭数据库
ctrl+c
***
### 链接数据库
**开启数据库，开启的窗口不能关闭，打开新cmd**
+ 开启数据库，新建cmd输入
`mongo`
+ 链接到目标数据库，非默认地址
`mongod --dbpath D:\data\db`
+ 关闭数据库
`exit`
链接数据库必须保证Mongodb开启服务否则报错，每次使用先开启灾链接
### 配置mongo.config
在安装目录下C:\developer\MongoDB\Server\3.6\bin，添加mongo.config，输入
```
##数据库目录##
dbpath=C:\data\db

##日志输出文件##
logpath=C:\data\log\db.log
```
安装服务
`mongod --config "C:\developer\MongoDB\Server\3.6\bin\mongo.config" --install`

***
## 查看数据库具体操作
**在数据库链接的cmd界面输入**
***
#### 数据库操作
+ 查看所有数据库
`show dbs`
+ 查看当前数据库名称
`db`
+ 创建数据库并切换到该数据库
`use 数据库名`
+ 删除当前数据库
  `db.dropDatabase()`

  ***
#### 数据库下的集合操作
+ 创建集合，类似数组，向该数组增删数据
`db.createCollection(name, options)`
参数，表示创建集合的属性
```js
db.createCollection("mycol", { capped : true, autoIndexId : true, size : 
   6142800, max : 10000 } )
```
|   字段   |  类型    |   描述   |
| :--: | ---- | ---- |
| capped |   布尔   |   （可选）如果为 true，则创建固定集合。固定集合是指有着固定大小的集合，当达到最大值时，它会自动覆盖最早的文档。当该值为 true 时，必须指定 size 参数。   |
|   autoIndexId   |   布尔   |   （可选）如为 true，自动在-id 字段创建索引。默认为 false   |
|   size   |   数值   |   （可选）为固定集合指定一个最大值（以字节计）。如果 capped 为 true，也需要指定该字段。   |
|   max   |   数值   |   （可选）指定固定集合中包含文档的最大数量。   |



+ 删除集合
`db.集合名称.drop()`
+ 显示所有集合
`show collections`
+ 查看单个集合数据
`db.集合名称.find()`
***
#### 集合下文档操作
+ 集合下插入文档
`db.集合名称.insert({})`
```
db.col.insert({title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '菜鸟教程',
    url: 'http://www.runoob.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
})
```
+ 集合下更新
	`db.集合名称.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}}，{multi:true})`
