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

[练习](https://leetcode.cn/problemset/database/)

- 条件查询: `where`一般用于过滤原始数据, `having`一般用于过滤分组后的数据

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

- 聚合函数: `AVG`, `COUNT`, `SUM`...

```sql
select class as 班级, AVG(score) as 平均分 from student group by class order by 平均分 desc;
```

- 聚合查询: 去重 `distinct`

```sql
-- 去重
select distinct class from student;

-- 聚合
select class as '班级', avg(score) as '平均分', count(*) as '人数', sum(score) as '班级总分', min(score) as '最低分', max(score) as '最高分'
  from student
  group by class
  order by '最高分';
  having score >= 60
```

- 字符串函数: `concat`, `substr`, `length`, `upper`, `lower`, mysql的下标从1开始, `substr('一二三', 2, 3)`的结果是 二三

- 数值函数: `round`, `ceil`, `floor`, `abs`, `mod`

- 日期函数: `year`, `month`, `day`, `time`

- 条件函数: `if` `exists` `not exists`

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

- 子查询: `where`后放入查询语句

```sql
select name, class from student where score=(select max(score) from student);
```

- join查询: `join on`

`join on`默认就是`inner join on`: 只返回两个表中能关联上的数据

`left join on`: 额外返回左表中没有关联上的数据 (比如`user`没有对应的`card`, 比`join on`多的数据是左表`user`中的内容)

`right join on`: 额外返回右表中没有关联上的数据 (比如`card`没有`user_id`, 比`join on`多的数据是右表`id_card`中的内容)

在`from`后的是左表, `join` 后的是右表。

假设有两张表`user`, `id_card`, 其中`id_card.user_id` = `user.id`

字段

`user`: `id name`

`id_card`: `id card_name user_id`

```sql
select * from user join id_card on user.id = id_card.user_id;

-- 结果简化
select user.id, name, id_card.id as card_id, id_card.card_name as card_name
  from user
  inner join id_card on user.id = id_card.user_id;
```

条件查询: 查询出出所有有员工存在的部门

```sql
select name from department
  where exists (
   select * from employee where department.id = employee.department_id
  )
```

- **注意点**

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

### 联表

- 外键: 上述`dql`中联合查询部分里, `id_card`的`user_id`字段就是外键，与`user`表的`id`字段相关联

1. 关联更新方式
   1. 当`user`中的数据删除时, 对应的`id_card`可以设置删除模式, 默认是`RESTICT`, 意味着, 只有`id_card`中外键没有值指向该`user.id`时, 才允许当前user删除
   2. `CASCADE`: 主表记录修改, 对应关联的从表数据也修改, 主表记录删除, 从表关联的数据也删除
   3. `SET NULL`: 主表主键`user.id`的记录删除, 从表关联的外键`id_card.user_id`置为`null`
   4. `NO ACTION`: `mysql`中同`RESTRICT`
2. 一对一、一对多:

3. 多对多: 文章与标签

字段

`article`: `id title content`

`tag`: `id name`

`article_tag`: `article_id tag_id` 将`article_id`与`article.id`关联 `tag_id`与`tag.id`关联, 更新方式设置为`CASCADE`

查询 `tag_id`为1的所有文章

```sql
select * from article
  join article_tag
  on article.id = article_tag.article_id
  join tag
  on article_tag.tag_id = tag.id
  where tag_id=1;
```

## 最佳实践
