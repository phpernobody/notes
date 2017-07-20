## 参考
[官方文档](https://docs.mongodb.com/manual)

## 下载安装
[传送门](https://www.mongodb.com/download-center#community)

## 创建数据目录
>mongodb的数据目录不会主动创建，安装完需要手动去创建，数据目录名可以自定义

例： 创建一个数据库地址为`d:\data\db`

## 运行mongodb服务器
`mongod --dpath d:\data\db`

#### 参数说明：
- `--bind_ip`：绑定服务IP，若绑定127.0.0.1，则只能本机访问，不指定默认本地所有IP
- `--logpath`：定MongoDB日志文件，注意是指定文件不是目录
- `--logappend`：使用追加的方式写日志
- `--dbpath`：指定数据库路径
- `--port`：指定服务端口号，默认端口27017
- `--serviceName`：指定服务名称
- `--serviceDisplayName`：指定服务名称，有多个mongodb服务时执行。
- `--install`：指定作为一个Windows服务安装
- `--auth`：开启认证，开启后需登录用户才能操作mongodb


#### 将MongoDB服务器作为Windows服务运行
`mongod --bind_ip yourIPadress --logpath "C:\data\dbConf\mongodb.log" --logappend --dbpath "C:\data\db" --port yourPortNumber --serviceName "YourServiceName" --serviceDisplayName "YourServiceName" --install`

常用本地开启`mongod --bind_ip 127.0.0.1 --logpath "D:\data\dbConf\mongodb.log" --logappend --dbpath "D:\data\db" --port 27017 --serviceName "mymongo" --serviceDisplayName "mymongo" --install`

## 后台管理shell
`mongo`

#### 查询语句
- 查看所有数据库：`show dbs`
- 查看所有集合：`show tables`
- 查看集合数据:`db.<colName>.find()`
- 创建数据库：`use <dbName>`
- 创建索引：`db.<collectionName>.ensureIndex({ "<val>": <Number> })`（Number为1或0，代表是否为索引）
- 插入数据：`db.<colName>.insert(<data>)`
- 删除当前数据库：`db.dropDatabase()`
- 删除集合：`db.<collectionName>.drop()`
- 更新文档：`db.<collectionName>.update(<query>, <update>, { upsert: <boolean>, multi: <boolean>, writeConcern: <document> })`
	- `query`：update的查询条件，类似sql update查询内where后面的
	- `update`：update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的
	- `upsert`：可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入
	- `multi`：可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新
	- `writeConcern`：可选，抛出异常的级别
	- 例子：`db.col.update({ "count": { $gt: 5 } }, { $set: { "test5": "OK" } }, true, true })`
- 更新文档（直接替换已有数据）：`db.<collectionName>.save(<document>, { writeConcern: <document> })`
	- `document`：文档数据
- 删除文档数据：`db.collection.remove(<query>, { justOne: <boolean>, writeConcern: <document> })`
	- `query`:（可选）删除的文档的条件
	- `justOne`: （可选）如果设为 true 或 1，则只删除一个文档
	- `writeConcern`:（可选）抛出异常的级别
- 查询文档：`db.collection.find(query, projection)[.pretty()]`(pretty可以用来格式化所有文档，可选)
	- `query`：可选，使用查询操作符指定查询条件
	- `projection`：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）
- 限制条数：`db.<collectionName>.find().limit(<Number>)`
- 跳过条数：`db.<collectionName>.find().skip(<Number>)`
- 排序：`db.<collectionName>.find().sort({ "val": <Number> })`（以val为排序依据，Number有两个值可写，1表示正序，-1表示倒序）
- 索引：`db.<collectionName>.find().ensureIndex({ "val": <Number> })`（以val为索引，Number有两个值可写，1表示正序，-1表示倒序）
- 聚合：`db.<collectionName>.aggregate(<aggregateName>)`
	- 案例
			
			查询命令：			
			db.mycol.aggregate([{ 
				$group: {
					_id: "$by_user",
					num_tutorial: { $sum: 1}
				}
			}])
			结果：
			{
			   "result" : [
			      {
			         "_id": "runoob.com",
			         "num_tutorial": 2
			      },
			      {
			         "_id" : "Neo4j",
			         "num_tutorial": 1
			      }
			   ],
			   "ok" : 1
			}
	- 管道：将当前命令的输出结果作为下一个命令的参数
		- $project：修改输入文档的结构。可以用来重命名、增加或删除域，也可以用于创建计算结果以及嵌套文档
		- `$match`：用于过滤数据，只输出符合条件的文档。$match使用MongoDB的标准查询操作
		- `$limit`：用来限制MongoDB聚合管道返回的文档数
		- `$skip`：在聚合管道中跳过指定数量的文档，并返回余下的文档
		- `$unwind`：将文档中的某一个数组类型字段拆分成多条，每条包含数组中的一个值
		- `$group`：将集合中的文档分组，可用于统计结果
		- `$sort`：将输入文档排序后输出
		- `$geoNear`：输出接近某一地理位置的有序文档
	- 聚合表达式
		- `$sum`：计算总和
		- `$avg`	：计算平均值
		- `$min`	：获取集合中所有文档对应值得最小值
		- `$max`	：获取集合中所有文档对应值得最大值
		- `$push	`：在结果文档中插入值到一个数组中
		- `$addToSet`：在结果文档中插入值到一个数组中，但不创建副本
		- `$first`：根据资源文档的排序获取第一个文档数据
		- `$last	`：根据资源文档的排序获取最后一个文档数据
- 查看查询信息（可以用于分析查询性能）：`db.<collectionName>.find(<condition>).explain()`
	- `indexOnly`：字段为 true ，表示我们使用了索引。
	- `cursor`：因为这个查询使用了索引，MongoDB 中索引存储在B树结构中，所以这是也使用了 BtreeCursor 类型的游标。如果没有使用索引，游标的类型是 BasicCursor。这个键还会给出你所使用的索引的名称，你通过这个名称可以查看当前数据库下的system.indexes集合（系统自动创建，由于存储索引信息，这个稍微会提到）来得到索引的详细信息
	- `n`：当前查询返回的文档数量
	- `nscanned/nscannedObjects`：表明当前这次查询一共扫描了集合中多少个文档，我们的目的是，让这个数值和返回文档的数量越接近越好
	- `millis`：当前查询所需时间，毫秒数
	- `indexBounds`：当前查询具体使用的索引



#### 操作符
例：`db.col.find({ "$or": [ "val1": { $lt: 50 }, "val2": { $gt: 10 } ], "$and": [ "val3": { $gt: 1 }] }).pretty()`查询likes小于50的数据

- `$lt`：小于
- `$lte`：小于等于
- `$gt`：大于
- `$gte`：大于等于
- `$nt`：不等于
- `$or`：或
- `$and`：与
- `$type`：字符类型（值为数字）
	- Double：1	 
	- String：2	 
	- Object：3	 
	- Array：4	 
	- Binary data：5	 
	- Undefined	6：已废弃
	- Object id：7	 
	- Boolean：8	 
	- Date：9	 
	- Null：10	 
	- Regular Expression：11	 
	- JavaScript：13	 
	- Symbol：14	 
	- JavaScript (with scope)：15	 
	- 32-bit integer：16	 
	- Timestamp：17	 
	- 64-bit integer：18	 
	- Min key：255（Query with -1）
	- Max key：127


## 文档关系设计
#### 文档关系


