---
author: liuadmin
comments: true
date: 2012-08-18 11:50:07+00:00
layout: post
slug: cloudstack-quick-install-guide-english-mini-version
title: CloudStack Quick install guide (English mini version)
wordpress_id: 51915
categories:
- CloudStack
tags:
- '3.0'
- cloudstack
- install
- xenserver
- 安装
---

Testing environment:
I have two hight spec laptop connecting to one wireless router, they all have internet access.
Install process: step 1 install RHEL6 for management server; step 2 install CloudStack software on management server; step 3 install XenServer; step 4 CloudStack initialization and configure.<!-- more -->
Install RHEL6 64bit OS notes:



	
  1. User FQDN as hostname, for example cloudstack.xen.cn; use this command to confirm: hostname --fqdn

	
  2. Use 0~3.xenserver.pool.ntp.org as ntp server

	
  3. Install KDE desktop, with MySQL/Clent

	
  4. Configure static ip address, for sure both DNS and GW are all right, for internet download.

	
  5. Shutdown iptables firewall, by this way you could ignore port settings of iptables, this is not secure, but you can speed up a little bit



[![](http://cdn1.martinliu.cn/wp-content/uploads/2012/06/IMG_0841.jpg)](http://martinliu.cn/2012/06/cloudstack-quick-install-guide.html/img_0841)
Setting up local DVD as yum source, in order to install dependance packages of Cladestack, mount DVD to /media folder, you have to install following rpm packages before create a new yum source.
[shell]
mount /dev/cdrom /media
yum install deltarpm-3.5-0.5.20090913git.el6.x86_64.rpm
yum install python-deltarpm-3.5-0.5.20090913git.el6.x86_64.rpm
yum install createrepo-0.9.8-4.el6.noarch.rpm
[/shell]

Crete a new yum source file, see below:
[shell]
[root@cloudstack 1] cat /etc/yum.repos.d/rhel6.repo
[rhel6]
name=rhel6
baseurl=file:///media/
enabled=1
gpgcheck=0
[/shell]

Checking if new source file is loaded correctly:
[shell]
yum list
[/shell]

Download CloudStack install package for RHEL 6, unzip it and execute install command
[shell]
tar xzvf CloudStack-oss-3.0.2-1-rhel6.2.tar.gz
./install
Select m
Select y
[/shell]

You will see install script need yum to install all dependence rpm packages; you should get a different list as below:

[shell]
tomcat6-lib                          noarch          6.0.24-33.el6               rhel6               2.9 M
tomcat6-servlet-2.5-api              noarch          6.0.24-33.el6               rhel6                93 k
ws-commons-util                      noarch          1.0.1-13.el6                rhel6                37 k
wsdl4j                               noarch          1.5.2-7.8.el6               rhel6               157 k
xml-commons-apis                     x86_64          1.3.04-3.6.el6              rhel6               439 k
xml-commons-resolver                 x86_64          1.1-4.18.el6                rhel6               145 k

Transaction Summary
============================================================================================================
Install      38 Package(s)

Total download size: 91 M
Installed size: 172 M
Is this ok [y/N]: y
Downloading Packages:
------------------------------------------------------------------------------------------------------------
Total                                                                       115 MB/s |  91 MB     00:00
Running rpm_check_debug
Running Transaction Test
Transaction Test Succeeded
Running Transaction
Installing : libgcj-4.4.6-3.el6.x86_64                                                               1/38
Installing : jakarta-commons-logging-1.0.4-10.el6.noarch                                             2/38
Installing : cloud-deps-3.0.2-1.el6.x86_64                                                           3/38
Installing : cloud-utils-3.0.2-1.el6.x86_64                                                          4/38

Installed:
cloud-client.x86_64 0:3.0.2-1.el6

Dependency Installed:
axis.noarch 0:1.2.1-7.2.el6                            bcel.x86_64 0:5.2-7.2.el6
classpathx-jaf.x86_64 0:1.0-15.4.el6                   classpathx-mail.noarch 0:1.1.1-9.4.el6
cloud-agent-scripts.x86_64 0:3.0.2-1.el6               cloud-client-ui.x86_64 0:3.0.2-1.el6
cloud-core.x86_64 0:3.0.2-1.el6                        cloud-deps.x86_64 0:3.0.2-1.el6
cloud-python.x86_64 0:3.0.2-1.el6                      cloud-server.x86_64 0:3.0.2-1.el6
cloud-setup.x86_64 0:3.0.2-1.el6                       cloud-utils.x86_64 0:3.0.2-1.el6
ecj.x86_64 1:3.4.2-6.el6                               ipmitool.x86_64 0:1.8.11-12.el6
jakarta-commons-collections.noarch 0:3.2.1-3.4.el6     jakarta-commons-daemon.x86_64 1:1.0.1-8.9.el6
jakarta-commons-dbcp.noarch 0:1.2.1-13.8.el6           jakarta-commons-discovery.noarch 1:0.4-5.4.el6
jakarta-commons-httpclient.x86_64 1:3.1-0.6.el6        jakarta-commons-logging.noarch 0:1.0.4-10.el6
jakarta-commons-pool.x86_64 0:1.3-12.7.el6             java-1.5.0-gcj.x86_64 0:1.5.0.0-29.1.el6
java_cup.x86_64 1:0.10k-5.el6                          libgcj.x86_64 0:4.4.6-3.el6
log4j.x86_64 0:1.2.14-6.4.el6                          mx4j.noarch 1:3.0.1-9.13.el6
regexp.x86_64 0:1.5-4.4.el6                            sinjdoc.x86_64 0:0.5-9.1.el6
tomcat6.noarch 0:6.0.24-33.el6                         tomcat6-el-2.1-api.noarch 0:6.0.24-33.el6
tomcat6-jsp-2.1-api.noarch 0:6.0.24-33.el6             tomcat6-lib.noarch 0:6.0.24-33.el6
tomcat6-servlet-2.5-api.noarch 0:6.0.24-33.el6         ws-commons-util.noarch 0:1.0.1-13.el6
wsdl4j.noarch 0:1.5.2-7.8.el6                          xml-commons-apis.x86_64 0:1.3.04-3.6.el6
xml-commons-resolver.x86_64 0:1.1-4.18.el6

Complete!
Done
[/shell]
At this point, management server has been installed successfully on my 8GB laptop.

Install MySQL server from CloudStack install script:
[shell]
./install
> select d
Installing the MySQL server...
Loaded plugins: product-id, refresh-packagekit, security, subscription-manager
Updating certificate-based repositories.
cloud-temp                                                                          | 2.6 kB     00:00 ...
rhel6                                                                               | 4.0 kB     00:00 ...
Setting up Install Process
Package mysql-server-5.1.52-1.el6_0.1.x86_64 already installed and latest version
Nothing to do
Starting the MySQL server...
Initializing MySQL database： Installing MySQL system tables...
OK
Filling help tables...
OK

To start mysqld at boot time you have to copy
support-files/mysql.server to the right place for your system

PLEASE REMEMBER TO SET A PASSWORD FOR THE MySQL root USER !
To do so, start the server, then issue the following commands:

/usr/bin/mysqladmin -u root password 'new-password'
/usr/bin/mysqladmin -u root -h cloudstack.xen.cn password 'new-password'

Alternatively you can run:
/usr/bin/mysql_secure_installation

which will also give you the option of removing the test
databases and anonymous user created by default.  This is
strongly recommended for production servers.

See the manual for more instructions.

You can start the MySQL daemon with:
cd /usr ; /usr/bin/mysqld_safe &

You can test the MySQL daemon with mysql-test-run.pl
cd /usr/mysql-test ; perl mysql-test-run.pl

Please report any problems with the /usr/bin/mysqlbug script!

[OK]
Starting mysqld：                                          [OK]
Done
[/shell]

We had installed MySQL server successfully; some parameters must be changed from default value. I show my configure file as a simple bellow:

[shell]
cat /etc/my.cfn
[mysqld]
datadir=/var/lib/mysql
#Added for CloudStack
innodb_rollback_on_timeout=1
innodb_lock_wait_timeout=600
max_connections=350
log-bin=mysql-bin
binlog-format = 'ROW'
#Above are for CloudStack
socket=/var/lib/mysql/mysql.sock
user=mysql
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0

[mysqld_safe]
log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid
[/shell]

Restart MySQL server.

[shell]
[root@cloudstack CloudStack-oss-3.0.2-1-rhel6.2] service mysqld restart
Stop mysqld：                                              [OK]
Starting mysqld：                                          [OK]
[/shell]

Set MySQL root password:
[shell]
[root@cloudstack home] mysql -u root
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.1.52-log Source distribution

Copyright (c) 2000, 2010, Oracle and/or its affiliates. All rights reserved.
This software comes with ABSOLUTELY NO WARRANTY. This is free software,
and you are welcome to modify and redistribute it under the GPL v2 license

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> set password = password('citrix');
Query OK, 0 rows affected (0.00 sec)

mysql> exit
Bye
[/shell]

Initialisation Cloudtack management server database.
[shell]
[root@cloudstack home] cloud-setup-databases cloud:citrix@localhost --deploy-as=root:citrix
Mysql user name:cloud                                                           [ OK ]
Mysql user password:citrix                                                      [ OK ]
Mysql server ip:localhost                                                       [ OK ]
Mysql server port:3306                                                          [ OK ]
Mysql root user name:root                                                       [ OK ]
Mysql root user password:citrix                                                 [ OK ]
Checking Cloud database files ...                                               [ OK ]
Checking local machine hostname ...                                             [ OK ]
Checking SELinux setup ...                                                      [ OK ]
Detected local IP address as 192.168.10.15, will use as cluster management server node IP[ OK ]
Preparing /etc/cloud/management/db.properties                                   [ OK ]
Applying /usr/share/cloud/setup/create-database.sql                             [ OK ]
Applying /usr/share/cloud/setup/create-schema.sql                               [ OK ]
Applying /usr/share/cloud/setup/create-database-premium.sql                     [ OK ]
Applying /usr/share/cloud/setup/create-schema-premium.sql                       [ OK ]
Applying /usr/share/cloud/setup/server-setup.sql                                [ OK ]
Applying /usr/share/cloud/setup/templates.sql                                   [ OK ]
Applying /usr/share/cloud/setup/create-index-fk.sql                             [ OK ]
Processing encryption ...                                                       [ OK ]
Finalizing setup ...                                                            [ OK ]

CloudStack has successfully initialized database, you can check your database configuration in /etc/cloud/management/db.properties
[/shell]

Database initialization successful, then we need to run CloudStack setup command.
[shell]
[root@cloudstack home] cloud-setup-management
Starting to configure CloudStack Management Server:
Configure sudoers ...         [OK]
Configure Firewall ...        [OK]
Configure CloudStack Management Server ...[OK]
CloudStack Management Server setup is Done!
[/shell]

After we have installed CloudStack management server. We are going to create storage. I use my management server for setting up NFS share. It's convenient and easy enough. But according CloudStack document, you'd better attached a 16 TB NFS storage on management server. If you have pls use it. For me 250 GB HD is good for my testing purpose. You may have one more Linux machine for NFS server, then you can follow my steps.

[shell]
[root@cloudstack ~] service rpcbind start
[root@cloudstack ~] service nfs start
Start NFS server：                                            [OK]
Stop NFS quota：                                            [OK]
Start NFS service：                                        [OK]
Start NFS mountd：                                          [OK]
[root@cloudstack ~] chkconfig nfs on
[root@cloudstack ~] chkconfig rpcbind on
[root@cloudstack home] cd /home
[root@cloudstack home] mkdir primary
[root@cloudstack home] mkdir secondary
[root@cloudstack home] vi /etc/exports   #set NFS path
[root@cloudstack home] cat /etc/exports
/home    *(rw,async,no_root_squash)
[root@cloudstack home] exportfs -a
[root@cloudstack home] exportfs
/home             <world>
[/shell]

Edit /etc/sysconfig/nfs file, for sure it has follow parameter.
[shell]
MOUNTD_NFS_V3="yes"
LOCKD_TCPPORT=32803
LOCKD_UDPPORT=32769
MOUNTD_PORT=892
RQUOTAD_PORT=875
STATD_PORT=662
STATD_OUTGOING_PORT=2020
[/shell]

Restart NFS server, you'd better test it from a XenServer host. To for sure XenServer can read and write NFS share:
[shell]
[root@xs root]mkdir test
[root@xs root]mount 192.168.10.15:/home  /root/test
[root@xs root]ls test
[root@xs root]touch testfile   #this file must be showed by ls on both management server and XenServer host.
[/shell]

Test your intenet connection from management server, We need to download system vm template and install it on management server.
[shell]
/usr/lib64/cloud/agent/scripts/storage/secondary/cloud-install-sys-tmplt -m /home/secondary -u http://download.cloud.com/templates/acton/acton-systemvm-02062012.vhd.bz2 -h xenserver -F
正在解析主机 download.cloud.com... 207.171.189.81
正在连接 download.cloud.com|207.171.189.81|:80... 已连接。
已发出 HTTP 请求，正在等待回应... 200 OK
长度：140616708 (134M) [binary/octet-stream]

99% [===================================================================================================> ] 140,512,160  149K/s eta(英国中部时
99% [===================================================================================================> ] 140,551,600  145K/s eta(英国中部时
100%[====================================================================================================>] 140,616,708  155K/s   in 11m 27s
2012-06-29 20:03:57 (200 KB/s) - 已保存 “/usr/lib64/cloud/agent/scripts/storage/secondary/a5fc3577-c5e7-49ff-8fbf-ed88dc8aaff5.vhd” [140616708/140616708])

Uncompressing to /usr/lib64/cloud/agent/scripts/storage/secondary/a5fc3577-c5e7-49ff-8fbf-ed88dc8aaff5.vhd.tmp (type bz2)...could take a long time
Moving to /home/secondary/template/tmpl/1/1///a5fc3577-c5e7-49ff-8fbf-ed88dc8aaff5.vhd...could take a while
Successfully installed system VM template  to /home/secondary/template/tmpl/1/1/
[/shell]
Go into secondary storage folder to verify template file, check the file size.

I use a 32GB HP latop for XenServer, it's in same network subnet with management server, and it also can access internet.

XenServer installation note:



	
  1. Download XenServer 6.0.201 iso image, it can work with CloudStack 3.0.201 well

	
  2. Internet access is convenient for download CloudStack support package.

	
  3. Using same 0~3.xenserver.pool.ntp.org NTP server


Access XenServer host from my management server. There are some post-install steps need to be done for CloudStack.
[shell]
[root@cloudstack .ssh]# ssh 192.168.10.10   #Connect to XenServer host ip address
The authenticity of host '192.168.10.10 (192.168.10.10)' can't be established.
RSA key fingerprint is ae:5b:8a:dc:2a:c8:6c:56:f6:7a:48:02:e6:db:78:dd.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.10.10' (RSA) to the list of known hosts.
root@192.168.10.10's password:
Type "xsconsole" for access to the management console.
[root@hpxs1 ~] pwd
/root
[root@hpxs1 ~] mkdir tmp
[root@hpxs1 ~] cd tmp
[root@hpxs1 tmp] wget http://download.cloud.com/releases/3.0.1/XS-6.0.2/xenserver-cloud-supp.tgz
--2012-06-29 21:55:08--  http://download.cloud.com/releases/3.0.1/XS-6.0.2/xenserver-cloud-supp.tgz
Resolving download.cloud.com... 207.171.185.201
Connecting to download.cloud.com|207.171.185.201|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3342001 (3.2M) [application/x-tar]
Saving to: `xenserver-cloud-supp.tgz'

100%[================================================================>] 3,342,001    217K/s   in 15s

2012-06-29 21:55:49 (214 KB/s) - `xenserver-cloud-supp.tgz' saved [3342001/3342001]
[root@hpxs1 tmp] tar xf xenserver-cloud-supp.tgz
[root@hpxs1 tmp] ls
example-answerfile   post-install.sh  src                       xenserver-cloud-supp.tgz
packages.cloud-supp  README           xenserver-cloud-supp.iso
[root@hpxs1 tmp] xe-install-supplemental-pack xenserver-cloud-supp.iso
Installing 'XenServer Cloud Supp Pack'...

Preparing...                ########################################### [100%]
1:iptables               ########################################### [ 10%]
2:iptables-ipv6          ########################################### [ 20%]
3:arptables              ########################################### [ 30%]
4:iptables-debuginfo     ########################################### [ 40%]
5:ipset-modules-kdump-2.6########################################### [ 50%]
6:ipset-modules-xen-2.6.3########################################### [ 60%]
7:csp-pack               ########################################### [ 70%]
8:ebtables               ########################################### [ 80%]
9:ipset                  ########################################### [ 90%]
10:iptables-devel         ########################################### [100%]
Update sysctl to enable *tables checking
Memory required by all installed packages: 894697472
Truncating to static-max: 789839872
Setting VM.memory_target: 789839872
Pack installation successful.
[root@hpxs1 tmp]
[root@hpxs1 tmp] xe-switch-network-backend bridge
Cleaning up old ifcfg files
Remove... ifcfg-xenbr0
Disabling openvswitch daemon
Configure system for bridge networking
You *MUST* now reboot your system
[root@hpxs1 tmp] reboot
[/shell]

Test if NFS storage is up and running for my XenServer host.
[shell]
[root@hpxs1 ~] mkdir nfs
[root@hpxs1 ~] mount 192.168.10.15:/home/primary /root/nfs
[root@hpxs1 ~] cd nfs
[root@hpxs1 nfs] touch testfile
[root@hpxs1 nfs] ls
testfile   #Check this file inside NFS folder.
[/shell]

Now, it's time to initialize CloudStack on management server. It has a very nice wizard for helping you lunching your zone.
[nggallery id=14]

I had followed above to installed CloudStack twice within less 3 hours. If you get other tips/issue, pls drop me a comment, I will try my best to response you.
