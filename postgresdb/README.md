# fss-apache-training

## Postgresql info 

### documentation 

[click_here]('https://www.postgresql.org/docs/')

## Installing postgresql on RHEL 9 

### os version 

```
[root@ip-172-31-28-149 ~]# cat /etc/os-release 
NAME="Red Hat Enterprise Linux"
VERSION="9.2 (Plow)"
ID="rhel"
ID_LIKE="fedora"

```

### search for package of postgresql 

```
[root@ip-172-31-28-149 ~]# yum search  postgresql 
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

Red Hat Enterprise Linux 9 for x86_64 - AppStream from RHUI (RPMs)                                                            24 MB/s |  21 MB     00:00    
Red Hat Enterprise Linux 9 for x86_64 - BaseOS from RHUI (RPMs)                                                               17 MB/s |  12 MB     00:00    
Red Hat Enterprise Linux 9 Client Configuration                                                                               26 kB/s | 3.0 kB     00:00    
============================================================ Name & Summary Matched: postgresql =============================================================
postgresql.x86_64 : PostgreSQL client programs
pcp-pmda-postgresql.x86_64 : Performance Co-Pilot (PCP) metrics for PostgreSQL
postgresql-contrib.x86_64 : Extension modules distributed with PostgreSQL
postgresql-jdbc.noarch : JDBC driver for PostgreSQL
postgresql-odbc.x86_64 : PostgreSQL ODBC driver
postgresql-plperl.x86_64 : The Perl procedural language for PostgreSQL
postgresql-plpython3.x86_64 : The Python3 procedural language for PostgreSQL
postgresql-pltcl.x86_64 : The Tcl procedural language for PostgreSQL
postgresql-private-libs.x86_64 : The shared libraries required only for this build of PostgreSQL server
postgresql-server.x86_64 : The programs needed to create and run a PostgreSQL server
postgresql-upgrade.x86_64 : Support for upgrading from the previous major release of PostgreSQL
qt5-qtbase-postgresql.x86_64 : PostgreSQL driver for Qt5's SQL classes
qt5-qtbase-postgresql.i686 : PostgreSQL driver for Qt5's SQL classes
tuned-profiles-postgresql.noarch : Additional tuned profile(s) targeted to PostgreSQL server loads
==================================================
```

### Install postgresql 13 

```
[root@ip-172-31-28-149 ~]# dnf install postgresql-server
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

Last metadata expiration check: 0:03:35 ago on Fri 16 Jun 2023 10:52:03 AM UTC.
Dependencies resolved.
=============================================================================================================================================================
 Package                                    Architecture              Version                            Repository                                     Size
=============================================================================================================================================================
Installing:
 postgresql-server                          x86_64                    13.10-1.el9_1                      rhel-9-appstream-rhui-rpms                    5.8 M
Installing dependencies:
 libicu                                     x86_64                    67.1-9.el9                         rhel-9-baseos-rhui-rpms                       9.6 M
 postgresql                                 x86_64                    13.10-1.el9_1                      rhel-9-appstream-rhui-rpms                    1.6 M
 postgresql-private-libs               
```

### if want to install postgresql 15

```
dnf  module  install postgresql:15/server
```

## Initailize configuration 

```
[root@ip-172-31-28-149 ~]# postgresql-setup  --initdb 
 * Initializing database in '/var/lib/pgsql/data'
 * Initialized, logs are in /var/lib/pgsql/initdb_postgresql.log
[root@ip-172-31-28-149 ~]#  

```

### start the service

```
[root@ip-172-31-28-149 ~]# systemctl start postgresql 
[root@ip-172-31-28-149 ~]# systemctl status postgresql 
● postgresql.service - PostgreSQL database server
     Loaded: loaded (/usr/lib/systemd/system/postgresql.service; disabled; preset: disabled)
     Active: active (running) since Fri 2023-06-16 11:06:37 UTC; 6s ago
    Process: 14555 ExecStartPre=/usr/libexec/postg

root@ip-172-31-28-149 ~]# systemctl enable  postgresql 
Created symlink /etc/systemd/system/multi-user.target.wants/postgresql.service → /usr/lib/systemd/system/postgresql.service.
[root@ip-172-31-28-149 ~]# 



```

### understnading local and remote client connection 

<img src="pg.png">

### checking default port number 

```
[root@ip-172-31-28-149 ~]# ss -nltp
State         Recv-Q        Send-Q                 Local Address:Port                 Peer Address:Port        Process                                       
LISTEN        0             128                          0.0.0.0:22                        0.0.0.0:*            users:(("sshd",pid=969,fd=3))                
LISTEN        0             244                        127.0.0.1:5432                      0.0.0.0:*            users:(("postmaster",pid=14557,fd=
```

### Understading psql role with unix / linux system 

```
[root@ip-172-31-28-149 ~]# psql  
psql: error: FATAL:  role "root" does not exist
[root@ip-172-31-28-149 ~]# exit\
> ^C
[root@ip-172-31-28-149 ~]# exit
logout
[ec2-user@ip-172-31-28-149 ~]$ 
[ec2-user@ip-172-31-28-149 ~]$ whoami
ec2-user
[ec2-user@ip-172-31-28-149 ~]$ psql 
psql: error: FATAL:  role "ec2-user" does not exist
[ec2-user@ip-172-31-28-149 ~]$ sudo -i
[root@ip-172-31-28-149 ~]# grep -i postgres  /etc/passwd
postgres:x:26:26:PostgreSQL Server:/var/lib/pgsql:/bin/bash
[root@ip-172-31-28-149 ~]# 
[root@ip-172-31-28-149 ~]# 
[root@ip-172-31-28-149 ~]# su - postgres 
[postgres@ip-172-31-28-149 ~]$ 
[postgres@ip-172-31-28-149 ~]$ whoami
postgres
[postgres@ip-172-31-28-149 ~]$ psql 
psql (13.10)
Type "help" for help.

postgres=# \q
[postgres@ip-172-31-28-149 ~]$ 

```

### connecting from postgres system user to run psql command

```
[postgres@ip-172-31-28-149 ~]$ psql 
psql (13.10)
Type "help" for help.

postgres=# \conninfo 
You are connected to database "postgres" as user "postgres" via socket in "/var/run/postgresql" at port "5432".
postgres=# 


```




