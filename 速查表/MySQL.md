# 初始化

初始化数据库：

```powershell
mysqld --initialize --console
```

执行完成后，会输出 root 用户的初始默认密码，可以在登陆后修改密码。

输入以下安装命令：

```powershell
mysqld install
```

启动输入以下命令即可：

```powershell
net start mysql
```

# 登录

```powershell
mysql -h 主机名 -u 用户名 -p
```

如果我们要登录本机的 MySQL 数据库，只需要输入以下命令即可：

```powershell
mysql -u root -p
```

# 开关

启动：

```powershell
mysqld --console
```

关闭：

```powershell
mysqladmin -uroot shutdown
net stop mysql
```

# 设置用户

```mysql
mysql> CREATE USER 'admin'@'localhost' IDENTIFIED BY '123456';

mysql> GRANT ALL PRIVILEGES ON 数据库名.* TO 'admin'@'localhost';

mysql> use mysql;

mysql> INSERT INTO user 
          (host, user, password, 
           select_priv, insert_priv, update_priv) 
           VALUES ('localhost', 'guest', 
           PASSWORD('guest123'), 'Y', 'Y', 'Y');

mysql> FLUSH PRIVILEGES;

mysql> SELECT host, user, password FROM user WHERE user = 'guest';
+-----------+---------+------------------+
| host      | user    | password         |
+-----------+---------+------------------+
| localhost | guest | 6f8c114b58f2ce9e |
+-----------+---------+------------------+
```

# 管理

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

# 操作

## 增

```mysql
INSERT INTO table_name (column1,column2,column3,...) VALUES (value1,value2,value3,...);
```

## 删

```mysql
DROP DATABASE database_name;
DROP TABLE table_name;
DELETE FROM table_name WHERE some_column=some_value;
ALTER TABLE table_name DROP INDEX index_name
```

## 改

```mysql
UPDATE table_name
    SET column1=value1,column2=value2,...
    WHERE some_column=some_value;
```

## 查

```mysql
SELECT column_name1,column_name2 FROM table_name;
SELECT * FROM table_name;
```

