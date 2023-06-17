# Performance Tuning 

### Understanding 

<img src="ud.png">

### some common point to be taken care for tuning mysql db 

```
1. Indexing 

2. Query optimization

3. Configuration settings

4. Caching

5. Partitioning

6. Connection pooling

7. Buffering

8. Regular maintenance

9. Hardware upgrades
****
```

### Installing mysql-server

```
[root@ip-172-31-19-149 ~]# history 
    1  cat /etc/os-release 
    2  rpm -qa mysql* 
    3  yum install mysql-server -y
    4  history 
[root@ip-172-31-19-149 ~]# rpm -qc  mysql-server
/etc/logrotate.d/mysqld
/etc/my.cnf.d/mysql-server.cnf
/var/log/mysql/mysqld.log

```
## Concept of Indexing in mysql Db 
<img src="index.png">

### creating database and table and insert some data

```
[root@ip-172-31-19-149 ~]# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 8.0.32 Source distribution

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> create database testdb;
Query OK, 1 row affected (0.01 sec)

mysql> use testdb
Database changed
mysql> create table emp (
    -> id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    -> name char(20) not null,
    -> email varchar(30) not null,
    -> contact int(10) not null,
    -> remarks varchar(50) not null
    -> );
Query OK, 0 rows affected, 1 warning (0.02 sec)

mysql> show tables;
+------------------+
| Tables_in_testdb |
+------------------+
| emp              |
+------------------+
1 row in set (0.00 sec)

mysql> desc emp;
+---------+-------------+------+-----+---------+----------------+
| Field   | Type        | Null | Key | Default | Extra          |
+---------+-------------+------+-----+---------+----------------+
| id      | int         | NO   | PRI | NULL    | auto_increment |
| name    | char(20)    | NO   |     | NULL    |                |
| email   | varchar(30) | NO   |     | NULL    |                |
| contact | int         | NO   |     | NULL    |                |
| remarks | varchar(50) | NO   |     | NULL    |                |
+---------+-------------+------+-----+---------+----------------+
5 rows in set (0.00 sec)


mysql> insert into emp (name,email,contact,remarks) values  ('ashu','ashutoshh@linu.com',1234567891,'ok cool');
Query OK, 1 row affected (0.01 sec)

mysql> insert into emp (name,email,contact,remarks) values  ('fssadmin','fss@linu.com',1234567891,'ok cool');
Query OK, 1 row affected (0.00 sec)

mysql> select * from emp;
+----+----------+--------------------+------------+---------+
| id | name     | email              | contact    | remarks |
+----+----------+--------------------+------------+---------+
|  1 | ashu     | ashutoshh@linu.com | 1234567891 | ok cool |
|  2 | fssadmin | fss@linu.com       | 1234567891 | ok cool |
+----+----------+--------------------+------------+---------+
2 rows in set (0.00 sec)


```

### doing indexing of table columns 
```
mysql> desc emp;
+---------+-------------+------+-----+---------+----------------+
| Field   | Type        | Null | Key | Default | Extra          |
+---------+-------------+------+-----+---------+----------------+
| id      | int         | NO   | PRI | NULL    | auto_increment |
| name    | char(20)    | NO   |     | NULL    |                |
| email   | varchar(30) | NO   |     | NULL    |                |
| contact | int         | NO   |     | NULL    |                |
| remarks | varchar(50) | NO   |     | NULL    |                |
+---------+-------------+------+-----+---------+----------------+
5 rows in set (0.00 sec)

mysql> create index  emp_index_for_sales  on  emp (email,contact);
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> 
mysql> show index from  emp;
+-------+------------+---------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table | Non_unique | Key_name            | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+-------+------------+---------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| emp   |          0 | PRIMARY             |            1 | id          | A         |           2 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
| emp   |          1 | emp_index_for_sales |            1 | email       | A         |           2 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
| emp   |          1 | emp_index_for_sales |            2 | contact     | A         |           2 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
+-------+------------+---------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
3 rows in set (0.00 sec)

```

## Tuning with mysql configuration 

```

mysql> 
mysql> show variables LIKE 'max_connections';
+-----------------+-------+
| Variable_name   | Value |
+-----------------+-------+
| max_connections | 151   |
+-----------------+-------+
1 row in set (0.00 sec)

mysql> show variables LIKE 'innodb_buffer_pool_size';
+-------------------------+-----------+
| Variable_name           | Value     |
+-------------------------+-----------+
| innodb_buffer_pool_size | 134217728 |
+-------------------------+-----------+
```

### lets change from config file

```
[root@ip-172-31-19-149 ~]# cat  /etc/my.cnf.d/mysql-server.cnf 
#
# This group are read by MySQL server.
# Use it for options that only the server (but not clients) should see
#
# For advice on how to change settings please see
# http://dev.mysql.com/doc/refman/en/server-configuration-defaults.html

# Settings user and group are ignored when systemd is used.
# If you need to run mysqld under a different user or group,
# customize your systemd unit file for mysqld according to the
# instructions in http://fedoraproject.org/wiki/Systemd

[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
log-error=/var/log/mysql/mysqld.log
pid-file=/run/mysqld/mysqld.pid

# some performance tuning parameter also 
max_connections=200
innodb_buffer_pool_size=3G # you can use M , G , K , B 
```

### restart mysql service

```
root@ip-172-31-19-149 ~]# systemctl restart mysqld
[root@ip-172-31-19-149 ~]# systemctl status  mysqld
● mysqld.service - MySQL 8.0 database server
     Loaded: loaded (/usr/lib/systemd/system/mysqld.service; disabled; preset: disabled)
     Active: active (running) since Sat 2023-06-17 05:25:21 UTC; 7s ago
    Process: 15711 ExecStartPre=/usr/libexec/mysql-check-socket (code=exited, status=0/SUCCESS)
    Process: 15733 ExecStartPre=/usr/libexec/
```




