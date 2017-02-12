龙哥的教程传送入口： #3877-8

对框架的理解：
	目前项目为三个层结构：action层、service层和dao层；
	
	三者的功能分别是
	- action层：负责获取客户端的需求，将需求加工一下再去向service
	- service层：根据action传达的需求进行业务逻辑处理，如果有需要操作到数据库的地方通过dao层去实现
	- dao层：负责与数据库的交互，从数据库中提取service层需要的数据并返回给service层

	一个不一定恰当的比喻：这是一个开发团队，action是产品经理，负责客户需求并告知service；service是管理者，负责业务的实现；dao是程序员，负责提供业务所需要的代码；

每个业务需要涉及的几个包（以钱包业务为例子）：

	- com.wuwangkeji.homeflavor.controller.WalletController：控制层，实现业务逻辑
	- com.wuwangkeji.homeflavor.manager.impl.WalletManagerImpl：管理层，连接控制层和模型层
	- com.wuwangkeji.homeflavor.manager.impl.WalletManager：管理层接口
	- com.wuwangkeji.homeflavor.manager.dao：模型层接口
	- com.wuwangkeji.homeflavor.manager.dao.impl：模型层，连接数据库并返回给管理层
	- com.wuwangkeji.homeflavor.bean：数据实体，里面包含一个序列化的实体（代码中有引用java.io.serializable），还有一份xml，用于配置数据表与bean实体的字段对应关系

一个接口调用时程序内部的执行过程（以获取所有充值记录接口为例）

>客户端请求wallet.do?method=getAllRechargeRecord接口 -> com.wuwangkeji.homeflavor.controller.WalletController.getAllRechargeRecord() -> com.wuwangkeji.homeflavor.manager.impl.WalletManagerImpl.getAllRechargeRecord() -> com.wuwangkeji.homeflavor.dao.impl.WalletDAO.allRechargeRecordGet() -> 通过HQL语句向数据库请求数据


Hibernate中execute、executeQuery跟executeUpdate之间的区别:

- executeQuery:用于产生单个结果集的语句，例如 SELECT 语句。 被使用最多的执行 SQL 语句的方法是 executeQuery。这个方法被用来执行 SELECT 语句，它几乎是使用最多的 SQL 语句。
- executeUpdate:用于执行 INSERT、UPDATE 或 DELETE 语句以及 SQL DDL（数据定义语言）语句，例如 CREATE TABLE 和 DROP TABLE。INSERT、UPDATE 或 DELETE 语句的效果是修改表中零行或多行中的一列或多列。executeUpdate 的返回值是一个整数，指示受影响的行数（即更新计数）。对于 CREATE TABLE 或 DROP TABLE 等不操作行的语句，executeUpdate 的返回值总为零。
- execute:用于执行返回多个结果集、多个更新计数或二者组合的语句。


