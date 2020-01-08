## 简介

MongoDB是一个基于分布式文件存储的开源数据库系统。

## 启动

**服务端启动**

```bash
mongodb/bin/mongod --dbpath D:\mongodb\data
```

如果出现`waiting for connections on port 27017`就表示启动成功。

当然`mongod`还有其他命令行参数：

* `--port`：指定服务端口，默认是27017
* `--logpath`：指定MongoDB日志文件，注意是文件而不是目录
* `--logappend`：使用追加的方式写日志
* `--dbpath`：指定数据库路径
* `--directoryperdb`：设置每个数据库将被保存再一个单独的目录

**客户端链接**

```bash
mongo

# 或者
# mongo --host 127.0.0.1
```

> GUI客户端推荐`Robo 3T`。

## 基本概念

* 集合：数据库是由多个集合组成，一个集合用来表示一个实体
* 文档：集合是由文档组成，一个文档表示一条记录

和MySQL的对应关系如下：

| MongoDB     | MySQL       | 释义    |
| ----------- | ----------- | ------- |
| database    | database    | 数据库  |
| collection  | table       | 集合/表 |
| document    | row         | 文档/行 |
| field       | cloumn      | 域/字段 |
| index       | index       | 索引    |
| primary key | primary key | 主键    |

## 数据库操作

### 查看所有数据库

```bash
show dbs
```

### 使用数据库

切换数据库。

```bash
use school
```

如果要切换到的数据库不存在，那么也能切换过来。但是如果要将该数据库显示出来，需要insert一条数据。

### 查看当前使用的数据库

```bash
db
# 或
# db.getName()
```

### 删除数据库

```bash
db.dropDatabase()
```

## 集合操作

### 查看集合帮助

```bash
db.school.help()
```

### 查看数据库下的集合

```bash
show collections
```

### 创建集合

创建一个空集合。

```bash
db.createCollection('class_one')
```

创建集合并插入一个文档。

```bash
db.class_one.insert({name: 'ugu'})
```

## 文档操作

### 插入文档

#### insert

基本用法：`db.collection_name.insert(document)`。

```bash
db.class_one.insert({name: 'ugu'})
```

每当插入一条新文档的时候，`MongoDB`会自动为此文档添加一个唯一的ID属性作为主键来标识这个文档。当然，这个主键也可以手动指定。

**insertOne**

```
db.class_one.insertOne(<document>, {
	writeConcern: <document>
})
```

其中`writeConcern`定义了本次文档创建操作的安全写级别。安全写用来判断依次数据库写入操作是否成功，级别越高，丢失数据的风险就越低，然而写入操作的延迟就可能更高。如果不提供`writeConcern`文档，那么就使用默认的安全写级别。

**insertMany**

```
db.class_one.insert([
    {name: 'ugu1'},
    {name: 'ugu2'}
])
```

#### save

基本用法：`db.collection_name.save(document)`。

注意：如果不指定`_id`字段，`save`方法类似于`insert`方法；如果指定`_id`字段，则会更新该`_id`的数据。

### 更新文档

基本用法：

```bash
db.collection.update(
	<query>,
	<updateObj>,
	{
		upsert: <boolean>,
		multi: <boolean>
	}
)
```

其中：

* query：查询条件，指定要更新符合条件的文档
* updateObj：更新后的对象或指定一些更新的操作符
* upsert：可选参数，标识不存在符合条件的记录时是否插入updateObj，默认为false。即不插入
* multi：可选参数，mongodb默认只更新符合条件的第一条记录，如果这个参数为true，则更新所有符合条件的记录

#### 操作符

**$inc**

基本用法：`{$inc: {}}`，表示在原基础上累加。

```bash
db.class_one.find()
# { "_id" : ObjectId("5dd25b1c1cdf52141e66261f"), "name" : "ugu" }

db.class_one.update({name: 'ugu'}, {$inc: {age: 10}})

db.class_one.find()
# { "_id" : ObjectId("5dd25b1c1cdf52141e66261f"), "name" : "ugu", "age": 10 }
```

查询`name=ugu`的文档，然后在原有的基础上添加`{age: 10}`。

**$push**

表示在某个数组字段添加元素，不会覆盖数组中已有的元素。

```bash
db.class_one.find()
# { "_id" : ObjectId("5dd25b1c1cdf52141e66261f"), "name" : "ugu", "hobby" : [ "study" ] }

db.class_one.update({name: 'ugu'}, {$push: {hobby: 'study'}})

db.class_one.find()
# { "_id" : ObjectId("5dd25b1c1cdf52141e66261f"), "name" : "ugu", "hobby" : [ "study" ] }
```

注意：若这个数组字段不存在则会自动创建。

```bash
db.class_one.find()
# { "_id" : ObjectId("5dd25b1c1cdf52141e66261f"), "name" : "ugu" }

db.class_one.update({name: 'ugu'}, {$push: {hobby: 'study'}})

db.class_one.find()
# { "_id" : ObjectId("5dd25b1c1cdf52141e66261f"), "name" : "ugu", "hobby" : [ "study" ] }
```

**$addToSet**

表示在某个数组字段添加元素，但是如果这个添加的值已经在数组中则不会再添加。

```bash
db.class_one.find()
# { "_id" : ObjectId("5dd25b1c1cdf52141e66261f"), "name" : "ugu", "hobby" : [ "study" ] }

db.class_one.update({name: 'ugu'}, {$addToSet: {hobby: 'study'}})

db.class_one.find()
# { "_id" : ObjectId("5dd25b1c1cdf52141e66261f"), "name" : "ugu", "hobby" : [ "study" ] }
```

注意：若这个数组字段不存在则会自动创建。

**$pop**

表示删除某个数组字段第一个或最后一个元素，若传1则删除最后一个元素，传-1则删除第一个元素。

**$set**

用来修改字段值。

```bash
# 修改字段的值
db.class_one.update({name: 'ugu'}, {$set: {name: 'guyou'})

# 修改字段的某个属性值
db.class_one.update({name: 'ugu'}, {$set: {info.age: 10}})

# 修改指定索引的元素值
db.class_one.update({name: 'ugu'}, {$set: {frends.1: 'lilei'}})
```

**$unset**

用来移除字段值。

```bash
db.class_one.update({name: 'ugu'}, {$unset: {age: ''}})		# 删除age字段
```

#### 总结

还有`$each`，`$ne`等操作符，自己了解。

### 删除文档

通过`remove`方法移除集合中的文档，基本用法如下：

```bash
db.collection.remove(
	<query>,
	{
		justOne: <boolean>
	}
)
```

其中：

* query：可选参数，表示查询条件
* justOne：可选参数，默认为true。当值为true时，会删除匹配到的第一个文档；若值为false，会删除匹配的所有文档

### 查询文档

#### 方法

**find**

通过`find`方法可以查询集合中的文档，基本用法如下：

```bash
db.collection_name.find(query, projection)
```

其中：

* query：指定查询条件
* projection：指定返回的字段

注意：默认情况下`_id`字段会自动一直返回，除非你手动将`_id`字段设置为0或false。

```bash
db.class_one.find()			# 查询class_one下的所有文档

db.class_one.find({_id: '5ae1b6e3e4366d57f3307239'})		# 根据id查询

db.class_one.find({_id: '5ae1b6e3e4366d57f3307239'}).count() # 获取查询的结果个数
```

**findOne**

通过`findOne`查询匹配到第一条文档。

#### 操作符

**$in**

在指定数组区间内查询。

```bash
# 原始数据
# { "_id" : 1, "name" : "Tom1", "age" : 9 }
# { "_id" : 2, "name" : "Tom2", "age" : 15 }
# { "_id" : 3, "name" : "Tom3", "age" : 11 }

db.class_one.find({age:{$in:[9,11]}})
# { "_id" : 1, "name" : "Tom1", "age" : 9 }
# { "_id" : 3, "name" : "Tom3", "age" : 11 }
```

**$nin**

查询不在执行区间内的文档。

```bash
db.class_one.find({age:{$nin:[9,11]}})
# { "_id" : 1, "name" : "Tom1", "age" : 15 }
```

**$not**

查询不符合条件的文档。

```bash
db.class_one.find({age:{$not:{$lt:11}}})
# { "_id" : 2, "name" : "Tom2", "age" : 15 }
# { "_id" : 3, "name" : "Tom3", "age" : 11 }
```

**$gt**

表示大于。

**$gte**

表示大于等于。

**$lt**

表示小于。

**$lte**

表示小于等于。

**$ne**

表示不等于。

```bash
db.class_one.find({age:{$ne:9}})
# { "_id" : 2, "name" : "Tom2", "age" : 15 }
# { "_id" : 3, "name" : "Tom3", "age" : 11 }
```

**$where**

`$where`接收两种参数进行查询：

* JavaScript表达式或者字符串
* JavaScript函数

```bash
# 原始数据
# { "_id" : 1, "name" : "Tom1", "age" : 9, "friends" : [ "Lily", "Jobs", "Lucy", "Zhang San" ] }
# { "_id" : 2, "name" : "Tom2", "age" : 15, "friends" : [ "Zhange San", "Li Si" ] }
# { "_id" : 3, "name" : "Tom3", "age" : 11, "friends" : [ "Zhange San", "Lily" ] }

# JavaSCript表达式的字符串
db.class_one.find({$where:'this.name == "Tom1"'})
# { "_id" : 1, "name" : "Tom1", "age" : 9, "friends" : [ "Lily", "Jobs", "Lucy", "Zhang San" ] }

# JavaScript函数
db.grade1.find({$where: function(){return this.age == 9}})
# { "_id" : 1, "name" : "Tom1", "age" : 9, "friends" : [ "Lily", "Jobs", "Lucy", "Zhang San" ] }
```

注意：`$where`性能不是很好。

#### 数组操作符

针对数组操作的操作符。

```bash
# 原始数据
# { "_id" : ObjectId("5dd266381cdf52141e662620"), "arr" : [ "ugu", "zhangsan" ] }
# { "_id" : ObjectId("5dd266421cdf52141e662621"), "arr" : [ "ugu", "zhangsan", "lisi" ] }
# { "_id" : ObjectId("5dd2665d1cdf52141e662623"), "arr" : [ "guyou", "zhangsan", "lisi", "ugu" ] }
```

**$all**

```bash
db.classone.find({arr: {$all: ['ugu']}})
# { "_id" : ObjectId("5dd266381cdf52141e662620"), "arr" : [ "ugu", "zhangsan" ] }
# { "_id" : ObjectId("5dd266421cdf52141e662621"), "arr" : [ "ugu", "zhangsan", "lisi" ] }
# { "_id" : ObjectId("5dd2665d1cdf52141e662623"), "arr" : [ "guyou", "zhangsan", "lisi", "ugu" ] }
```

**$in**

```bash
db.classone.find({arr: {$in: ['ugu']}})
# { "_id" : ObjectId("5dd266381cdf52141e662620"), "arr" : [ "ugu", "zhangsan" ] }
# { "_id" : ObjectId("5dd266421cdf52141e662621"), "arr" : [ "ugu", "zhangsan", "lisi" ] }
# { "_id" : ObjectId("5dd2665d1cdf52141e662623"), "arr" : [ "guyou", "zhangsan", "lisi", "ugu" ] }
```

**$size**

```bash
db.classone.find({arr: {$size: 4}})
# { "_id" : ObjectId("5dd2665d1cdf52141e662623"), "arr" : [ "guyou", "zhangsan", "lisi", "ugu" ] }
```

**$slice**

```bash
db.classone.find({arr: {$size: 4}},{arr:{$slice:2}})
# { "_id" : ObjectId("5dd2665d1cdf52141e662623"), "arr" : [ "guyou", "zhangsan" ] }
```

#### 正则表达式

你还可以通过正则表达式进行查询。

```bash
db.class_one.find({name: /^u/})		# 查询name值以u开头
```

#### 逻辑查询

**and**

```bash
db.class_one.find({name: /^u/, age: 10})		# 查询name值以u开头并且age为10
```

**or**

```bash
db.class_one.find({$or:[{name: /^u/}, {age: 11}]})	# 查询name值以u开头并且age为11
```

当然and和or还可以联合使用。

#### 分页查询

**limit**

```bash
db.class_one.find({name: /^u/}).limit(10)		# 查询指定数量的文档
```

**skip**

```bash
db.class_one.find({name: /^u/}).skip(10)		# 跳过指定数量查询
```

#### 排序查询

**sort**

```bash
db.class_one.find({name: /^u/}).sort({name: 1})			# 以name值升序排序
db.class_one.find({name: /^u/}).sort({name: -1})		# 以name值降序排序
```

#### 格式化

```bash
db.class_one.find().pretty()
```

#### Cursor Methods

包括`cursor.forEach()`、`cursor.map()`、`cursor.limit()`、`cursor.size()`、`cursor.count()`等。

下面以`forEach()`为例：

```bash
var result = db.grade1.find({$where: function(){return this.age >= 9}});
result.forEach(elem => printjson(elem))
###
    {
        "_id" : 1,
        "name" : "Tom1",
        "age" : 9,
        "friends" : [
            "Lily",
            "Jobs",
            "Lucy",
            "Zhang San"
        ]
    }
    {
        "_id" : 2,
        "name" : "Tom2",
        "age" : 15,
        "friends" : [
            "Zhange San",
            "Li Si"
        ]
    }
###
```

## ObjectId

`Mongodb`中的文档主键具有如下特性：

* 唯一性
* 支持所有类型（数组除外）
* 复合主键

当然你也可以不指定主键，`Mongodb`会自动生成`ObjectId`对象主键，它是一个12字节的`BSON`类型字符串。按照字节顺序，分别代表：

- 4字节：UNIX时间戳
- 3字节：表示运行MongoDB的机器
- 2字节：表示生成此`_id`的进程
- 3字节：由一个随机数开始的计数器生成的值

> `MySQL`这些关系型数据库的主键都是设置成自增的。但是在但在分布式环境下，这种方法会产生冲突。

## 运维

* 用户权限
* 允许指定IP链接