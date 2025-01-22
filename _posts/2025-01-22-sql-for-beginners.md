# 数据库

[toc]

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

## sql语句

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
  `email` VARCHAR(45) NULL,
  PRIMARY KEY (`id`));
```

### DML

> data manipulate language

增删改语句

1. 插入

``` sql
INSERT INTO `study-sql`.`student` (`id`, `name`, `age`, `email`) VALUES ('1', 'jack', '12', 'abc@qq.com');
```

2. 修改

```sql
UPDATE `study-sql`.`student` SET `age` = '14' WHERE (`id` = '1');
```

3. 删除

```sql
DELETE FROM `study-sql`.`student` WHERE (`id` = '1');
```

4. 清空表

```sql
TRUNCATE TABLE `study-sql`.`student`
```

5. 删除表

``` sql
DROP TABLE `study-sql`.`student`
```

### DQL

> data query language

查询语句

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
