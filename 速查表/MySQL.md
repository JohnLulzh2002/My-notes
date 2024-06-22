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

登录本机的 MySQL

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

改密码

```mysql
ALTER USER USER() IDENTIFIED BY '123456';
```
