# 如何在现有Hadoop部署上安装CM


## 管理服务器Server安装配置

## 节点服务器Agent配置

本节所有路径皆为通过rpm包方式安装的缺省路径。

【步骤1】编辑agent服务config.ini配置

文件路径为*/etc/cloudera-scm-agent/config.ini*。

1.1 修改CM的hostname：

```
# Hostname of Cloudera SCM Server
server_host=zchadoop218
```

如果CM的监听端口非默认7182，还需修改*server_port*参数。

1.2 （可选）如果默认9000端口不可用，可修改agent监听服务端口：

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

有两种方式，建议使用方式一：

方法一：
修改*/usr/lib64/cmf/service/common/cloudera-config.sh*文件，在文件开始加入**JAVA_HOME**声明，比如：

```
export JAVA_HOME=/usr/java/jdk1.7.0_79
```

方法二：
依次进入*/usr/lib64/cmf/service/*下对应组件的目录，修改以下文件hdfs.sh、yarn.sh、zkserver.sh、hive.sh、hbase.sh，修改JAVA_HOME调用：

```
# attempt to find java
#locate_cdh_java_home	//注释该行
export JAVA_HOME=/usr/java/jdk1.7.0_79 //添加该行
```

2.2 修改**HBASE_BIN**路径

假设hbase命令的路径为*/opt/boh-2.0.0/core/hbase/bin*。有两种方式修改，建议使用方式一：

方法一：
修改*/usr/lib64/cmf/service/common/cloudera-config.sh*文件中
函数locate_hbase_script()实现：

原有内容：

```
# Sets the path to the HBase script in HBASE_BIN.
locate_hbase_script() {
  if [ "$CDH_VERSION" -ge "5" ]; then
    # CDH-13250 use bigtop script to start hbase
    # Disable sourcing defaults dir, which CM will manage instead.
    export BIGTOP_DEFAULTS_DIR=""
    HBASE_BIN="$HBASE_HOME/../../bin/hbase"
  else
    HBASE_BIN="$HBASE_HOME/bin/hbase"
  fi
}
```

更改为：

```
locate_hbase_script() {
    export HBASE_BIN="/opt/boh-2.0.0/core/hbase/bin"
}
```

方法二：
在*/usr/lib64/cmf/service/hbase/hbase.sh*文件中替换*locate_hbase_script*调用：（**注意一定要注释掉locate_hbase_script调用**）

原有内容：
```
locate_hbase_script
```

更改为：

```
#locate_hbase_script
export HBASE_BIN=/opt/boh-2.0.0/core/hbase/bin
```


【步骤3】分发修改后的配置文件到所有节点

/opt/cm-5.1.3
```
for i in `cat ~/ip`
do
scp -rp /opt/cm-5.1.3 $i:/opt &
done
```

注：/opt目录用户需要有写的权限

【步骤4】修改所有节点的系统配置

```
echo never > /sys/kernel/mm/transparent_hugepage/defrag
```

【步骤5】在所有节点添加cloudera-scm用户

```
useradd --system --home=/var/lib/cloudera-scm-server --no-create-home --shell=/bin/false --comment "Cloudera SCM User" cloudera-scm
```

其中，如果非rpm方式安装，可能需要修改home目录。

【步骤6】在所有节点安装JDK

```
yum remove `rpm -qa |grep java*` -y
rpm -ivh /home/hadoop/jdk-7u79-linux-x64.rpm
```

【步骤7】（可选）在所有节点配置mysql库jdbc包

```
cd /usr/share/java
cp /home/hadoop/mysql-connector-java-5.1.29.jar ./
ln -s mysql-connector-java-5.1.29.jar mysql-connector-java.jar
```

【步骤8】启动所有节点agent服务，在页面检查主机状态

```
/opt/cm-5.1.3/etc/init.d/cloudera-scm-agent start
```


