# MongoDB 数据库技术实验报告







<center>
  姓名：王磊
</center>


<center>
  学号：2020211538
</center>


<center>
  提交日期：2023-4-21
</center>




## Content

[toc]

## 实验环境

平台：MacOS ventura

数据库版本：MongoDB community 6.0.5

其它工具：Studio 3T，VS Code

## 实验内容

### DB0

启动数据库

切换到 admin

![image-20230422170219526](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230422170219526.png)

创建 root 用户，设置角色及密码

![image-20230422170707915](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230422170707915.png)

db.auth（）验证是否成功

![image-20230422170834514](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230422170834514.png)

关闭服务，修改配置文件，开启认证授权

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230422171259453.png" alt="image-20230422171259453" style="zoom:50%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230422174703718.png" alt="image-20230422174703718" style="zoom:50%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230422174615121.png" alt="image-20230422174615121" style="zoom:50%;" />



重启服务

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230422175044390.png" alt="image-20230422175044390" style="zoom:50%;" />

以 root 用户连接数据库

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230422180534986.png" alt="image-20230422180534986" style="zoom:50%;" />

### DB1

1. 创建 Movie_538 数据库
   1. shell 显示当前数据库
   2. shell 查询所有数据库

![](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230422192109076.png)

![image-20230422192605382](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230422192605382.png)

![image-20230422192745494](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230422192745494.png)

### DB2 

创建集合 Movies，将电影文档插入集合

```MongoDB
db.Movies.insertOne({
	"movieTittle":"流浪地球2",
	"year":2023,
	"director":[
		{
			"DNo":1,
			"pname":"郭帆",
			"pID":"001"
		}
	],
	"screenwriterList":[
		{
			"No":1,
			"pname":"杨治学",
			"pID":"002"
		},
		{
			"No":2,
			"pname":"龚格尔",
			"pID":"003"
		},
		{
			"No":3,
			"pname":"郭帆",
			"pID":"001"
		},
		{
			"No":4,
			"pname":"叶濡畅",
			"pID":"004"
		}
	],
	"actorList5":[
		{
			"No":1,
			"pname":"吴京",
			"pID":"005"
		},
		{
			"No":2,
			"pname":"刘德华",
			"pID":"006"
		},
		{
			"No":3,
			"pname":"李雪健",
			"pID":"007"
		},
		{
			"No":4,
			"pname":"沙溢",
			"pID":"008"
		},
		{
			"No":5,
			"pname":"宁理",
			"pID":"009"
		}
	],
	"movieType":["science fiction","adventural","catastrophical"],
	"producerCountry":"China",
	"language":["Madarian","Russion","English","French"],
	"releaseDateList":[
		{
			"rName":"China",
			"rDate":"20230122"
		}
	],
	"length":173,
	"alternativeName":["The Wandering Earth II","The Wandering Earth 2","《流浪地球》前传"],
	"IMdbLink":"tt13539646",
	"picture":"www.xxx.com/pic",
	"desc":"太阳即将毁灭……",
	"scoreInfo":{
		"aveScore":8.3,
		"sCount":1037609,
		"sRatio":[45,34.1,15.9,3.5,1.6],
	},
	"betterThan":[
		{
			"typeName":"science fiction",
			"ratio":96
		},
		{
			"typeName":"catastrophical",
			"ratio":97
		}
	]
})
```



#### 运行结果



<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425135125994.png" alt="image-20230425135125994" style="zoom:50%;" />



<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425140536416.png" alt="image-20230425140536416" style="zoom:50%;" />

### DB3

创建电影院集合

```
db.createCollection("Cinemas");
```

#### 运行结果

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425143846147.png" alt="image-20230425143846147" style="zoom:50%;" />

### DB4

插入四个电影院文档，其中 c2 的评分为 2.8

```
db.Cinemas.insertMany(
	{
		"name":"c1",
		"address":"1st Avenue 168-01",
		"location":
			{
				"type":"Point",
				"coordinates":[40,5]
			},
		"serviceScore":8.6,
		"tel":["3275188","1323234665"],
		"latesetMovieList":["Titanic","Interstellar","The Shawnshank Redemption","Leon","Inception"]
	},
	{
		"name":"c2",
		"address":"34th Street 34",
		"location":
			{
				"type":"Point",
				"coordinates":[41,4.5]
			},
		"serviceScore":2.8,
		"tel":["5478359","1357459856"],
		"latesetMovieList":["Forrest Gump","Leon","Titanic"]
	},
	{
		"name":"c3",
		"address":"Route 66 725-33",
		"location":
			{
				"type":"Point",
				"coordinates":[46,8]
			},
		"serviceScore":9.2,
		"tel":["7648234","123075047"],
		"latesetMovieList":["The Truman Show","3 Idiots","The Godfather","Titanic","The Last Emperor"]
	},
	{
		"name":"c4",
		"address":"1st Avenue 226",
		"location":
			{
				"type":"Point",
				"coordinates":[39.8,5.2]
			},
		"serviceScore":8.4,
		"tel":["329875","1320945704"],
		"latesetMovieList":["Coco","Titanic","Farewell My Concubine","Leon"]
	}
);
```

#### 运行结果

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425144211048.png" alt="image-20230425144211048" style="zoom:50%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425144326925.png" alt="image-20230425144326925" style="zoom:50%;" />



### DB5

查询近3天播放某部电影（指定电影名称）的所有 
影院名称、服务评分及地址信息

```
db.Cinemas.find({"latesetMovieList":{$exists:"Titanic"}},{"_id":0,"name":1,"serviceScore":1,"address":1})
```



#### 运行结果

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425145604461.png" alt="image-20230425145604461" style="zoom:50%;" />





### DB6

修改某一影院信息，增加影院星级字段，并设置为 
5星

```
db.Cinemas.updateOne({name:"c1"},{$set:{star:5}});
```

#### 运行结果

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425150121228.png" alt="image-20230425150121228" style="zoom:50%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425150207170.png" alt="image-20230425150207170" style="zoom:50%;" />



### DB7

删除影院服务评分低于3分的所有影院信息

```
db.Cinemas.deleteMany({serviceScore:{$lt:3}});
```

#### 运行结果

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425150456536.png" alt="image-20230425150456536" style="zoom:50%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425150545063.png" alt="image-20230425150545063" style="zoom:50%;" />

可以看到，评分为 2.8 的 c2 电影院已被删除

### DB8

创建文本索引

```
db.Movies.createIndex({"desc":"text"},{language_override:"dummy"})
```

将电影的 desc 字段修改为英文的

```
db.Movies.updateOne({movieTittle:"流浪地球2"},{$set:{desc:"Humans built huge engines on the surface of the earth to find a new home. But the road to the universe is perilous. In order to save earth, young people once again have to step forward to start a race against time for life and death."}})
```

用文本索引进行查询

```
db.Movies.find({$text:{$search:"engines"}})
```



#### 运行结果

![image-20230425151940071](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425151940071.png)

原因：创建文本索引会对字符串字段根据空格和标点符号自动分词，而 MongoDB 并未提供中文分词功能。

![image-20230425153819199](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425153819199.png)

![image-20230425154107651](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425154107651.png)

### DB9

针对电影院位置创建索引

```
db.Cinemas.createIndex({location:"2dsphere"})
```

查询距离 c1 电影院 200km 以内的电影院信息

```
db.Cinemas.find({location:{$near:{$geometry:{type:"Point",coordinates:[40,5]},$maxDistance:200000}})
```



#### 运行结果

创建索引

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425154926863.png" alt="image-20230425154926863" style="zoom:50%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425161840870.png" alt="image-20230425161840870" style="zoom:50%;" />

出现两条结果，一条为 c1 本身，另一条为与 c1 相距 31km 的 c4



### DB10

针对某一部电影、查询汇总播放该电影的影院数

```
db.Cinemas.find({latesetMovieList:{$exists:"Titanic"}}).count()
```

#### 运行结果

![image-20230425162611770](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425162611770.png)

### DB11

将数据库电影集合数据导出到本地JSON格式文件 
中；

```
mongoexport -d Movie_538 -c Movies -o /Users/Wangpaopaopao/Desktop/Movies.json -- type json 
```

#### 运行结果

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425163353820.png" alt="image-20230425163353820" style="zoom:50%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425163421044.png" alt="image-20230425163421044" style="zoom:50%;" />

### DB12

删除电影集合

```
 db.Movies.drop()
```

通过 JSON 文件导入该集合

```
mongoimport -d Movie_538 -c Movies --file /Users/Wangpaopaopao/Desktop/Movies.json --type json
```



#### 运行结果

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425163717785.png" alt="image-20230425163717785" style="zoom:50%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425163923734.png" alt="image-20230425163923734" style="zoom:50%;" />

### DB13

备份到指定路径

```
mongodump -o /Users/Wangpaopaopao/Desktop
```

删除当前数据库

```
db.dropDatabase()
```

恢复该数据库

```
mongorestore --db=Movie_538 --dir=/Users/Wangpaopaopao/Desktop/
```

#### 运行结果

备份数据库到指定路径下

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425184252263.png" alt="image-20230425184252263" style="zoom:50%;" />

备份成功<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425184348721.png" alt="image-20230425184348721" style="zoom:50%;" />

删除当前数据库

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425174726002.png" alt="image-20230425174726002" style="zoom:50%;" />

导入备份数据库

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425175005141.png" alt="image-20230425175005141" style="zoom:50%;" />

检查，发现导入成功

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425175127070.png" alt="image-20230425175127070" style="zoom:50%;" />



### DB14

选择大小为 28MB 的 "5友谊类.mp4" 的视频文件上传

```
mongofiles -d Movie_538 put /Users/wangpaopaopao/Desktop/5友谊类.mp4
```

查询上传结果

```
db.fs.files.find()
```

#### 运行结果

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425184605191.png" alt="image-20230425184605191" style="zoom:50%;" />

查询上传的文件

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425184706971.png" alt="image-20230425184706971" style="zoom:50%;" />

### DB15

新建电影院文档

```python
ct.insert_one({"name":"cnew","address":"add new","location":{"type":"Point","coordinates":[50,10]},"tel":"32438548","serviceScore":9.8,"star":4})
```

查找电影院集合中所有文档

```Python
rows=ct.find()
for row in rows : print(row)
```

修改新增的文档星级为 5

```python
ct.update_one({"name":"cnew"},{'$set':{"star":5}})
```

根据 _id 的值删除新增的文档，需要引入 ObjectId 方法。

先运行查询语句得到新增文档的 _id 值，再根据值删除

```Python
row=ct.find({"name":"c1"},{"_id":1})
for x in row:
    print(x)
    
from bson import ObjectId
ct.delete_one({"_id":ObjectId('6447ae08ac9275eba0e46444')})
```



#### 运行结果



![image-20230425185142382](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425185142382.png)

创建成功

![image-20230425190106315](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425190106315.png)

查询集合中所有文档

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425190151388.png" alt="image-20230425190151388" style="zoom:50%;" />

更新新增文档的星级为 5

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425191652099.png" alt="image-20230425191652099" style="zoom:50%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425190701579.png" alt="image-20230425190701579" style="zoom:50%;" />

查询得到新增文档的 _id 值，根据 _id 删除文档

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425191659843.png" alt="image-20230425191659843" style="zoom:50%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230425191643767.png" alt="image-20230425191643767" style="zoom:50%;" />



## 实验问题总结

为保证报告文档的流畅性，将遇到的问题及解决方法分散到各个实验步骤中。