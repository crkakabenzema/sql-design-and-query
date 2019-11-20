## SQL数据库设计的三范式：

开发中，最为 常见的设计范式有三个：

### 1.第一范式（确保每列保持原子性）：

数据库表中的所有字段值都是不可分解的原子值，则满足了第一范式。

### 2.第二范式（确保表中的每列都和主键相关）：

确保表中的每一列都和主键相关，而不能至于只与主键的某一部分相关（针对联合主键而言）

### 3.第三范式（确保每列都和主键列直接相关，而不是间接相关）:



## SQL数据库主键的选取：

1.主键和外键的设计原则。
a. 主键应尽量分离于业务的。
b. 主键应尽量是单列的，以便提高筛选和连接的效率。
c. 主键不应该被更新，且不含动态变化的数据。
d. 主键应是有计算机自动生成的。 



## SQL数据库中的五种约束：

### 1.主键约束：

唯一性，非空性

sql: 添加主键约束，将UserId设为主键

```mysql
alter table UserId

add constraint PK_UserId primary key (UserId);
```

### 2.唯一约束：

唯一性，可以空，但只有一个

sql: 添加唯一约束，每个人的id不一样

```mysql
alter table UserInfo

add constraint UQ_IDNumber unique (IdentityCardNumber);
```

### 3.检查约束：

对该列数据的范围、格式的限制

sql: 添加检查约束，对年龄范围加以限定

```mysql
alter table UserInfo

add constraint CK_UserAge check (UserAge between 20 and 30)   
```

### 4.默认约束：

该数据的默认值

sql: 设置地址默认值

```mysql
alter table UserInfo

add contraint DF_UserAddress default ('地址不详') for UserAddress;
```

### 5.外键约束：(不推荐，增加数据库开销)

建立两表间的关系并引用主表的列

sql: 主表UserInfo和从表UserOrder建立关系，关联字段UserId

```mysql
alter table UserOrder

add constraint FK_UserId 

foreign key(UserId) 

references UserInfo(UserId) 

on update/delete  restrict/cascade/set null/no action;
```

附：on delete/update 规则：cascade (操作主表的同时，操作外键表)

​                             no action (删除主表某行时，如该行的键被其他表现有行的外键引用，产生错误并回滚delete语句，在Mysql，等同于restrict)

​                             restrict (拒绝父表操作)

​                            set null (操作主表时，将子表对应外键列设置为null) 



## SQL数据库多表查询：

注1：mysql只对简单表提供高速缓冲区域（cache buffering），init.ora为高速缓冲区域设置合适的参数。

注2：在from子句包含多个表的情况下，选择记录条数最少的表作为基础表，当处理多表时，按照<u>从右到左</u>的顺序，运用排序及合并的方式连接。

注3：执行sql语句时，减少访问数据库的次数

### 1. 交叉查询：（笛卡尔集，即两个表的乘积）

```mysql
select * from db1 and db2;
```

### 2.自身连接查询：数据库自己连接自己

```mysql
select A.property1, B.property1 from db A, db B where Condition;
```

### 3. 内连接查询：即两个表的交集

```mysql
select e.property1,d.property2
from db e
INNER JOIN db1 d
ON Condition;
```

### 4. 左外连接查询：左边表加两表交集

即包含左表所有行，加没有匹配的对应右表部分全为空

```mysql
select * from db left join db1 on Condition; 
```

### 5. 右外连接查询：右边表加两表交集C

即包含右表所有行，加没有匹配的对应左表部分全为空

```mysql
select * from db right join db1 on Condition;
```

### 6.完全外连接查询： 包含full join左右两表中所有的行，

如果右表中某行在左表中没有匹配，则结果中对应行右表的部分全部为空(NULL)，如果左表中某行在右表中没有匹配，则结果中对应行左表的部分全部为空(NULL) 

```mysql
select * from db inner join db1 on Condition;
```

### 7. 子查询：即查询中还有查询 

```mysql
select param1 from db where param2 in/>/</= (select param2 from db where param2 Condition);
```

注：聚合函数不可以用在条件中

注：带有exists的sql: 

```mysql
select param1 from db where exists (...), exists;
```

 返回 true or false 。为提高效率，尽量用exists代替in, no exists代替not in,

注：集合运算的运算符：intersect 交集， union 并集， except 差集， union all 并集且不去除重复集

注：减少对表的查询，如 ：

```mysql
`SELECT TAB_NAME` 
`FROM TABLES` 
`WHERE  (TAB_NAME,DB_VER)` 
`= ( SELECT TAB_NAME,DB_VER)` 
`FROM TAB_COLUMNS` 
`WHERE Condition;
```



## NOSQL数据库：                          













