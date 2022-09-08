不要用中文教材

理论课 40 课时

实验课 8 课时

![image-20220906131747411](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220906131747411.png)



以事物为单位进行管理





数据库技术：进行数据管理的工具

前端应用——》DBMS——》数据库

应用：逻辑信息

DBMS：为用户屏蔽底层操作





NoSql：Not only sql

为什么数据库不是面向对象的？

前端是各种各样的应用，DBMS 在中间，Database 是存储数据的地方。厂商提供服务的核心部分在于 DBMS



Three-level Architecture——DBMS要维护的三级模式

1. pysical level: 描述记录的物理存储方式
2. logical level：描述数据的逻辑结构
3. view level：数据库的一部分，属于每个用户的 view 



mappings两级映像



Data Independence

1. 物理数据独立性：在对物理模式进行修改时无需对逻辑模式进行修改==完备的==
2. 逻辑数据独立性：在 view 不改变的情况下对逻辑模式进行修改==不完备==

![image-20220906151924596](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220906151924596.png)