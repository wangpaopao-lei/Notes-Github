# 第五章 MongoDB 数据库技术

## 5.8

1）

```JSOn
db.ct.find(
	{
		{$or:[
    	{age:{$lte:30}},
  		{amount:{$gte:500}}
		]},
		type:"vip"
	},
	{
		_id:0
	})
```

2)

```JSON
db.ct.updateOne(
	{
    name:"Wang"
  }
  {
  	{$set:{
  		type:"VIP",
  		age:28
  	}}
  }
	{
    upsert:true
  }
)
```

## 5.9

1. group：对每个唯一的 province 和 city 分组，重命名为 state 和 city，计算 num 值之和，重命名为 num 字段
2. sort：按照 num 的值升序排列
3. group：对 state 进行分组，输出在每个省内 num 最大的城市和 num 值以及 num 最小的城市和 num 值