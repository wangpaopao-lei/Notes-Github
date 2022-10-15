##  Outline

![image-20221011140252879](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221011140252879.png)

## 目录

[toc]

# Intermediate SQL

## Join Expression

---

自然连接

```sql
select *  
  from course natural join prereq
```

普通连接（条件）

```sql
select *  
  from course join prereq on course.course_id=prereq. course_id
```

与 from where 其实是等价的（条件笛卡尔积）

```sql
select *      
from  student , takes        
where  student_ID  = takes_ID
```

外连接

```sql
select *  
  from course natural left outer join prereq
```

![image-20221011141637954](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221011141637954.png)

## Views视图

---

==虚表==，不会在数据库中存储

创建视图

```sql
create view faculty as    
	select ID, name, dept_name   
		from instructor
```

创建了的视图也可以作为表用于查询

但是在 DBMS 中，在执行查询之前需要检查表是基表还是视图，视图在被执行之前要被转换为查询语句（因为本质上没有存储，只是指代了一系列查询操作）

```sql
select name 
	from faculty
		where dept_name = ‘Biology’
```

因此，在创建视图时是不涉及查询操作的

更新视图

！！！视图更新是受限制的，创建视图的目的是查询而非维护

```sql
insert into faculty values (’30765’, ’Green’, ’Music’)
```

## 完整性约束

----



## 复杂的 check 语句

---





## 权限授予

---

U-R-P

用户-角色-权限

权限：

1. 系统权限
2. 数据操作权限



作业3.9,3.10，3.18,4.3,4.9,4.20