# 第七章 Redis 测试 2

## 1-3 新增新闻信息

### 命令

```
hmset N1000 "tittle" "Tittle1" "author" "user1" "content" "Content1" "pdata" "20230416"

hmset N2000 "tittle" "Tittle2" "author" "user2" "content" "Content2" "type" "edu" "pdata" "20230417"

hmset N1001 "tittle" "Tittle3" "author" "user3" "content" "Content3" "type" "tec" "pdata" "20230417"
```

### 结果

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230424182048540.png" alt="image-20230424182048540" style="zoom:50%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230424181157002.png" alt="image-20230424181157002" style="zoom:50%;" />

## 4 查找 N1000

### 命令

```
hgetall N1000
```

### 结果

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230424182213283.png" alt="image-20230424182213283" style="zoom:50%;" />

## 5 修改 N2000

### 命令

```
hset N2000 "followerNum" 1238086
```

### 结果

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230424182255750.png" alt="image-20230424182255750" style="zoom:50%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230424181311401.png" alt="image-20230424181311401" style="zoom:50%;" />

## 6 删除 N1001 类型字段

### 命令

```
hdel N1001 type
```

### 结果

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230424182416406.png" alt="image-20230424182416406" style="zoom:50%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230424182433699.png" alt="image-20230424182433699" style="zoom:50%;" />

## 7 输出 N2000 field

### 命令

```
hvals N2000
```

### 结果

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230424182507289.png" alt="image-20230424182507289" style="zoom:50%;" />



## 8 删除 N1000

### 命令

```
del N1000
```

### 结果

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230424182530498.png" alt="image-20230424182530498" style="zoom:50%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230424182542068.png" alt="image-20230424182542068" style="zoom:50%;" />