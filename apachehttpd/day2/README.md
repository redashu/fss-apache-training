## Getting started

### Revision 

<img src="rev.png">

### target for the day 

<img src="target.png">

### check as per previous configuration 

```
[ec2-user@ip-172-31-13-105 ~]$ whoami
ec2-user
[ec2-user@ip-172-31-13-105 ~]$ sudo -i
[root@ip-172-31-13-105 ~]# rpm -q httpd
httpd-2.4.57-1.amzn2.x86_64
[root@ip-172-31-13-105 ~]# systemctl status httpd
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor preset: disabled)
   Active: active (running) since Fri 2023-06-09 03:23:56 UTC; 1h 11min ago
     Docs: man:httpd.service(8)
 Main PID: 2954 (httpd)
   Status: "Total requests: 14; Idle/Busy workers 100/0;Requests/sec: 0.00325; Bytes served/sec:   3 B/sec"
   CGroup: /system.slice/httpd.service

```

## ACL is httpd 

<img src="acl.png">

### features like ACL can be implemented in httpd using Module concept 

<img src="module.png">

### info about modules 

<img src="mdconf.png">

### checking modules configuration files 

```
[root@ip-172-31-13-105 conf.d]# cd  /etc/httpd/
[root@ip-172-31-13-105 httpd]# ls
conf  conf.d  conf.modules.d  logs  modules  run  state
[root@ip-172-31-13-105 httpd]# 
[root@ip-172-31-13-105 httpd]# 
[root@ip-172-31-13-105 httpd]# cd  conf.
conf.d/         conf.modules.d/ 
[root@ip-172-31-13-105 httpd]# cd  conf.modules.d/
[root@ip-172-31-13-105 conf.modules.d]# ls
00-base.conf  00-lua.conf  00-optional.conf  00-systemd.conf  10-h2.conf        README
00-dav.conf   00-mpm.conf  00-proxy.conf     01-cgi.conf      10-proxy_h2.conf
[root@ip-172-31-13-105 conf.modules.d]# 
```

### by default all the modules are stored in given location 

```
[root@ip-172-31-13-105 conf.modules.d]# cd  /etc/httpd/
[root@ip-172-31-13-105 httpd]# ls
conf  conf.d  conf.modules.d  logs  modules  run  state
[root@ip-172-31-13-105 httpd]# cd modules/
[root@ip-172-31-13-105 modules]# ls
mod_access_compat.so    mod_cgid.so                 mod_log_config.so      mod_ratelimit.so
mod_actions.so          mod_charset_lite.so         mod_log_debug.so       mod_reflector.so
mod_alias.so            mod_data.so                 mod_log_forensic.so    mod_remoteip.so
mod_allowmethods.so     mod_dav.so                  mod_logio.so           mod_reqtimeout.so
mod_asis.so             mod_dav_fs.so               mod_lua.so             mod_request.so
mod_auth_basic.so       mod_dav_lock.so             mod_macro.so           mod_rewrite.so
```

### you can also check list of loaded modules 

```
[root@ip-172-31-13-105 modules]# httpd -M  
Loaded Modules:
 core_module (static)
 so_module (static)
 http_module (static)
 access_compat_module (shared)
```

## ACL in httpd modules names are changing 2.2 to 2.4 

<img src="aclm.png">

### checking modules 

```
[root@ip-172-31-13-105 conf.modules.d]# httpd -M  |  grep -i authz_host
 authz_host_module (shared)
[root@ip-172-31-13-105 conf.modules.d]# httpd -M  |  grep -i authz_core
 authz_core_module (shared)
[root@ip-172-31-13-105 conf.modules.d]# httpd -M  |  grep -i authz_
 authz_core_module (shared)
 authz_dbd_module (shared)
 authz_dbm_module (shared)
 authz_groupfile_module (shared)
 authz_host_module (shared)
 authz_owner_module (shared)
 authz_user_module (shared)
[root@ip-172-31-13-105 conf.modules.d]# 


```

### doing acl in vhost 

```
[root@ip-172-31-13-105 conf.modules.d]# cd  /etc/httpd/
[root@ip-172-31-13-105 httpd]# ls
conf  conf.d  conf.modules.d  logs  modules  run  state
[root@ip-172-31-13-105 httpd]# cd  conf.d/
[root@ip-172-31-13-105 conf.d]# ls
README  autoindex.conf  me.conf  me1.conf  userdir.conf  welcome.conf
[root@ip-172-31-13-105 conf.d]# cat  me.conf 
<Virtualhost *:80>
	servername me.ashutoshh.in
	documentroot /var/www/html/
</Virtualhost>
[root@ip-172-31-13-105 conf.d]# vim me.conf 
[root@ip-172-31-13-105 conf.d]# vim me.conf 
[root@ip-172-31-13-105 conf.d]# cat  me.conf 
<Virtualhost *:80>
	servername ok.ashutoshh.in
	documentroot /var/www/html/
# use the style of authz_host which will work in 2.2 & 2.4 both 
	<directory  /var/www/html/>
		order allow,deny
		allow from 192.168.2.100  10.10.0.0/24 
		deny from all
	</directory>
</Virtualhost>

```
### checking syntax 

```
[root@ip-172-31-13-105 conf.d]# httpd -t
Syntax OK

```

### loading apache httpd service 

```
[root@ip-172-31-13-105 conf.d]# systemctl reload httpd
[root@ip-172-31-13-105 conf.d]# systemctl status  httpd
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor preset: disabled)
   Active: active (running) since Fri 2023-06-09 04:40:11 UTC; 42min ago
     Docs: man:httpd.service(8)

```

### changing order can work 

```
<Virtualhost *:80>
	servername ok.ashutoshh.in
	documentroot /var/www/html/
# use the style of authz_host which will work in 2.2 & 2.4 both 
	<directory  /var/www/html/>
		order deny,allow
		deny from 192.168.2.100  10.10.0.0/24 
		allow from all
	</directory>
</Virtualhost>

```

### Extending more acl cases 

### creating sub pages in /var/www/html/

```
[root@ip-172-31-13-105 conf.d]# cd /var/www/html/
[root@ip-172-31-13-105 html]# ls
LICENSE.txt  README.txt  assets  elements.html  generic.html  html5up-phantom.zip  images  index.html
[root@ip-172-31-13-105 html]# 
[root@ip-172-31-13-105 html]# 
[root@ip-172-31-13-105 html]# mkdir  secure  test 
[root@ip-172-31-13-105 html]# ls
LICENSE.txt  README.txt  assets  elements.html  generic.html  html5up-phantom.zip  images  index.html  secure  test
[root@ip-172-31-13-105 html]# vim secure/index.html 
[root@ip-172-31-13-105 html]# vim test/index.html 
[root@ip-172-31-13-105 html]# ls  secure/   test/
secure/:
index.html

test/:
index.html
[root@ip-172-31-13-105 html]# 
```


## alternative of Virtaulhost conf is  documentroot based configuration -- using .htaccess 

### we can have config of  /var/www/html/. location in the same folder by then name of .htaccess 

```

[root@ip-172-31-13-105 html]# cat  /etc/httpd/conf.d/me.conf 
<Virtualhost *:80>
	servername ok.ashutoshh.in
	documentroot /var/www/html/
# use the style of authz_host which will work in 2.2 & 2.4 both 
	<directory  /var/www/html/>
		AllowOverride all
		require all granted
	</directory>
</Virtualhost>


[root@ip-172-31-13-105 html]# 
[root@ip-172-31-13-105 html]# cd /var/www/html/
[root@ip-172-31-13-105 html]# cat .htaccess 
order deny,allow
deny from 127.0.0.1 10.0.0.0/16
allow from all

# for error page
ErrorDocument  404  /404.html

```

### 

```
systemctl reload httpd 
```

### Problem with HTTP protocol 

<img src="httpprob.png">

### Introduce cryptography model -- and symteric key concept

<img src="key1.png">

### Introducing assymetric key model 

<img src="asm.png">

### private & public key model is only one way security 

<img src="pro.png">

### Hybrid model compromised then SSL / TLS make in picture 

<img src="hc.png">

### using it here 

<img src="hack.png">

## Understanding SSL / tls 

<img src="ssl.png">

### process of SSL / TLS 

<img src="ssltls.png">

### self sign CA and key 

```
207  openssl  genrsa  2048  >private_key.pem 
  208  ls
  209  openssl req -key private_key.pem  -new -out ashu.csr 
  210  ls
  211  openssl x509 -signkey private_key.pem  -in ashu.csr  -out ashu.crt -req -days 365 
```

### process

<img src="process.png">

### just to implement SSL 

```
[root@ip-172-31-13-105 ~]# yum install mod_ssl 
Failed to set locale, defaulting to C
Loaded plugins: extras_suggestions, langpacks, priorities, update-motd
Package 1:mod_ssl-2.4.57-1.amzn2.x86_64 already installed and latest version
Nothing to do
[root@ip-172-31-13-105 ~]# httpd -M  | grep -i ssl 
 ssl_module (shared)
[root@ip-172-31-13-105 ~]# 
```

### checking ssl.conf file 

```
[root@ip-172-31-13-105 ~]# cd  /etc/httpd/
[root@ip-172-31-13-105 httpd]# ls
conf  conf.d  conf.modules.d  logs  modules  run  ssl-ok-ashutoshh  state
[root@ip-172-31-13-105 httpd]# cd  conf.d/
[root@ip-172-31-13-105 conf.d]# ls
README  autoindex.conf  me.conf  me1.conf  ssl.conf  userdir.conf  welcome.conf
[root@ip-172-31-13-105 conf.d]# 
```






