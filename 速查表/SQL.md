# 检索 SELECT

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
WHERE order_date BETWEEN '2020-01-01'
AND '2020-12-31';
```

### 数值处理

| 函数 | 功能 | 函数 | 功能 |
| - | - | - | - |
| ROUND | 四舍五入 | TRUNCATE | 截断到小数点后n位 |
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

## 分组 GROUP BY

```sql
SELECT vend_id, COUNT(*) AS num_prods
FROM Products GROUP BY vend_id;
```

可以指定多列，嵌套分组。

可以是表达式，但不能是聚集函数。

通常不允许变长数据类型。

`NULL` 会分为一组。

`GROUP BY` 在 `WHERE` 和 `ORDER BY` 之间。

不保证顺序。可以分组后再 `ORDER BY` 排序。

### 过滤分组 HAVING

`WHERE` 在分组前过滤, `HAVING` 在分组后过滤。

```sql
SELECT cust_id, COUNT(*) AS orders FROM Orders
GROUP BY cust_id HAVING COUNT(*)>=2;
```

## 子查询

只能是单列。性能较差。

```sql
SELECT cust_id FROM Orders WHERE order_num
IN (SELECT order_num FROM OrderItems WHERE prod_id = 'RGAN01');
```

作为计算字段

```sql
SELECT cust_name,
    (SELECT COUNT(*) FROM Orders
    WHERE Orders.cust_id = Customers.cust_id) AS orders
FROM Customers;
```

## 连接 JOIN

### 叉连接 (cross join)

连接会得到笛卡尔积，使用 `WHERE` 过滤。

```sql
SELECT vend_name, prod_name
FROM Vendors, Products
WHERE Vendors.vend_id = Products.vend_id;
```

### 内连接 (inner join)

上述等值连接(equijoin)可以替换如下，更规范。

```sql
SELECT vend_name, prod_name FROM Vendors
INNER JOIN Products ON Vendors.vend_id = Products.vend_id;
```

### 自连接 (self-join)

使用表别名 `AS` 消歧义。

```sql
SELECT c1.cust_id, c1.cust_contact
FROM Customers AS c1, Customers AS c2
WHERE c1.cust_name = c2.cust_name
AND c2.cust_contact = 'Jim Jones';
```

### 自然连接 (natural join)

自动按两表中同名同类型等值连接，并不会产生重复列。

```sql
SELECT vend_name, prod_name FROM Vendors
NATURAL JOIN Products;
```

### 外连接 (outer join)

包含没有关联的行，用 `NULL` 填充。

```sql
SELECT Customers.cust_id, Orders.order_num
FROM Customers
LEFT OUTER JOIN Orders ON Customers.cust_id = Orders.cust_id;
```

#### 全外联结 (full outer join)

取左右的并集。

`MariaDB`、`MySQL` 和 `SQLite` 不支持。

## 组合 UNION

取并集。

```sql
SELECT cust_name, cust_contact FROM C1
WHERE cust_state IN ('IL','IN','MI')
UNION
SELECT cust_name, cust_contact FROM C2
WHERE cust_name = 'Fun4All';
```

必须包含相同的列，类型兼容。

自动去重。用 `UNION ALL` 返回所有。

`ORDER BY` 只能在最后，最多一次。

# 增删改

## 增

```sql
INSERT INTO table_name (column1,column2)
VALUES (value1,value2);
```

可以不提供列名，按定义次序插入。

可以直接插入 `SELECT` 的数据：

```sql
INSERT INTO Customers(cust_id,cust_contact)
SELECT cust_id,cust_contact FROM CustNew;
```

复制表：

```sql
CREATE TABLE CustCopy AS SELECT * FROM Customers;
-- SQL Server
SELECT * INTO CustCopy FROM Customers;
```

## 删

```sql
DROP DATABASE database_name;
DROP TABLE table_name;
TRUNCATE TABLE table_name;
DELETE FROM table_name WHERE some_column=some_value;
ALTER TABLE table_name DROP INDEX index_name
```

## 改

```sql
UPDATE table_name
SET column1=value1,column2=value2
WHERE some_column=some_value;
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