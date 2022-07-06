## MongoDB

MongoDB 是一个非关系型数据库。MongoDB  和MySQL、redis是同一个级别的。是基于分布式文件存储的数据库。由c++语言编写。不支持多表查询，表数据需要存在一个集合里，一个文件里 。

MySQL 关系型数据库是二维表格，mongoDB 非关系型数据库是json 字符串。

![image-20220608165606194](F:\Typora\typora-user-images\image-20220608165606194.png)

MongoDB 支持的数据结构非常松散，是类似json的bson格式，因此可以存储比较复杂的数据类型。Mongo最大的特点是它支持的查询语言非常强大，其语法有点类似于面向对象的查询语言，几乎可以实现类似关系数据库单表查询的绝大部分功能，而且还支持对数据建立索引。

MongoDB服务端可运行在Linux、Windows或mac os x平台，支持32位和64位应用，默认端口为27017。


下载地址： https://www.mongodb.com/download-center/community

安装教程：https://blog.csdn.net/qq_46163588/article/details/117661312

MongoDB 安装过程中比较需要注意的是 修改安装目录到非c盘路径下。 安装完，要配置 系统变量 path路径 ：

```
安装目录 + \bin
```

MongoDB引入了Visual Studio Code 的新扩展，这使得与Mongo一起使用变得超级容易。



#### MongoDB 主要特点

<img src="F:\Typora\typora-user-images\image-20220321233155013.png" alt="image-20220321233155013" style="zoom:67%;" />

##### 1、MongoDB 集合

<img src="F:\Typora\typora-user-images\image-20220321234234629.png" alt="image-20220321234234629" style="zoom:67%;" />

##### 2、MongoDB 文档

MongoDB中的记录是一个文档，它是由字段和值对组成的数据结构。
多个键及其关联的值有序地放在一起就构成了文档。
MongoDB文档类似于JSON对象。字段的值可以包括其他文档，数组和文档数组。

![在这里插入图片描述](F:\Typora\typora-user-images\20200823165839980.png#pic_center)

{“gender”:“man”}这个文档只有一个键“gender”，对应的值为“man”。多数情况下，文档比这个更复杂，它包含多个键/值对。

例如：{“gender”:“man”,“foo”: 9} 文档中的键/值对是有序的，下面的文档与上面的文档是完全不同的两个文档。{“foo”: 9 ,“gender”:“man”}

文档中的值不仅可以是双引号中的字符串，也可以是其他的数据类型，例如，整型、布尔型等，也可以是另外一个文档，即文档可以嵌套。文档中的键类型只能是字符串。

<img src="F:\Typora\typora-user-images\image-20220321234436737.png" alt="image-20220321234436737" style="zoom:67%;" />

以不同的字段 / 不同域，可以存储不同的数据

![image-20220321234822337](F:\Typora\typora-user-images\image-20220321234822337.png)

**使用文档的优点是：**

文档（即对象）对应于许多编程语言中的本机数据类型。
嵌入式文档和数组减少了对昂贵连接的需求。
动态模式支持流畅的多态性。

##### MongoDB术语

|                   |                |                                      |
| ----------------- | -------------- | ------------------------------------ |
| sql 术语          | MongoDB术语    | 解析、说明                           |
| database   数据库 | database       | 数据库                               |
| table  表         | collection     | 数据库中的表 / 集合                  |
| row    行         | document       | 数据记录行 / 文档                    |
| column  列/字段   | field          | 数据 字段 / 域                       |
| index  索引       | index          | 索引                                 |
| table joins       | 不支持多表查询 | 表连接，MongoDB不支持                |
| primary key       | primary key    | 主键，MongoDB自动将_id字段设置为主键 |

#### 在windows系统 中安装

验证MongoBD 安装是否成功，在cmd 中输入 mongo ，弹出如下说明安装成功了：

![image-20220322000621735](F:\Typora\typora-user-images\image-20220322000621735.png)

安装完后，记得配置环境变量。

#### MongoDB 数据库常用命令

```
命令	            											说明
show dbs 或者show databases		显示数据库列表
use 数据库名										切换数据库，如果不存在创建数据库
db.dropDatabase()							 删除数据库
show collections或者show tables	显示当前数据库的集合列表
db.集合名.stats()								 查看集合详情
db.集合名.drop()								 删除集合
show users										  显示当前数据库的用户列表
show roles										   显示当前数据库的角色列表
show profile									    显示最近发生的操作
load(“xxx.js”)									   执行一个JavaScript脚本文件
exit或者quit()									  退出当前shell
help													 查看mongodb支持哪些命令
db.help()											 查询当前数据库支持的方法
db.集合名.help()								显示集合的帮助信息
db.version()										查看数据库版本
```



```
1、数据库操作
db //查询数据库
show dbs  //查询所有的数据库
use music //创建 或者 切换数据库
db.dropDatabase()  //删除数据库

2、集合操作   （一个集合 对应 一个表格）
db.createCollection("集合名称")   创建一个集合
db.getCollectionNames()   显示当前db的所有集合

3、文档操作
（1）插入数据
db.xiaohong.insert([{name:'xiaohong',age:'18'}])

db.china.save([{name:'zhangsan',age:'19'},{name:'lisi',age:'20'}])   //同上
（1.2）继续添加属性和属性值
db.china.insert([{name:'xiaohong',age:'19',sex:'woman'}])  

（2）修改数据
db.china.update({name:'xiaohong'},{$set:{age:'21'}})
结果：{ "_id" : ObjectId("62a099ccdbf9b129e9f2d0bf"), "name" : "xiaohong", "age" : "21" }

（3）删除数据
db.china.remove({name:'lisi'})
结果：WriteResult({ "nRemoved" : 1 })

 (4)查询数据
db.china.find()   
{ "_id" : ObjectId("62a099ccdbf9b129e9f2d0bf"), "name" : "xiaohong", "age" : "18" }
{ "_id" : ObjectId("62a09aa0dbf9b129e9f2d0c0"), "name" : "zhangsan", "age" : "19" }
{ "_id" : ObjectId("62a09aa0dbf9b129e9f2d0c1"), "name" : "lisi", "age" : "20" }

(4.1) 查询去重后数据
db.china.distinct('name')
[ "xiaohong", "zhangsan" ]

(4.2) 查询age=19的记录
db.china.find({age:'19'})
{ "_id" : ObjectId("62a09aa0dbf9b129e9f2d0c0"), "name" : "zhangsan", "age" : "19" }
{ "_id" : ObjectId("62a09db3dbf9b129e9f2d0c2"), "name" : "xiaohong", "age" : "19", "sex" : "woman" }       
{ "_id" : ObjectId("62a0a17cdbf9b129e9f2d0c3"), "name" : "zhangsan", "age" : "19", "sex" : "woman" }  

(4.3) 查询age大于19的记录
db.china.find({age:{$gt:'19'}})
(4.4) 查询age大于等于19的记录
db.china.find({age:{$gte:'19'}})
(4.5) 查询age小于21的记录
db.china.find({age:{$lt:'21'}})
(4.6) 查询age小于等于19的记录
db.china.find({age:{$lte:'19'}})
(4.7) 查询age大于等于19 并且 age 小于等于21 的记录
db.china.find({age:{$gte:'19',$lte:'21'}})
(4.8)查询name中包含zhang的数据
db.china.find({name:/zhang/})
(4.9)查询name以zhang开头的数据
db.china.find({name:/^zhang/})
(5)查询name以hong结尾的数据
db.china.find({name:/hong$/})
(5.1)查询指定列sex数据
db.china.find({},{sex:0})
(5.2)按照年龄排序
db.china.find().sort({age:1})
(5.3)查询前3条数据
db.china.find().limit(3)
(5.4) or查询
db.china.find({$or:[{age:'19'},{sex:'woman'}]})
(5.5) 查询第一条数据
db.china.findOne() 
(5.5) 查询总记录条数
db.china.find().count()
(5.5) 查询某个结果集的记录条数
db.china.find()
```



#### 本地 启动 mongod 服务

在开一个cmd中 直接输入mongo  会报错：

![image-20220527105731830](F:\Typora\typora-user-images\image-20220527105731830.png)

解决方法：

在开一个cmd中 输入：mongod -dbpath D:\resource\MongoDB\Server\5.0\data

再在开一个cmd,输入mongo命令，启动成功了。



#### 阿里云的docker中启动 mongod 服务

![image-20220527105606687](F:\Typora\typora-user-images\image-20220527105606687.png)

#### docker中配置mongodb 文件

首先进入Mongo 安装目录 ：

```
docker ps -a  查看 docker 容器，显示所有的容器，包括未运行的（容器即镜像，比如已下载安装的MongoDB镜像
 
docker exec -it 30c5235d288e /bin/bash     进入容器
```

`/etc/mongod.conf`通过附加以下行在MongoDB 配置文件中启用数据库授权：

```
security:
  authorization: enabled
```

想查看刚安装的MongoDB 里面有哪些什么东西，首先要知道如何使用命令查看：

```mysql
db       	  查看当前使用的数据库  
show dbs     查询所有的数据库 （显示当前里面有几个数据库）

use demo1    创建数据库 / 切换数据库    （但是如果你没有在当前数据库里面创建 集合，存文档，         使用  show dbs 命令查看时是暂时看不到新建的数据库的，但可以通过 db 可以查看当前使用的数据库  是demo1）

db.getName()  查看当前使用的数据库
db.stats()   显示当前DB状态
db.version()  查看当前BD版本     5.0.6
db.getMongo()  查看当前DB的链接机器地址      connection to 127.0.0.1:27017    端口号 27017
db.dropDatabase()   删除数据库   一定要在当前打开的数据库里去删除，才能删除当前使用的数据库
help   查看命令提示

```

<img src="F:\Typora\typora-user-images\image-20220322014414699.png" alt="image-20220322014414699" style="zoom: 50%;" />



`nodebb`创建一个管理用户（与我们稍后创建的用户不同）。`<Enter a secure password>`用您自己选择的密码替换占位符 :

```
db.createUser( { user: "admin", pwd: "nodebb2022!", roles: [ { role: "root", db: "admin" } ] } )
```



```
db.createUser( { user: "nodebb", pwd: "nodebb2022!", roles: [ { role: "readWrite", db: "nodebb" }, { role: "clusterMonitor", db: "admin" } ] } )
```



```
db.createUser( { user: "node", pwd: "1234567", roles: [ { role: "readWrite", db: "node" }, { role: "clusterMonitor", db: "admin" } ] } )
```

mongo -u admin -p nodebb2022! --authenticationDatabase=admin

```
导入包管理系统使用的公钥**（直接复制就好了，适用于5.0版本以上的mongodb）

wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | sudo apt-key add -

**下面命令针对 Ubuntu16.04 版本，在其他 Ubuntu 版本系统请查看 MongoDB 官网。**

echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/5.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-5.0.list

更新一下源
sudo apt-get update

sudo apt-get install -y mongodb-org
```

ubuntu ； konka2022



#### Visual Studio Code 中 安装扩展

在Visual Studio Code的 **扩展搜索** 中进行搜索，或在市场上导航至 MongoDB for VS Code ，下载并启用。

MongoDB建立了一个名为MongoDB for VS Code的扩展，该扩展使你可以直接从编辑器连接到MongoDB Shell和MongoDB Atlas。

**通过扩展，你可以：**

- 直接从编辑器连接到MongoDB Shell或Atlas Cluster。
- 浏览数据库，集合和文档。
- 查看和分析你的架构。
- 在MongoDB Playground中使用自动完成和语法高亮显示原型CRUD操作和MongoDB命令。

##### 连接到MongoDB

该扩展使你可以连接到多个MongoDB实例，你可以连接到本地MongoDB实例，Atlas群集或任何自托管实例。

要进行连接，你可以输入主机名和端口，也可以提供如下所示的连接字符串：

```javascript
mongodb://localhost:27017/
```

连接后，该扩展将为你提供数据库，集合和文档的树状视图。此外，你还可以概览每个集合的架构。

