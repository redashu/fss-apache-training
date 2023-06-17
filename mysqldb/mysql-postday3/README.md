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


