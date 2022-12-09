不要用中文教材

理论课 40 课时

实验课 8 课时

![image-20220906131747411](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220906131747411.png)



以事务为单位进行管理





数据库技术：进行数据管理的工具

三层架构：前端应用——》DBMS——》数据库

应用：逻辑信息

DBMS：为用户屏蔽底层操作





NoSql：Not only sql

为什么数据库不是面向对象的？

前端是各种各样的应用，DBMS 在中间，Database 是存储数据的地方。厂商提供服务的核心部分在于 DBMS



Three-level Architecture——DBMS要维护的==三级模式==

1. pysical level: 描述记录的物理存储方式
2. logical level：描述数据的逻辑结构
3. view level：数据库的一部分，属于每个用户的 view 



Data Independence

1. 物理数据独立性：在对物理模式进行修改时无需对逻辑模式进行修改==完备的==
2. 逻辑数据独立性：在 view 不改变的情况下对逻辑模式进行修改==不完备==



mappings两级映像

![image-20220906151924596](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220906151924596.png)





DBMS 可以类比为对数据库的操作系统



## 数据库体系结构

----

![image-20220913131905237](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220913131905237.png)

### DATABASE

data：数据库存储的对象

indices：index 也需要物理存储

data dictionary：存储三级模式、两级映像、用户权限，又叫元数据（关于数据的数据）。用户在外部能看到的系统表，是数据字典的一个 view，在操作系统为应用分配内存时，数据字典的空间不会被置换（非常重要！！！）

### DBMS

#### 存储管理

buffer、file manager

事务管理

#### 查询处理

（sql 是说明性语句，DBMS 收到 sql 不能直接运行）

在这个部分将 sql 转化为可执行内容（编译）

### APPLICATION

前端应用，提交请求给 DBMS，由 DBMS 访问数据库

### C/S 架构

![image-20220913133857182](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220913133857182.png)

两层：胖客户端结构，服务器端仅为数据库系统，因此客户端不仅要实现通信，还要实现业务功能

三层（B/S）：浏览器-业务逻辑服务器-数据库服务器，在这个结构中，业务逻辑服务器是数据库系统的client