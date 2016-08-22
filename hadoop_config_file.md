# 如何获取Hadoop配置文件

Hadoop使用xml等文件做配置，但在被CM接管集群以后，文件内容也由CM动态控制。但由于历史或调试等原因，可能需要查看具体Hadoop真实使用的xml配置文件的内容，可以通过以下方式来获得。

一般情况下，没有更改配置或者更改配置后及时重启服务生效，下面两种方式的效果相同，区别在于一个是在CM端看，一个是在节点服务器上看，可视实际情况择方便使用。

**注意，由于Hadoop服务和客户端有可能有不一样的配置，因此CM对即使在同一台服务器上的不同的角色使用不同的配置文件分别管理。**

## 获取服务角色配置
### 获取当前CM管理的最新配置文件

1. 获取某个服务的所有角色：
```
http://cm_server_host:7180/api/v13/clusters/clusterName/services/serviceName/roles
```
2. 查找到具体角色，并所使用的配置文件列表：
```
http://cm_server_host:7180/api/v13/clusters/clusterName/services/serviceName/roles/roleName/process
```
3. 获得具体角色所使用的某个配置文件的具体内容：
```
http://cm_server_host:7180/api/v13/clusters/clusterName/services/serviceName/roles/roleName/process/configFiles/configFileName
```

比如：

```
http://cm_server_host:7180/api/v13/clusters/Cluster%201/services/OOZIE-1/roles/OOZIE-1-OOZIE_SERVER-e121641328fcb107999f2b5fd856880d/process/configFiles/oozie-site.xml
```

注意：服务名称、角色名称等在CM中的唯一标识名称一般是CM自动生成的，需要试实际情况查询更改。

### 获取当前正在使用的配置文件

1. 登录角色服务器，查询相应的服务进程号，比如：
```
ps -elf | grep java
```
2. 并进入*/var/run/cloudera-scm-agent/process*目录，根据具体角色类型以及上一步看到的进程号进入相应子目录。注意，因为可能有残留目录，对应的角色有不止一个目录，需要根据进场号确认。
3. 目录内为该角色所使用的所有配置

## 获取客户端配置
### 通过API
### 本地查找
在缺省目录，如果通过parcel方式安装，路径在*/opt/cloudera/parcels/CDH/lib*相应目录下，如HDFS的配置文件目录为*/opt/cloudera/parcels/CDH/lib/hadoop/etc/hadoop*。