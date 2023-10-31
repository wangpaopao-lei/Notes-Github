# MongoDB 数据库技术实验报告







<center>
  姓名：王磊
</center>




<center>
  学号：2020211538
</center>



<center>
  提交日期：2023-5
</center>






## Contents

[toc]





## 实验环境

平台：MacOS ventura

数据库版本：cassandra-latest

其它工具：Docker v4.19, DataGrip, VS Code

![image-20230523233611278](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230523233611278.png)



## 实验内容

### C1 创建键空间

**创建两个键空间**

```cql
// 键空间 1
CREATE KEYSPACE hotel_538 WITH replication = { 'class' : 'SimpleStrategy', 'replication_factor' : 1 };
// 键空间 2
CREATE KEYSPACE reservation_538 WITH replication = { 'class' : 'SimpleStrategy', 'replication_factor' : 1 };
```

终端未报错，创建成功

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230524151230437.png" alt="image-20230524151230437" style="zoom:33%;" />

### C2 创建相关表

**在 hotel 键空间中建表**

```cql

CREATE TYPE hotel_538.address (
    street text,
    city text,     
    state_or_province text,     
    postal_code text,     
    country text
);

CREATE TABLE hotel_538.hotels_by_poi (
    poi_name text,     
    hotel_id text,     
    name text,     
    phone text,     
    address frozen<address>,     
    PRIMARY KEY ((poi_name), hotel_id)
) WITH comment = 'Q1. Find hotels near given poi'
AND CLUSTERING ORDER BY (hotel_id ASC) ;

CREATE TABLE hotel_538.hotels (
    id text PRIMARY KEY,     
    name text,     
    phone text,     
    address frozen<address>,     
    pois set<text>
) WITH comment = 'Q2. Find information about a hotel';

CREATE TABLE hotel_538.pois_by_hotel (
    poi_name text,     
    hotel_id text,     
    description text,     
    PRIMARY KEY ((hotel_id), poi_name)
) WITH comment = 'Q3. Find pois near a hotel';

CREATE TABLE hotel_538.available_rooms_by_hotel_date (
    hotel_id text,     
    date date,     
    room_number smallint,     
    is_available boolean,     
    PRIMARY KEY ((hotel_id), date, room_number)
) WITH comment = 'Q4. Find available rooms by hotel / date';

CREATE TABLE hotel_538.amenities_by_room (
    hotel_id text,     
    room_number smallint,     
    amenity_name text,     
    description text,     
    PRIMARY KEY ((hotel_id, room_number), amenity_name)
) WITH comment = 'Q5. Find amenities for a room';



CREATE TYPE reservation_538.address (
    street text,    
    city text,     
    state_or_province text,     
    postal_code text,    
    country text
);

CREATE TABLE reservation_538.reservations_by_hotel_date (
    hotel_id text,     
    start_date date,     
    end_date date,     
    room_number smallint,     
    confirm_number text,     
    guest_id uuid,     
    PRIMARY KEY ((hotel_id, start_date), room_number)
) WITH comment = 'Q8. Find reservations by hotel and date';

CREATE MATERIALIZED VIEW reservation_538.reservations_by_confirmation AS SELECT * FROM reservation_538.reservations_by_hotel_date WHERE confirm_number IS NOT NULL and hotel_id IS NOT NULL and start_date IS NOT NULL and room_number IS NOT NULL PRIMARY KEY (confirm_number, hotel_id, start_date, room_number);

CREATE TABLE reservation_538.reservations_by_guest (
    guest_last_name text,     
    hotel_id text,    
    start_date date,     
    end_date date,     
    room_number smallint,    
    confirm_number text,     
    guest_id uuid,     
    PRIMARY KEY ((guest_last_name), hotel_id, guest_id)
) WITH comment = 'Q7. Find reservations by guest name';

CREATE TABLE reservation_538.guests (
    guest_id uuid PRIMARY KEY,
    first_name text,     
    last_name text,    
    title text,     
    emails set<text>,    
    phone_numbers list<text>,     
    addresses map<text, frozen<address>>,     
    confirm_number text
) WITH comment = 'Q9. Find guest by ID';
```

在数据库管理工具datagrip 里显示已创建成功

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230524151853557.png" alt="image-20230524151853557" style="zoom:33%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230524153144402.png" alt="image-20230524153144402" style="zoom:33%;" />

### C3 在 reservation 的三张表中插入数据

**在 cqlsh 中运行如下插入数据语句**

```cql
// 插入reservations_by_hotel_date表
INSERT INTO reservation_538.reservations_by_hotel_date (hotel_id, start_date, end_date, room_number, confirm_number, guest_id)
VALUES ('hotel1', '2023-06-01', '2023-06-03', 101, 'CONFIRM001', uuid());

INSERT INTO reservation_538.reservations_by_hotel_date (hotel_id, start_date, end_date, room_number, confirm_number, guest_id)
VALUES ('hotel1', '2023-06-02', '2023-06-05', 201, 'CONFIRM002', uuid());

INSERT INTO reservation_538.reservations_by_hotel_date (hotel_id, start_date, end_date, room_number, confirm_number, guest_id)
VALUES ('hotel2', '2023-06-03', '2023-06-06', 301, 'CONFIRM003', uuid());



// 插入reservations_by_guest表
INSERT INTO reservation_538.reservations_by_guest (guest_last_name, hotel_id, start_date, end_date, room_number, confirm_number, guest_id)
VALUES ('Smith', 'hotel1', '2023-06-01', '2023-06-03', 101, 'CONFIRM001', uuid());

INSERT INTO reservation_538.reservations_by_guest (guest_last_name, hotel_id, start_date, end_date, room_number, confirm_number, guest_id)
VALUES ('Johnson', 'hotel1', '2023-06-02', '2023-06-05', 201, 'CONFIRM002', uuid());

INSERT INTO reservation_538.reservations_by_guest (guest_last_name, hotel_id, start_date, end_date, room_number, confirm_number, guest_id)
VALUES ('Williams', 'hotel2', '2023-06-03', '2023-06-06', 301, 'CONFIRM003', uuid());


// 插入 guests 表
INSERT INTO reservation_538.guests (guest_id, first_name, last_name, title, emails, phone_numbers, addresses, confirm_number)
VALUES (uuid(), 'John', 'Smith', 'Mr.', {'john.smith@example.com'}, ['1234567890'], {'home': {street: '123 Main St', city: 'New York', state_or_province: 'NY', postal_code: '10001', country: 'USA'}}, 'CONFIRM001');

INSERT INTO reservation_538.guests (guest_id, first_name, last_name, title, emails, phone_numbers, addresses, confirm_number)
VALUES (uuid(), 'Sarah', 'Johnson', 'Ms.', {'sarah.johnson@example.com'}, ['9876543210'], {'home': {street: '456 Elm St', city: 'Los Angeles', state_or_province: 'CA', postal_code: '90001', country: 'USA'}}, 'CONFIRM002');

INSERT INTO reservation_538.guests (guest_id, first_name, last_name, title, emails, phone_numbers, addresses, confirm_number)
VALUES (uuid(), 'Michael', 'Williams', 'Mr.', {'michael.williams@example.com'}, ['5555555555'], {'home': {street: '789 Oak St', city: 'Chicago', state_or_province: 'IL', postal_code: '60601', country: 'USA'}}, 'CONFIRM003');


```



终端运行结果

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230524162732199.png" alt="image-20230524162732199" style="zoom:25%;" />

在 datagrip 中，可以看到数据已经插入成功

- guests 表
- <img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230524162828617.png" alt="image-20230524162828617" style="zoom:33%;" />
- reservations_by_guest表
- <img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230524162902463.png" alt="image-20230524162902463" style="zoom: 33%;" />
- reservations_by_hotel_date表
- <img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230524162949443.png" alt="image-20230524162949443" style="zoom:33%;" />



### C4 修改客户信息

对 John Smith 的 emails、phone_numbers、address 进行修改，将这些字段都改为两个

**首先查询到此人的 guest_id**

```cql
select guest_id from reservation_538.guests where first_name='John' and last_name='Smith' allow filtering;
```

显示结果

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230525094623066.png" alt="image-20230525094623066" style="zoom:50%;" />

**通过 guest_id 修改客户信息**

```cql
UPDATE reservation_538.guests
SET emails = emails + {'johnsmith@gmail.com'},
    phone_numbers = phone_numbers + ['9876543210'],
    addresses = addresses + {'work': {street: '456 Elm St', city: 'Los Angeles', state_or_province: 'CA', postal_code: '90001', country: 'USA'}}
where guest_id=9ff33e4b-9798-4bca-892e-695233a30b37;
```

显示结果

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230525110245501.png" alt="image-20230525110245501" style="zoom:50%;" />

在 datagrip 中查看

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230525110330043.png" alt="image-20230525110330043" style="zoom:50%;" />

### C5 查询数据

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230525110502873.png" alt="image-20230525110502873" style="zoom:50%;" />

#### Q7 查找 hotel2酒店、2023-06-03日开始、Williams 的预定

```cql
expand on;
select * from reservation_538.reservations_by_guest 
where hotel_id='hotel2' and start_date='2023-06-03' and guest_last_name='Williams' allow filtering;
```

显示结果

<img src="/Users/wangpaopaopao/Library/Application%20Support/typora-user-images/image-20230526152603577.png" alt="image-20230526152603577" style="zoom:50%;" />

#### Q8 查找 Williams 的所有预定

```cql
select * from reservation_538.reservations_by_guest
where guest_last_name='Williams' allow filtering;
```

显示结果

<img src="/Users/wangpaopaopao/Library/Application%20Support/typora-user-images/image-20230526152934611.png" alt="image-20230526152934611" style="zoom:50%;" />

#### Q9 查看 Williams 的详细信息

```cql
select * from reservation_538.guests
where last_name='Williams' allow filtering;
```

显示结果

<img src="/Users/wangpaopaopao/Library/Application%20Support/typora-user-images/image-20230526153059204.png" alt="image-20230526153059204" style="zoom:50%;" />











### C6 数据导出

**将 guests 表导出到桌面**

```
copy reservation_538.guests(guest_id,first_name,last_name) to '/etc/cassandra/data/guests.csv'
```

显示结果，导出成功

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230526123220169.png" alt="image-20230526123220169" style="zoom:50%;" />



### C7 数据导入

**新建一张表，名为 guests_new**

```cql
CREATE TABLE reservation_538.guests_new (
    guest_id uuid PRIMARY KEY,
    first_name text,     
    last_name text,    
);
```

**将 guests.csv 中的数据导入到 guests_new 表中**

```cql
COPY reservation_538.guests (guest_id, first_name, last_name) FROM '/etc/cassandra/data/guests.csv' WITH HEADER = true;
```

显示结果

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230526124000639.png" alt="image-20230526124000639" style="zoom:50%;" />



### C8 数据删除

#### 1 删除 John Smith 客户的 '9876543210' 这条电话号码

1. 找到 John Smith 的 guest_id
2. 找到 John Smith 的 phone_numbers 列，得到删除'9876543210'后的值
3. 使用 update 语句更新

**运行如下语句**

```cql
select guest_id 
from reservation_538.guests 
where first_name='John' and last_name='Smith' allow filtering;
```

找到 id

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230526143857772.png" alt="image-20230526143857772" style="zoom:50%;" />

**运行如下语句**

```cql
select phone_numbers 
from reservation_538.guests 
where guest_id=9ff33e4b-9798-4bca-892e-695233a30b37;
```

找到 phone_numbers

![image-20230526144018803](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230526144018803.png)

得到删除指定元素后的值为`['1234567890']`

**运行如下语句**

```cql
update reservation_538.guests 
set phone_numbers=['1234567890'] 
where guest_id=9ff33e4b-9798-4bca-892e-695233a30b37;
```

显示结果

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230526144323863.png" alt="image-20230526144323863" style="zoom:50%;" />

已经成功删除指定元素

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230526144401488.png" alt="image-20230526144401488" style="zoom:50%;" />

#### 2 删除 John Smith 这个客户 address 列的内容

**使用 update 语句将这列的值设为空**

```cql
UPDATE reservation_538.guests 
SET addresses = NULL 
WHERE guest_id =9ff33e4b-9798-4bca-892e-695233a30b37;
```

查看结果

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230526144718909.png" alt="image-20230526144718909" style="zoom:50%;" />

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230526144750307.png" alt="image-20230526144750307" style="zoom:50%;" />

#### 3 删除Smith 在 hotel1 的订单

**使用 delete 语句**

```cql
delete from reservation_538.reservations_by_guest 
where guest_last_name='Smith' and hotel_id='hotel1';
```

删除成功

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230526145121897.png" alt="image-20230526145121897" style="zoom:50%;" />

## C9 Python 访问 Cassandra

![image-20230526145507998](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230526145507998.png)

### 1 2、配置环境

**使用 pip install 安装相关驱动**

![image-20230526145842128](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230526145842128.png)

![image-20230526145837161](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230526145837161.png)

### 3 链接键空间

**由于我使用 docker 运行 Cassandra 时，使用了 9046:9042 的端口映射，因此使用 Cluster 构造器的参数来创建连接**

```Python
cluster = Cluster(['localhost'], port=9046)
session = cluster.connect('hotel_538')
```

连接成功

![image-20230526150930388](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230526150930388.png)

### 4 CRUD 操作

#### 插入数据

**我选择hotel_538 键空间中的 hotels 表，向其中插入三条数据**

```Python
session.execute("INSERT INTO hotel_538.hotels (id, name, phone, address, pois)
VALUES ('1', 'Hotel A', '1234567890', {street: 'Street A', city: 'City A', state_or_province: 'State A', postal_code: '12345', country: 'Country A'}, {'pool', 'gym'});")

session.execute("INSERT INTO hotel_538.hotels (id, name, phone, address, pois)
VALUES ('2', 'Hotel B', '9876543210', {street: 'Street B', city: 'City B', state_or_province: 'State B', postal_code: '54321', country: 'Country B'}, {'restaurant', 'bar'});")

session.execute("INSERT INTO hotel_538.hotels (id, name, phone, address, pois)
VALUES ('3', 'Hotel C', '5555555555', {street: 'Street C', city: 'City C', state_or_province: 'State C', postal_code: '67890', country: 'Country C'}, {'spa', 'parking'});")
```

 运行结果

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230526155031492.png" alt="image-20230526155031492" style="zoom:50%;" />

在 datagrip 中查看，插入成功

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230526155127609.png" alt="image-20230526155127609" style="zoom:50%;" />

#### 查询数据

**查询名为 hotel A 的酒店的所有信息**

```python
session.execute("select * from hotel_538.hotels where name='Hotel A' allow filtering;")
```

显示结果

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230526155403238.png" alt="image-20230526155403238" style="zoom:50%;" />

#### 修改数据

**将 Hotel A 更名为 Hotel Alpha**

1. 找到 Hotel A 的分区键 id
2. 通过 id 修改 name 字段

```Python
id=session.execute("select id from hotel_538.hotels where name='Hotel A' allow filtering;")
for r in id:
    print(r)
    
session.execute("update hotel_538.hotels set name='Hotel Alpha' where id='1';")
```

显示结果

<img src="/Users/wangpaopaopao/Library/Application%20Support/typora-user-images/image-20230526155844925.png" alt="image-20230526155844925" style="zoom:50%;" />

<img src="/Users/wangpaopaopao/Library/Application%20Support/typora-user-images/image-20230526155906445.png" alt="image-20230526155906445" style="zoom:50%;" />

#### 删除数据

**（行删除）删除名为 Hotel Alpha 的酒店信息**

```Python
session.execute("delete from hotel_538.hotels where id='1';")
```

显示结果

<img src="/Users/wangpaopaopao/Library/Application%20Support/typora-user-images/image-20230526160051437.png" alt="image-20230526160051437" style="zoom:50%;" />

<img src="/Users/wangpaopaopao/Library/Application%20Support/typora-user-images/image-20230526160124498.png" alt="image-20230526160124498" style="zoom:25%;" />

