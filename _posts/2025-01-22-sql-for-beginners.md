---
title: 数据库
tags: 数据库 SQL
---

## 类型介绍

1. 关系型数据库(`Relational Database`): `mysql`, `PostgreSQL`, `oracle`...
2. 非关系型数据库(`NoSQL`): `mongoDB`, `redis`...
3. 图形数据库(`Graph Database`): `Neo4j`...
4. 列式数据库(`Columnar Databases`): `Apache HBase, Google BigQuery.`, 区别于`mysql`, 数据存储在列中，查询会更快，因为不需要读取整行，而只需要读取具体的列

```sql
ID:   1, 2, 3
Name: Alice, Bob, Eve
Age:  30, 25, 28
City: London, New York, Paris
```
<!--more-->

## sql语句

> [mysql 官网学习文档](https://dev.mysql.com/doc/refman/8.0/en/select.html)

### DDL

> data definition language

创建数据库、创建`table`的语句

1. 创建数据库(`database`): `CREATE SCHEMA study-sql`
2. 创建表(`table`)

``` sql
CREATE TABLE `study-sql`.`student` (
  `id` INT NOT NULL,
  `name` VARCHAR(45) NOT NULL,
  `age` INT NOT NULL,
  `class` VARCHAR(45) NOT NULL COMMENT '班级名',
  `score` VARCHAR(45) NOT NULL COMMENT '分数',
  `email` VARCHAR(45) NULL,
  PRIMARY KEY (`id`));
```

### DML

> data manipulate language

增删改语句

- 插入

``` sql
INSERT INTO `study-sql`.`student` (`id`, `name`, `age`, `class`, `score`, `email`) VALUES ('1', 'jack', '12', '一班', 60 'abc@qq.com');
```

- 修改

```sql
UPDATE `study-sql`.`student` SET `age` = '14' WHERE (`id` = '1');
```

- 删除

```sql
DELETE FROM `study-sql`.`student` WHERE (`id` = '1');
```

- 清空表

```sql
TRUNCATE TABLE `study-sql`.`student`
```

- 删除表

``` sql
DROP TABLE `study-sql`.`student`
```

### DQL

> data query language

[查询语句](https://dev.mysql.com/doc/refman/8.0/en/built-in-function-reference.html)

- 条件查询

```sql
select name as 姓名, age as 年龄 from student when age >= 10 and age <= 30;
```

- 模糊查询: 查询出所有名字以`李`开头的学生

```sql
select * from student where name like '李%';
```

- 从指定集合查询: `in`, `not in`, `between A and B`

```sql
-- 查询出名字为张三或者李四的学生
select * from student where name in ('张三', '李四');
-- 查询出id在1到10之间的学生
select * from student where id between 1 and 10;
```

- 查询限制: `limit number`, `limit offset, number`

```sql
-- 前10条
select * from student limit 10;
-- 第20-30条
select * from student limit 20, 30;
```

- 排序: 指定排序 `order by`

```sql
select * from student order by age desc, id asc;
```

- 分组: `group by`

```sql
-- * 代表当前行
select class as 班级, count(*) as 人数 from student group by class order by 人数;
```

- 函数: `AVG`, `COUNT`, `SUM`...

```sql
select class as 班级, AVG(score) as 平均分 from student group by class order by 平均分 desc;
```

- 聚合查询: 去重 `distinct`

```sql
-- 去重
select distinct class from student;

-- 聚合
select class as '班级', avg(score) as '平均分', count(*) as '人数', sum(score) as '班级总分', min(score) as '最低分', max(score) as '最高分' from student_new group by class order by '最高分';
```

- 字符串函数: `concat`, `substr`, `length`, `upper`, `lower`, mysql的下标从1开始, `substr('一二三', 2, 3)`的结果是 二三

- 数值函数: `round`, `ceil`, `floor`, `abs`, `mod`

- 日期函数: `year`, `month`, `day`, `time`

- 条件函数: `if`

```sql
select name, if(score >=60, '及格', '不及格') from student;
```

- 其他函数: `nullif`, `coalesce`, `greatest`, `least`

- 类型转换函数: `date_format`, `str_to_date`

```sql
-- 2024年01月21日
select date_format('2024-01-21', '%Y年%m月%d日');
-- 2024-01-12
select str_to_date('2024年01月12日', '%Y年%m月%d日')
```

## mysql

### mysql名词

1. 数据库(`database/schema`):
2. 表格(`table`):
   1. 主键(`primary key`):
   2. 自增(`auto increment`):
3. 视图(`views`):
4. 存储过程(`stored producer`):
5. 函数(`function`):

### mysql 类型

1. `INT`：存储整数

2. `VARCHAR(100)`: 存储变长字符串，可以指定长度

3. `CHAR`：定长字符串，不够的自动在末尾填充空格

4. `DOUBLE`：存储浮点数

5. `DATE`：存储日期 `2025-01-22`

6. `TIME`：存储时间 `14:39`

7. `DATETIME`：存储日期和时间 `14:39 14:39`

### 默认参数

| Function | Description |
|----------|------------|
| `CURRENT_TIMESTAMP` | Returns the current date and time (`YYYY-MM-DD HH:MM:SS`). |
| `NOW()` | Similar to `CURRENT_TIMESTAMP`, returns the current date and time. |
| `CURDATE()` | Returns the current date (`YYYY-MM-DD`). |
| `CURTIME()` | Returns the current time (`HH:MM:SS`). |
| `UTC_TIMESTAMP` | Returns the current UTC date and time. |

### 基本操作

### 数据查询语句

## mongoDB

## redis

## 数据库设计

## 最佳实践
