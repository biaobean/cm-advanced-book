# 如何修改主机IP


## 1. 介绍
本节介绍如何在Cloudera Manager中更新已有主机的IP信息，操作后除IP地址以外的其他信息，如主机名、节点属性、配置、环境等皆不变，整个过程中无添加或移除节点操作。

## 2. 操作影响
操作过程中需要停止集群服务、停止Cloudera Manager服务；需要禁止其他用户进行集群操作。

## 3. 步骤
###	3.1 停止集群服务
在Cloudera Manager界面，停止集群组件服务
###	3.2 停止集群管理服务
在Cloudera Manager界面，停止集群管理服务
###	3.3	停止CM Server服务
在Cloudera Manager节点上，进入命令行执行：

```
service cloudera-scm-server stop
```

### 3.4	停止CM Agent服务
在所有IP地址发生变更的节点上，进入命令行执行

```
service cloudera-scm-agent stop
```

### 3.5	备份CM Server数据库
备份Cloudera Manager Server的后台数据库，如果外部数据库选用的是Mysql，可以使用mysqldump命令，如下示例：

```
mysqldump -uroot -prootroot cm > ~/cm_bak_20150423.sql
mysqldump -uroot -prootroot metastore > ~/metastore_bak_20150423.sql
```

### 3.6	修改主机名文件
在所有IP地址发生变更的节点上，编辑*/etc/hosts*文件，修改IP地址，如下示例：

```
cp /etc/hosts /etc/hosts.bak
vim /etc/hosts
```

hosts文件格式示例如下，第一列为IP，第二列为机器名称：

```
127.0.0.1   localhost
168.9.2.201 hdhead1
168.9.2.202 hdhead2
168.9.2.204 hddata1
168.9.2.205 hddata2
168.9.2.206 hddata3
168.9.2.207 hddata4
168.9.2.208 hddata5
168.9.2.209 hddata6
```

### 3.7	修改config.ini文件
在所有IP地址发生变更的节点上，编辑config.ini文件，修改CM Server的IP地址：

```
vim /etc/cloudera-scm-agent/config.ini
```

比如，修改原server host地址168.9.3.12为新地址168.9.2.202

```
#server_host=168.9.3.12
server_host=168.9.2.202
```

### 3.8	修改CM数据库的HOST表
修改CM外部数据库中的HOST表，更新IP_ADDRESS，如下示例：
#### 3.8.1．进入 mysql

```
mysql> use cm;
mysql> SELECT NAME, IP_ADDRESS FROM HOSTS;
+---------+------------+
| NAME    | IP_ADDRESS |
+---------+------------+
| hdhead2 | 168.9.3.12 |
| hdhead1 | 168.9.3.11 |
| hddata2 | 168.9.3.52 |
| hddata1 | 168.9.3.51 |
| hddata6 | 168.9.3.56 |
| hddata3 | 168.9.3.53 |
| hddata5 | 168.9.3.55 |
| hddata4 | 168.9.3.54 |
+---------+------------+

8 rows in set (0.00 sec)
```

#### 3.8.2．通过NAME更新 IP_ADDRESS

```
update HOSTS set IP_ADDRESS='168.9.2.201' where NAME='hdhead1';
commit;
update HOSTS set IP_ADDRESS='168.9.2.202' where NAME='hdhead2';
commit;
update HOSTS set IP_ADDRESS='168.9.2.204' where NAME='hddata1';
commit;
update HOSTS set IP_ADDRESS='168.9.2.205' where NAME='hddata2';
commit;
update HOSTS set IP_ADDRESS='168.9.2.206' where NAME='hddata3';
commit;
update HOSTS set IP_ADDRESS='168.9.2.207' where NAME='hddata4';
commit;
update HOSTS set IP_ADDRESS='168.9.2.208' where NAME='hddata5';
commit;
update HOSTS set IP_ADDRESS='168.9.2.209' where NAME='hddata6';
commit;
```

#### 3.8.3．检查新的IP_ADDERSS

```
mysql> SELECT NAME, IP_ADDRESS FROM HOSTS;

+---------+-------------+
| NAME    | IP_ADDRESS  |
+---------+-------------+
| hdhead2 | 168.9.2.202 |
| hdhead1 | 168.9.2.201 |
| hddata2 | 168.9.2.205 |
| hddata1 | 168.9.2.204 |
| hddata6 | 168.9.2.209 |
| hddata3 | 168.9.2.206 |
| hddata5 | 168.9.2.208 |
| hddata4 | 168.9.2.207 |
+---------+-------------+

8 rows in set (0.00 sec)
```

### 3.9	启动CM Server服务
在Cloudera Manager节点上，进入命令行执行：

```
service cloudera-scm-server start
```
### 3.10	启动CM Agent服务
在所有IP地址发生变更的节点上，进入命令行执行：

```
service cloudera-scm-agent start
```

### 3.11	启动集群管理服务
在Cloudera Manager界面，启动集群管理服务
### 3.12	启动集群服务
在Cloudera Manager界面，启动集群组件服务
### 3.13	部署客户端配置
在Cloudera Manager界面，点击“部署客户端配置”
### 3.14	验证集群
1．在Cloudera Manager界面，观察各个组件状态是否正常
2． 进入“主机”界面，观察主机IP地址是否已更新
3． 进入命令行，分别执行hadoop fs、hive、impala-shell等命令验证组件工作是否正常，如下示例：
```
[root@hdhead1 ~]# hadoop fs -ls /

Found 4 items

drwxr-xr-x   - hdfs supergroup          0 2015-03-24 13:16 /htdata
drwxr-xr-x   - hdfs supergroup          0 2015-03-16 15:01 /opt
drwxrwxrwt   - hdfs supergroup          0 2015-03-24 11:43 /tmp
drwxrwxrwx   - hdfs supergroup          0 2015-03-24 13:32 /user
```