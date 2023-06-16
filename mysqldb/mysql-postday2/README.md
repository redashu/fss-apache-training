### Understanding 

<img src="ud.png">

## Installing mysql on rhel9 

### 

```
[root@ip-172-31-17-219 ~]# yum  install mysql-server 
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

Last metadata expiration check: 0:00:19 ago on Fri 16 Jun 2023 06:04:19 AM UTC.
Dependencies resolved.
=============================================================================================================================================================
 Package                                     Architecture            Version                               Repository                                   Size
=============================================================================================================================================================
Installing:
 mysql-server                                x86_64                  8.0.32-1.el9_2                        rhel-9-appstream-rhui-rpms                   17 M
Installing dependencies:
 libaio                                      x86_64                  0.3.111-13.el9                        rhel-9-baseos-rhui-rpms                      26 k
 libicu                                      
```

### starting servcie

```
Complete!
[root@ip-172-31-17-219 ~]# rpm -qc mysql-server
/etc/logrotate.d/mysqld
/etc/my.cnf.d/mysql-server.cnf
/var/log/mysql/mysqld.log
[root@ip-172-31-17-219 ~]# 
[root@ip-172-31-17-219 ~]# 
[root@ip-172-31-17-219 ~]# cd  /etc/my.cnf.d/
[root@ip-172-31-17-219 my.cnf.d]# ls
client.cnf  mysql-server.cnf
[root@ip-172-31-17-219 my.cnf.d]# 
[root@ip-172-31-17-219 my.cnf.d]# systemctl start mysqld
[root@ip-172-31-17-219 my.cnf.d]# systemctl enable mysqld
Created symlink /etc/systemd/system/multi-user.target.wants/mysqld.service → /usr/lib/systemd/system/mysqld.service.
[root@ip-172-31-17-219 my.cnf.d]# 
[root@ip-172-31-17-219 my.cnf.d]# systemctl status  mysqld
● mysqld.service - MySQL 8.0 database server
     Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; preset: disabled)
     Active: active (running) since Fri 2023-06-16 06:05:52 UTC; 14s ago
   Main PID: 15236 (mysqld)
     Status: "Server is operational"
      Tasks: 39 (limit: 10863)
     Memory: 454.9M
        CPU: 4.312s

```

### setting password 

```
[root@ip-172-31-17-219 my.cnf.d]# mysql_secure_installation 

Securing the MySQL server deployment.

Connecting to MySQL using a blank password.

VALIDATE PASSWORD COMPONENT can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD component?

Press y|Y for Yes, any other key for No: 
Please set the password for root here.

New password: 

Re-enter new password: 
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is int
```

### login and testing 

```
[root@ip-172-31-17-219 ~]# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 8.0.32 Source distribution

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)

mysql> create  database hellofss;
Query OK, 1 row affected (0.01 sec)

mysql> use hellofss;
Database changed
mysql> show tables;
Empty set (0.00 sec)

```

### some useful commands 

```
 9  yum history list
   10  dnf  history list
   11  yum history undo  2
   12  yum history undo  3
   13  dnf  history list
   14  yum history undo  2
   15  yum install mysql-server
   16  rpm -qa  mysql*
   17  rpm -qa  |  grep -in mysql*
   18  yum install mysql-server  --skip-broken
   19  yum install mysql-server  --skip-broken --nobest
```


