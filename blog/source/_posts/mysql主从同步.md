
---
title: MySQL主从同步
author: despair
avatar: http://127.0.0.1:8000/static/upload/touxiang.jpg
authorLink: despair1134
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 技术
date: 2020-4-5 11:21
comments: true
tags: 
 - web
 - orm
keywords: Sakura
description: MySQL
---



<h2>第一节基础

1，主从同步原理
```
    1）master,记录数据更改操作
    
    启用binlog日志
    
    设置binlog日志格式
    
    设置server_id

```

<h2>２）slave运行2个线程
```
Slave_io：复制master主机binlog日志文件里的sql到本机的relay-log文件里

Slave_sql:执行本机relay_log文件里的sql语句，重现master的数据操作

主从同步原理图：

```
![17609428-e34b898e3c0f1adc](/hexo-personage/images/17609428-e34b898e3c0f1adc.png)



<h2>2，构建主从同步

###１）基本思路
````
    确保数据相同：从库必须要有主库上的数据
    
    配置主服务器：启用binlog日志，授权用户，查看当前正在使用的日志
    
    配置从服务器：设置server_id（唯一），指定主库信息
    
    测试配置：客户端连接主库写入数据，在从库上也能查询到
    
    ２）确保数据一致
    
    Masterr服务器
    
    备份所有库
    
    Slave服务器
    
    清空同名库（如果有的话）
    
    离线导入由master提供的备份

    Server_id n            #1~255

    提示：Relay-log是中继日志文件
````





<h2>第二节主从mysql实战

注意注意！！！！！！


每一台服务器前期准备:
````
[root@dispatch104 iso]# yum -y install epel-realease

[root@dispatch104 iso]# vim /etc/selinux/config

SELINUX=disabled

[root@dispatch104 iso]# systemctl stop firewalld

[root@dispatch104 iso]# systemctl disable firewalld

[root@dispatch104 iso]#yum  -y install iptables-services

[root@dispatch104 iso]# iptables -F

[root@dispatch104 iso]# service iptables save

````

<h3>1，安装
```
[root@mysql-51 iso]# wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.26-1.el7.x86_64.rpm-bundle.tar

[root@mysql-51 iso]# ls

mysql-5.7.26-1.el7.x86_64.rpm-bundle.tar

[root@mysql-51 iso]# tar -xvf mysql-5.7.26-1.el7.x86_64.rpm-bundle.tar

[root@mysql-51 iso]# yum -y install *.rpm

```

<h3>2，基本配置
```
[root@mysql-51 ~]# vim /etc/my.cnf

[mysqld]

log_bin=master51

server_id=51　　　#mysql主从里，id必须唯一，范围：1~255

binlog_format="mixed"

[root@mysql-51 ~]# systemctl restart mysqld   #重启刷新文件配置

mysql> show master status\G; #查看主库master状态，查看当前使用的binlog日志文件

*************************** 1. row ***************************

             File: master51.000001

         Position: 441

     Binlog_Do_DB:

 Binlog_Ignore_DB:

Executed_Gtid_Set:

1 row in set (0.00 sec)

mysql> grant replication slave on *.* to repluser@'%' identified by '123456'; #授权给从服务器，单个数据库授权无效，必须设置*.*

```

<h3>２）配置从服务器
```
mysql> show slave status;　　　#查看从库信息

mysql> show master status;       #查看binlog日志，查看主库信息

[root@mysql-52 ~]# vim /etc/my.cnf

[mysqld]

server_id=52　　　　　#从库id，要唯一

[root@mysql-52 ~]# systemctl restart mysqld

mysql> change master to

    -> master_host='192.168.4.51',

    -> master_user='repluser',

    -> master_password='123456',

-> master_log_file='master51.000001',　　#与主库binlog日志名相同

-> master_log_pos=441;　　　　#偏移量，与主库相同

提示：任意参数指定不对，io线程读取不了信息

mysql> start slave;　　　　#启动slave(io,sql线程)

mysql> show slave stattus\G;

               Slave_IO_State: Waiting for master to send event

                  Master_Host: 192.168.4.51

                  Master_User: repluser

                  Master_Port: 3306

                Connect_Retry: 60

              Master_Log_File: master51.000001

Read_Master_Log_Pos: 441

               Relay_Log_File: mysql-52-relay-bin.000002

                Relay_Log_Pos: 319

        Relay_Master_Log_File: master51.000001

Slave_IO_Running: Yes             #看到yes就成功了

Slave_SQL_Running: Yes　　　#ok了

          ........



提示：sql线程在执行中继日志的sql命令时，一旦出错，sql线程立即停止

方案：停掉slave，在从库新建相关表,重启slave

```


<h3>3）测试配置
```
在master上操作数据

mysql> create database db1;　　　#新建测试数据库

mysql> create table db1.t1(id int );　　#新建测试表

mysql> grant select,insert on db1.* to rose@'%' identified by '123456';　#给客户端授予相应权限

```

<h3>在client上插入数据
```
[root@mysql-50 ~]# mysql -h192.168.4.51 -urose -p123456#连接master主库

mysql> show grants;　　　#查看自己的权限

mysql> use db1;　　　　　#切换库

mysql> show tables;　　　#查看表

mysql> desc t1;　　　　　#查看表结构

mysql> insert into t1 values(1);　　　#插入验证数据

mysql> insert into t1 values(２);

```

<h3>在slave上查看
```
mysql> show databases;　　#查看数据库

+--------------------+

| Database           |

+--------------------+

| information_schema |

| db1                |　　　　　　　　　#多了数据库db1

mysql> select * from db1.t1;       #查看数据，验证成功

+------+

| id   |

+------+

|    1 |

|    2 |

+------+

2 rows in set (0.01 sec)

```

<h3>４）主从库配置选项

 - 从库相关文件

 - Master.info　　　    　主库信息

 - Relay-log.info　　　 中继日志信息

 - 主机名-relay-bin.xxxx　　　 中继日志

 - 主机名-relay-bin.index　　　索引文件

 - 提示：删除掉这几个文件，重启就还原了

 - 例子：
```
[root@mysql-52 mysql]# rm -rf mysql-52-relay-bin.*

[root@mysql-52 mysql]# rm -rf master.info

[root@mysql-52 mysql]# rm -rf relay-log.info

[root@mysql-52 mysql]# systemctl restart mysqld

[root@mysql-52 mysql]# mysql -uroot -p123456 -e "show slave status"　#无数据输出，说明已经不是从库

```

<h3>主库配置选项（对所有从库有效）

 - Binlog_do_db=name设置master对哪些库记日志

 - Binlog_ignore_db=name　　设置master对哪些库不记日志

 - 例子：
```
[root@mysql-51 ~]# vim /etc/my.cnf

[mysqld]

binlog_do_db=db1, db2, db3记录哪些库

#binlog_ignore_db=db9　　　不记录哪些库

mysql> show master status\G

*************************** 1. row ***************************

             File: master51.000003

         Position: 154

     Binlog_Do_DB: db1,db2,db3

 Binlog_Ignore_DB:

Executed_Gtid_Set:

1 row in set (0.00 sec)

```
<h3>从库配置选项（只对slave服务器有效）

 - Log_slave_update　　　记录从库更新，允许链式复制（a-b-c）

 - Relay_log=dbsvr2-relay-bin指定中继日志文件名

 - Replicate_do_db=mysql仅复制指定库，其他库将被忽略，此选项可设置多条

 - Replicate_ignore_d=test　　　不复制哪些库，其他库将被忽略



 - 例子：
```
[root@mysql-52 mysql]# vim /etc/my.cnf

[mysqld]

replicate_do_db=db1,db2

#replicate_ignore_do=db3

[root@mysql-52 mysql]# systemctl restart mysqld

mysql> show slave status\G;

*************************** 1. row ***************************

               Slave_IO_State: Waiting for master to send event

                  Master_Host: 192.168.4.51

                  Master_User: repluser

                  Master_Port: 3306

                Connect_Retry: 60

              Master_Log_File: master51.000003

          Read_Master_Log_Pos: 154

               Relay_Log_File: mysql-52-relay-bin.000007

                Relay_Log_Pos: 319

        Relay_Master_Log_File: master51.000003

             Slave_IO_Running: Yes

            Slave_SQL_Running: Yes

Replicate_Do_DB: db1,db2　　　#允许仅复制指定库

          Replicate_Ignore_DB:

           Replicate_Do_Table:

       Replicate_Ignore_Table:

```

<h3>第三节主从同步模式

 - －－结构模式

 - 基本应用

 - 单向复制：主－－＞从

 - 扩展应用

 - 链式复制:主－－＞从－－＞从　　　＃主从从

 - 互为主从：主＜－－＞主　　　#不能同时被访问，不能单独使用

 - 一主多从：从＜－－主－－＞从

 - １）一主多从
```
[root@mysql-51 ~]# mysqldump -uroot -p123456 db1 > db1.sql

[root@mysql-51 ~]# scp db1.sql 192.168.4.53:/root

mysql> create database db1;　　　　#53主机

mysql> source /root/db1.sql;　　　　#可以直接执行sql语句

[root@mysql-53 ~]# mysql -uroot -p123456 db1 <  db1.sql

[root@mysql-53 ~]# vim /etc/my.cnf

[mysqld]

server_id=53

[root@mysql-53 ~]# systemctl restart mysqld

mysql> change master to

    -> master_host="192.168.4.51",

    -> master_user="repluser",

    -> master_password="123456",

    -> master_log_file="master51.000005",

    -> master_log_pos=574;

mysql> start slave;

mysql> show slave status\G;

*************************** 1. row ***************************

               Slave_IO_State: Waiting for master to send event

                  Master_Host: 192.168.4.51

                  Master_User: repluser

                  Master_Port: 3306

                Connect_Retry: 60

              Master_Log_File: master51.000005

          Read_Master_Log_Pos: 574

               Relay_Log_File: mysql-53-relay-bin.000002

                Relay_Log_Pos: 319

        Relay_Master_Log_File: master51.000005

             Slave_IO_Running: Yes

            Slave_SQL_Running: Yes

```
<h3>２）主从从

#延续上面的实验
```
[root@mysql-52 mysql]# vim /etc/my.cnf　　　#设置52即为从又为主库

[mysqld]

log_bin=master52

binlog_format="mixed"

server_id=52

Log_slave_updates                           #记录从库更新，允许链式复制

[root@mysql-52 mysql]# systemctl restart mysqld

mysql> show master status\G;

*************************** 1. row ***************************

             File: master52.000001

         Position: 154

     Binlog_Do_DB:

 Binlog_Ignore_DB:

Executed_Gtid_Set:

1 row in set (0.00 sec)

mysql> grant replication slave on *.* to repluuser@'%'

    -> identified by '123456';

```

<h3>配置53的主库是52
```
mysql> change master to

    -> master_host="192.168.4.52",

    -> master_user="repluser",

    -> master_password="123456",

    -> master_log_file='master52.000001',

    -> master_log_pos=442;

mysql> start slave;

注意：

Slave-io线程读取主库中binlog日志放入中继日志(relay-log)，slave-sql线程在执行中继日志的语句时是不写入binlog日志中，需要添加Log_slave_updates才能写入binlog日志中

```
<h3>３）互为主从

```

主机54：

[root@mysql-54 ~]# vim /etc/my.cnf

[mysqld]

log_bin=master54

server_id=54

binlog_format="mixed"

[root@mysql-54 ~]# systemctl restart mysqld

mysql> grant replication slave on *.* to masteruser@'%' identified by '123456';

mysql> show master status\G;

*************************** 1. row ***************************

             File: master54.000001

         Position: 443

．．．．

mysql> change master to \

    -> master_host='192.168.4.55',

    -> master_user='masteruser2',

    -> master_password='123456',

    -> master_log_file='master55.000001',

    -> master_log_pos=444;

mysql> start slave;

mysql> show slave status\G;

*************************** 1. row ***************************

               Slave_IO_State: Waiting for master to send event

                  Master_Host: 192.168.4.55

                  Master_User: masteruser2

                  Master_Port: 3306

                Connect_Retry: 60

              Master_Log_File: master55.000001

          Read_Master_Log_Pos: 444

               Relay_Log_File: mysql-54-relay-bin.000002

                Relay_Log_Pos: 319

        Relay_Master_Log_File: master55.000001

             Slave_IO_Running: Yes

            Slave_SQL_Running: Yes

```

<h4>主机55：
```
[root@mysql-55 ~]# vim /etc/my.cnf

[root@mysql-55 ~]# systemctl restart mysqld

mysql> grant replication slave on *.* to masteruser2@'%' identified by '123456';

mysql> show master status\G;

*************************** 1. row ***************************

             File: master55.000001

         Position: 444

．．．．．

mysql> change master to\

    -> master_host="192.168.4.54",

    -> master_user="masteruser",

    -> master_password="123456",

    -> master_log_file='master54.000001',

    -> master_log_pos=443;

mysql> start slave;

mysql> show slave status\G;

*************************** 1. row ***************************

               Slave_IO_State: Waiting for master to send event

                  Master_Host: 192.168.4.54

                  Master_User: masteruser

                  Master_Port: 3306

                Connect_Retry: 60

              Master_Log_File: master54.000001

          Read_Master_Log_Pos: 595

               Relay_Log_File: mysql-55-relay-bin.000002

                Relay_Log_Pos: 471

        Relay_Master_Log_File: master54.000001

             Slave_IO_Running: Yes

            Slave_SQL_Running: Yes

              Replicate_Do_DB:

          Replicate_Ignore_DB:

           Replicate_Do_Table:

       Replicate_Ignore_Table:

```

<h4>测试：

 - 在54操作－－＞在55验证

 - 54主机
```

mysql> create database db1;

mysql> create table db1.t1(id int);

mysql> insert into db1.t1 values(54);

```
 - 55主机
```
mysql> select * from db1.t1;

+------+

| id   |

+------+

|   54 |

```
 - 在55操作数据－－＞在54验证


 - 55主机
```
mysql> create database db2;

mysql> create table db2.t2(id int);

mysql> insert into db2.t2 values(55);

```
54主机
```
mysql> select * from db2.t2;

+------+

| id   |

+------+

|   55 |

```

<h3>－－复制模式
```
１）基础

异步复制(Asynchronous replication)

　默认工作模式

　　主库执行完一次事务后，立即将结果返回给客户端，并不关心从库是否已经接收并处理

全同步复制( Full synchronous replication)

当主库执行完一次事务，且所有从库都执行了该事务后才返回给客户端

半同步复制(Semisynchronous replication)

介于异步复制和全同步复制之间

主库在执行完一次事务后，等待至少一个从库接收到并写入到relay log中才返回客户端

```

<h4>－－半同步复制

51，52主机设置：

查看是否允许动态加载模块（默认允许）
```
mysql> show variables like "have_dynamic_loading";

+----------------------+-------+

| Variable_name        | Value |

+----------------------+-------+

| have_dynamic_loading | YES   |

+----------------------+-------+

```
<h3>查看是否安装模块
```
mysql> select plugin_name,plugin_status from information_schema.plugins where plugin_name like "%semi%";

Empty set (0.00 sec)

```
<h3>命令行加载插件
```
mysql> install plugin rpl_semi_sync_master soname 'semisync_master.so';　mysql> install plugin rpl_semi_sync_slave soname 'semisync_slave.so';

```
<h3>启用半同步复制
```
mysql> show variables like 'rpl_semi_sync_%_enabled%';

mysql> set global rpl_semi_sync_master_enabled=1;

mysql> set global rpl_semi_sync_slave_enabled=1;

mysql> show variables like 'rpl_semi_sync_%_enabled';

+------------------------------+-------+

| Variable_name                | Value |

+------------------------------+-------+

| rpl_semi_sync_master_enabled | ON    |

| rpl_semi_sync_slave_enabled  | ON    |

+------------------------------+-------+
```
<h3>永久设置：
```
[root@mysql-51 ~]# vim /etc/my.cnf

[mysqld]

plugin-load="rpl_semi_sync_master=semisync_master.so;rpl_semi_sync_slave=semisync_slave.so"

rpl-semi-sync-master-enabled=1

rpl-semi-sync-slave-enabled=1

[root@mysql-51 ~]# systemctl restart mysqld

```