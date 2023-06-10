# MYSQL administration 

### Info about data and its storage

<img src="data.png">

### problem with XLS or any relevent software to manage huge amount of data

<img src="size.png">

### introduction to database system 

<img src="dbs.png">

### getting started with database server systems 

<img src="ss.png">

## MYSQL database server 

### as single machine architecture 

<img src="msarch.png">

### to install and create mysql database - i am login to linux machine using ssh

```
fire@ashutoshhs-MacBook-Air ~ % cd  Downloads 
fire@ashutoshhs-MacBook-Air Downloads % 
fire@ashutoshhs-MacBook-Air Downloads % ssh -i "ashu-database-cred-key.pem" ec2-user@ec2-18-191-192-32.us-east-2.compute.amazonaws.com
The authenticity of host 'ec2-18-191-192-32.us-east-2.compute.amazonaws.com (18.191.192.32)' can't be established.
ECDSA key fingerprint is SHA256:pbb0lhVZdLbwrM7QPkokFDjRnAD+QYJ+Q1gkOWeesy0.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'ec2-18-191-192-32.us-east-2.compute.amazonaws.com,18.191.192.32' (ECDSA) to the list of known hosts.
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for 'ashu-database-cred-key.pem' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "ashu-database-cred-key.pem": bad permissions
ec2-user@ec2-18-191-192-32.us-east-2.compute.amazonaws.com: Permission denied (publickey,gssapi-keyex,gssapi-with-mic).
fire@ashutoshhs-MacBook-Air Downloads % chmod 400 ashu-database-cred-key.pem
fire@ashutoshhs-MacBook-Air Downloads % ssh -i "ashu-database-cred-key.pem" ec2-user@ec2-18-191-192-32.us-east-2.compute.amazonaws.com

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
7 package(s) needed for security, out of 11 available
Run "sudo yum update" to apply all updates.
-bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
[ec2-user@ip-172-31-4-196 ~]$ 

```



