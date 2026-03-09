---
title: Java Web MySQL 学习笔记
published: 2026-03-06
description: "MySQL 数据库基础操作及表结构设计，包含具体的增删改查 SQL 实例"
tags: ["Java", "MySQL", "数据库", "DDL", "DML", "DQL"]
category: Java
draft: false
---

# MySQL 核心基础笔记

## 1. 基础概念与登录

### 1.1 登录数据库
```bash
mysql -u[用户名] -p[密码]
```

### 1.2 SQL 简介与分类
SQL：一门操作关系型数据库的编程语言，定义操作所有关系型数据库的**统一标准**。

| 分类 | 全称                     | 说明                                   |
|------|--------------------------|----------------------------------------|
| **DDL**  | Data Definition Language | 数据定义语言，用来定义数据库对象（数据库，表，字段） |
| **DML**  | Data Manipulation Language | 数据操作语言，用来对数据库表中的数据进行增删改 |
| **DQL**  | Data Query Language      | 数据查询语言，用来查询数据库中表的记录 |
| **DCL**  | Data Control Language    | 数据控制语言，用来创建数据库用户、控制数据库的访问权限 |

---

## 2. DDL：数据库与表结构操作

### 2.1 数据库操作
```sql
show databases;            -- 显示数据库列表
select database();         -- 显示当前使用的数据库
use [数据库名];             -- 选择数据库
create database [数据库名];  -- 创建数据库
drop database [数据库名];    -- 删除数据库
```
> **提示**：同一个数据库服务器中，数据库的名字不能相同；MySQL8 版本默认的字符集是 `utf8mb4`。建议使用 DataGrip 等客户端工具。

### 2.2 表操作 (创建)

#### 创建表语法
```sql
create table tablename(
    字段1 类型 [约束] [comment 注释],
    字段2 类型 [约束] [comment 注释]
)[comment 表注释];
```

**示例：创建一个无约束的信息表**
```sql
create table test(
    id int comment '学生ID，唯一标识',
    username varchar(50) comment '用户名',
    name varchar(10) comment '姓名',
    age int comment '年龄',
    gender char(1) comment '性别'
)comment '信息表';
```

#### 常见约束说明
保证数据库中数据的正确性、有效性和完整性：

| 约束       | 描述                                   | 关键字       |
|------------|----------------------------------------|--------------|
| **非空约束** | 限制该字段值不能为 null                 | `not null`   |
| **唯一约束** | 保证字段的所有数据都是唯一、不重复的   | `unique`     |
| **主键约束** | 一行数据的唯一标识，要求非空且唯一       | `primary key`|
| **默认约束** | 保存数据时，如果未指定该字段值，则采用默认值 | `default`    |
| **外键约束** | 让两张表的数据建立连接，保证数据一致性 | `foreign key`|

**示例：带有约束的创建表语句**
```sql
create table test(
  id int primary key auto_increment comment '学生ID，唯一标识',
  username varchar(50) unique not null comment '用户名',
  name varchar(10) not null comment '姓名',
  age int not null comment '年龄',
  gender char(1) not null default '男' comment '性别'
)comment '信息表';
```
> **注意**：在主键关键字后加入 `auto_increment`，可以让 MySQL 自动递增生成唯一值。

### 2.3 常用数据类型
1. **数值类型**：`tinyint` (1b), `smallint` (2b), `int` (4b), `bigint` (8b), `double`/`decimal(总长度,小数位)`。
   - *提示*：在满足业务前提下，尽量选择占用空间较小的数据类型，如年龄使用 `tinyint`。
2. **字符串类型**：
   - `char(n)`: 定长，性能略高，但浪费磁盘空间
   - `varchar(n)`: 变长，节约磁盘空间，性能略低
   - `text` / `longtext` / `blob`
3. **日期和时间类型**：`date` (YYYY-MM-DD), `datetime` (YYYY-MM-DD HH:MM:SS), `timestamp`

### 2.4 修改与删除表
```sql
show tables; -- 查询当前数据库的所有表
desc 表名; -- 查询表结构
show create table 表名; -- 查询建表语句

alter table 表名 add 字段名 类型(长度) [comment 注释] [约束]; -- 添加字段
alter table 表名 modify 字段名 新数据类型(长度); -- 修改字段类型
alter table 表名 change 旧字段名 新字段名 类型(长度) [comment 注释] [约束]; -- 修改字段名与类型
alter table 表名 drop column 字段名; -- 删除字段
alter table 表名 rename to 新表名; -- 修改表名

drop table [if exists] 表名; -- 删除表
```

---

## 3. DML：数据操作语法

### 3.1 添加数据 (INSERT)
```sql
-- 指定字段插入
insert into emp(username, password, name, gender, phone) 
values ('songjiang','12345678','宋江',1,'13300001111');

-- 全字段插入方案 1 (补全字段名)
insert into emp(id, username, password, name, gender, phone, job, salary, entry_date, image, create_time, update_time)
values(null, 'linchong','12345678','林冲',1,'13300001112',1,6000,'2020-01-01','1.jpg',now(),now());

-- 全字段插入方案 2 (直接填值)
insert into emp values(null, 'likui','12345678','李逵',1,'13300001113',1,6000,'2020-01-01','1.jpg',now(),now());

-- 批量插入多条记录
insert into emp(username, password, name, gender, phone) values
('ruanxiaoer','12345678','阮小二',1,'13300001114'),
('ruanxiaowu','12345678','阮小五',1,'13300001115');
```

### 3.2 修改数据 (UPDATE)
```sql
-- 1. 将 emp 表的ID为1的员工，修改用户名和姓名
update emp set username = 'zhangsan' , name = '张三' where id = 1;

-- 2. 将 emp 表的所有员工入职日期更新
update emp set entry_date = '2010-01-01';

-- 3. 所有人修改薪水+1000
update emp set salary = salary + 1000;
```

### 3.3 删除数据 (DELETE)
```sql
delete from 表名 [where 条件];
```

---

## 4. DQL：数据查询语言

完整的 DQL 语法顺序：
```sql
select 字段列表 from 表名列表 
where 条件列表 
group by 分组字段列表 having 分组后条件列表 
order by 排序字段列表 
limit 分页参数
```

### 4.1 基本查询
```sql
-- 1. 查询指定字段
select name, emp.entry_date from emp;

-- 2. 查询返回所有字段 (不建议生产环境使用)
select * from emp;

-- 3. 为查询字段设置别名 (as 可省略)
select name as '姓名', emp.entry_date as '加入日期' from emp;

-- 4. 去除重复查询记录
select distinct job from emp;
```

### 4.2 条件查询 (WHERE)
支持比较运算符 (`>`, `>=`, `<`, `<=`, `=`, `<>`/`!=`, `between...and`, `in(...)`, `is null`, `like` 占位符) 和 逻辑运算符 (`and`, `or`, `not`)。

```sql
-- 范围查询
select * from emp where entry_date between '2010-01-01' and '2020-10-10';

-- 集合多选查询
select * from emp where job in(2,3,4);

-- 空值查询
select * from emp where job is null;

-- 模糊查询：'%'匹配任意字符, '_'匹配单个字符
select * from emp where name like '柴%';
select * from emp where name like '柴_';
```

### 4.3 聚合函数
将一列数据作为一个整体，进行纵向计算**（不参与 null 统计）**：
- `count`: 统计数量 (`count(*)` / `count(-1)` / `count(字段)`)
- `max` / `min` / `avg` / `sum`

```sql
select count(*) from emp;                -- 统计总记录(更高性能推荐)
select avg(emp.salary) from emp;         -- 平均薪资
select max(emp.salary) from emp;         -- 最高薪水
select sum(emp.salary) from emp;         -- 薪资总额
```

### 4.4 分组查询 (GROUP BY)
**`where`与`having`的区别**：
1. `where` 在分组**前**过滤，不满足则不参与分组；`having` 在分组**后**对结果进行过滤。
2. `where` 不能对聚合函数进行判断，`having` 可以。

```sql
-- 根据性别分组并统计数量
select gender, count(gender) from emp group by gender;

-- 也可以配合 where 联合使用
select job, count(job) from emp where entry_date <= '2015-01-01' group by job;
```
> *注意*：分组之后，select 的对象一般是分组字段和聚合函数。

### 4.5 排序查询 (ORDER BY)
```sql
-- 排序方式：asc(升序-默认)，desc(降序)
order by 排序字段 [排序方式];
```
> *注意*：多字段排序时，当第一个字段值一样，才会根据第二个字段排列。

### 4.6 分页查询 (LIMIT)
```sql
limit 起始索引, 查询记录数;
```
> **核心说明**：
> 1. 起始索引从 $0$ 开始。如果为 $0$，可简写为 `limit 10`。
> 2. 分页是方言，MySQL 特有的是 `limit`。
> 3. **计算公式**：起始索引 = `(页码 - 1) * 每页展示记录数`

---

*最后更新：2026年3月9日*