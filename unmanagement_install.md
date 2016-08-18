# 如何在现有Hadoop部署上安装CM


## 管理服务器Server安装配置

## 节点服务器Agent配置

【步骤1】编辑agent服务config.ini配置

```
vi /opt/cm-5.1.3/etc/cloudera-scm-agent/config.ini
```

1.1 修改CM的hostname：

```
# Hostname of Cloudera SCM Server
server_host=zchadoop218
```

1.2 修改agent监听服务端口：

```
listening_port=9900
```

1.3 修改[hadoop]标签下缺省的路径，指向当前集群的tarball的路径：

```
cdh_hadoop_bin=/opt/boh-2.0.0/core/hadoop/bin
cdh_hadoop_home=/opt/boh-2.0.0/core/hadoop/
cdh_hdfs_home=/opt/boh-2.0.0/core/hadoop/
cdh_yarn_home=/opt/boh-2.0.0/core/hadoop/
cdh_hbase_home=/opt/boh-2.0.0/core/hbase/
cdh_hive_home=/opt/boh-2.0.0/core/hive/
cdh_mr2_home=/opt/boh-2.0.0/core/hadoop/
cdh_zookeeper_home=/opt/boh-2.0.0/core/zookeeper
```

【步骤2】编辑cm配置脚本

2.1 修改JAVA_HOME

```
cd /opt/cm-5.1.3/lib64/cmf/service
```

依次进入对应组件的目录，修改以下文件hdfs.sh、yarn.sh、zkserver.sh、hive.sh、hbase.sh。

```
# attempt to find java
#locate_cdh_java_home	//注释该行
export JAVA_HOME=/usr/java/jdk1.7.0_79 //添加该行
```

2.2 hbase.sh需要修改**HBASE_BIN**路径

```
#locate_hbase_script
export HBASE_BIN=/opt/boh-2.0.0/core/hbase/bin/
```

2.3	分发修改后的/opt/cm-5.1.3

```
for i in `cat ~/ip`
do
scp -rp /opt/cm-5.1.3 $i:/opt &
done
```

注：/opt目录hadoop用户需要有写的权限

2.4	修改所有节点的系统配置

```
echo never > /sys/kernel/mm/transparent_hugepage/defrag
```

2.5	在所有节点添加cloudera-scm用户

```
useradd --system --home=/opt/cm-5.1.3/run/cloudera-scm-server/ --no-create-home --shell=/bin/false --comment "Cloudera SCM User" cloudera-scm
```

2.6	在所有节点安装jdk-7u79-linux-x64.rpm

```
yum remove `rpm -qa |grep java*` -y
rpm -ivh /home/hadoop/jdk-7u79-linux-x64.rpm
```

2.7	在所有节点配置mysql库jdbc包

```
cd /usr/share/java
cp /home/hadoop/mysql-connector-java-5.1.29.jar ./
ln -s mysql-connector-java-5.1.29.jar mysql-connector-java.jar
```

2.8	启动所有节点agent服务，在页面检查主机状态

```
/opt/cm-5.1.3/etc/init.d/cloudera-scm-agent start
```


