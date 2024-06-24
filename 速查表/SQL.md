# 检索

## SELECT

```sql
SELECT column1,column2 FROM table_name;
SELECT * FROM table_name;
```

### 检索不同的值

作用于所有的列，都一样才合并

```sql
SELECT DISTINCT column1,column2 FROM Products;
```

### 限制输出数量

*`MySQL`、 `MariaDB`、 `PostgreSQL` 或 `SQLite`*

```sql
SELECT prod_name FROM Products
    LIMIT 5;
```

取 6~8 行

```sql
SELECT prod_name FROM Products
    LIMIT 3 OFFSET 5;
-- MySQL、 MariaDB 和 SQLite
SELECT prod_name FROM Products
    LIMIT 5,3;
```

*`SQL Server`*

```sql
SELECT prod_name FROM Products
    TOP 5;
```

## 排序 ORDER BY

```sql
SELECT prod_name FROM Products
    ORDER BY prod_name;
```

可以用非检索的列排序。

先按 `col_a` 再按 `col_b` 排序:

```sql
SELECT col_c FROM table_a
    ORDER BY col_a,col_b;
```

按相对列位置

```sql
-- 按 col_b 和 col_c 排序
SELECT col_a,col_b,col_c FROM Products
    ORDER BY 2,3;
```

降序

```sql
SELECT * FROM Products
    ORDER BY prod_price DESC, prod_name;
```

## 过滤 WHERE

```sql
SELECT prod_name FROM Products
    WHERE  vend_id != 'DLL01' AND NOT prod_price > 5;
```

先 `WHERE` 再 `ORDER BY`

| 操作符  | 说明   | 操作符          | 说明        |
| ------- | ------ | --------------- | ----------- |
| =       | 等于   | <               | 小于        |
| <>      | 不等于 | <=              | 小于等于    |
| !=      | 不等于 | !<              | 不小于      |
| IS NULL | 为空值 | BETWEEN 5 AND 8 | 在[5,8]之间 |

`OR` 操作符前面满足就会短路。

先 `AND` 后 `OR` 。用圆括号消歧义。

`IN` 操作符比一组 `OR` 更快；可以包含其他 `SELECT` 语句，动态地建立 `WHERE` 子句

```sql
WHERE vend_id IN ('DLL01','BRS01')
```

## 通配符过滤 LIKE

百分号 `%` 匹配0或多个任意字符。

```sql
SELECT prod_id, prod_name FROM Products
    WHERE prod_name LIKE '%bean bag%';
```

默认不区分大小写。

字符串末尾可能填充空格。需要在末尾加 `%` 。

`'%'`不会匹配 `NULL` 。

下划线 `_` 匹配一个任意字符。*DB2 不支持*

方括号 `[]` 匹配集合中的一个字符。*仅 `SQL Server`*

```sql
'[JM]%'
```

## 计算字段

### 拼接

| DBMS                              | 操作符     |
| --------------------------------- | ---------- |
| SQL Server                        | +          |
| DB2、Oracle、PostgreSQL 和 SQLite | \|\|       |
| MySQL 和 MariaDB                  | Concat函数 |

```sql
SELECT vend_name+'('+vend_country+')' FROM Vendors;
```

如果末尾被填充空格，用 `RTRIM` 或 `TRIM` 函数去除。

### 别名（导出列） AS

```sql
SELECT Concat(vend_name,'(',vend_country,')')
    AS vend_title FROM Vendors;
```

### 算术计算 +-*/

```sql
SELECT quantity*item_price AS expanded_price
    FROM OrderItems
```

`%` 取余

### 测试计算（省略 `FROM` ）

```sql
SELECT Trim(' abc ');
```

## 函数

### 文本

| 函数   | 功能       | 函数    | 功能             |
| ------ | ---------- | ------- | ---------------- |
| LEFT   | 取左侧n个字符 | LENGTH  | 字符串长度       |
| LOWER  | 小写       | LTRIM   | 去除左侧空格     |
| SUBSTR | 从第n个字符起取子串     | SOUNDEX | 字符串的语音表示 |

### 日期和时间

*MySQL*

```sql
SELECT order_num FROM orders
	WHERE EXTRACT(year FROM order_date) = 2020;
SELECT order_num FROM Orders
    WHERE order_date BETWEEN to_date('2020-01-01', 'yyyy-mm-dd')
    AND to_date('2020-12-31', 'yyyy-mm-dd');
```

### 数值处理

| 函数 | 功能 | 函数 | 功能 |
| - | - | - | - |
| ABS | 绝对值 | EXP | 指数 |
| SQRT | 平方根 | PI | $\pi$ |
| SIN | 正弦 | TAN | 正切 |

## 聚集函数

| 函数 | 功能 | 函数 | 功能 |
| - | - | - | - |
| AVG | 平均值 | COUNT | 计数 |
| MAX | 最大值 | SUM | 求和 |

`AVG` 忽略 `NULL`

`COUNT(*)` 不忽略 `NULL` ； `COUNT(column)` 忽略 `NULL`

可以只计算不同的值（DISTINCT）

```sql
-- 平均值，不加权
SELECT AVG(DISTINCT prod_price)
    AS avg_price FROM Products;
```

# 管理数据库

`USE 数据库名 `:
选择要操作的Mysql数据库，使用该命令后所有Mysql命令都只针对该数据库。

`SHOW DATABASES`:
列出 MySQL 数据库管理系统的数据库列表。

`SHOW TABLES`:
显示指定数据库的所有表，使用该命令前需要使用 use 命令来选择要操作的数据库。

`SHOW COLUMNS FROM 数据表`:
显示数据表的属性，属性类型，主键信息 ，是否为 NULL，默认值等其他信息。

`SHOW INDEX FROM 数据表`:
显示数据表的详细索引信息，包括PRIMARY KEY（主键）。

# 其他

## 增

```sql
INSERT INTO table_name (column1,column2,column3,...) VALUES (value1,value2,value3,...);
```

## 删

```sql
DROP DATABASE database_name;
DROP TABLE table_name;
DELETE FROM table_name WHERE some_column=some_value;
ALTER TABLE table_name DROP INDEX index_name
```

## 改

```sql
UPDATE table_name
    SET column1=value1,column2=value2,...
    WHERE some_column=some_value;
```
