## Outline

![image-20220920152600761](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220920152600761.png)

## 目录

[toc]

# SQL

## DDL

create table

```sql
create table instructor (              ID             char(5),             name           varchar(20) not null,                      dept_name      varchar(20),                      salary         numeric(8,2),                      primary key (ID),                      foreign key (dept_name) references department);
```

r：关系名称

A：属性名称

D：属性取值类型

()：完整性约束



drop table

表删除



alter table

修改模式、修改结结构而非修改表里的数据

```sql
alter table r add A D
```

## Basic Structure__query

### 查询


$$
\prod_{A 1, A 2, \ldots, A n}\left(\sigma_P\left(r_1 \times r_2 \times \quad \ldots \quad \times \quad r_m\right)\right)
$$
```sql
select A1, A2, ..., An
	from r1, r2, ..., rm
	where P
```

输出所有属性

```sql
select *
	from instructor
```

投影时与实数进行计算

```sql
select ID, name, salary/12
	from instructor
```

投影时去重

````sql
select distinct dept_name	
	from instructor
````

where 语句可以使用 between

```sql
select name
	from instructor
	where salary between 90000 and 100000
```

where 语句可以使用元组比较

`,`用于表的笛卡尔乘积操作

`instructor.ID = teaches.ID` ==作为隐式条件，非常重要！！==

```sql
select name, course_id
	from instructor, teaches
	where (instructor.ID, dept_name) = (teaches.ID, ’Biology’);
```

### 更名

```sql
old_name as new_name 
```

### 字符串操作

`%`匹配任意多个字符

`_`匹配一个字符

```sql
select name
	from instructor
	where name like '%dar%' 
```

如果%在字符中是有意义的

```sql
like 'Main\%'  escape  '\' 
```

### 输出结果排序

默认为升序

`desc`限定为降序

```sql
select distinct name
	from    instructor
	order by name desc
```

### 集合操作

默认包含去重

`all`输出不去重结果

```sql
(select course_id from section where sem = ‘Fall’ and year = 2009)
union all
(select course_id from section where sem = ‘Spring’ and year = 2010)
```

### 聚集函数

avg 求平均值

```sql
select avg (salary)
	from instructor
	where dept_name= ’Comp. Sci.’;
```



count 求记录数

```sql
select count (distinct ID)
	from teaches
	where semester = ’Spring’ and year = 2010;
```

count* 求整张表的记录数量

```sql
select count (*)
	from course;
```

group by按照属性进行分组，针对每个组进行聚集函数的操作

==如果有 groupby，输出只能为 groupby 的属性以及聚集函数==

```sql
select dept_name, avg (salary) as avg_salary
	from instructor
	group by dept_name;
```

having 指定分组条件，在分组之后对组的筛选，只会跟在 groupby 的后面

```sql
select dept_name, avg (salary)
	from instructor
	group by dept_name
	having avg (salary) > 42000;
```

嵌套查询

### 删除

delete from 进行删除

where里可以嵌套条件

```sql
delete from instructor
	where dept name in (
    select dept name                                 			from department                                  			where building = ’Watson’);
```

### 插入

insert into

最好把属性名称写上，这样系统可以自动填充空值

```sql
insert into course (course_id, title, dept_name, credits)
	values (’CS-437’, ’Database Systems’, ’Comp. Sci.’, 4);
```

把查询结果插入

```sql
insert into student
	select ID, name, dept_name, 0         
		from   instructor
```

### 更新



```sql
update instructor  
	set salary = salary * 1.03
	where salary > 100000;  

update instructor 
	set salary = salary * 1.05 
	where salary <= 100000;
```

支持 case 语句

```sql
update instructor               		set salary = case    
		when salary <= 100000 then salary * 1.05
    		else salary * 1.03                                   				 end

```

