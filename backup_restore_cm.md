# 如何备份和恢复CM配置

## 使用CM API

可以使用CM的REST API来导出和导入所有配置数据。API导出的是一个JSON文件，里面包含了所有CM里管理的实例的配置数据。可以直接将这个JSON文件导入CM，从而恢复CM的部署。

### 备份配置
Minimum Required Role: Cluster Administrator (also provided by Full Administrator)

Exporting the Cloudera Manager Configuration
Log in to the Cloudera Manager server host as the root user.
Run the following command:
```
# curl -u admin_uname:admin_pass "http://cm_server_host:7180/api/v13/cm/deployment" >
path_to_file/cm-deployment.json
```

Where:
admin_uname is a username with either the Full Administrator or Cluster Administrator role.
admin_pass is the password for the admin_uname username.
cm_server_host is the hostname of the Cloudera Manager server.
path_to_file is the path to the file where you want to save the configuration.

#### 导出时剔除掉敏感信息

The exported configuration may contain passwords and other sensitive information. You can configure redaction of the sensitive items by specifying a JVM parameter for Cloudera Manager. When you set this parameter, API calls to Cloudera Manager for configuration data do not include the sensitive information.
**Important: If you configure this redaction, you cannot use an exported configuration to restore the configuration of your cluster due to the redacted information.**
To configure redaction for the API:
Log in the Cloudera Manager server host.
Edit the /etc/default/cloudera-scm-server file by adding the following property (separate each property with a space) to the line that begins with export CMF_JAVA_OPTS:

```
-Dcom.cloudera.api.redaction=true
```

For example:

```
export CMF_JAVA_OPTS="-Xmx2G -Dcom.cloudera.api.redaction=true"
```

Restart Cloudera Manager:

```
sudo service cloudera-scm-server restart
```

### 恢复CM配置

**注意：这个功能做属于CM高级功能，需要商业版本的许可证，免费版CM无此功能。**

Using a previously saved JSON document that contains the Cloudera Manager configuration data, you can restore that configuration to a running cluster.

Using the Cloudera Manager Administration Console, stop all running services in your cluster:
On the Home > Status tab, click  to the right of the cluster name and select Stop.
Click Stop in the confirmation screen. The Command Details window shows the progress of stopping services.
When All services successfully stopped appears, the task is complete and you can close the Command Details window.

Warning: If you do not stop the cluster before making this API call, the API call will stop all cluster services before running the job. Any running jobs and data are lost.
Log in to the Cloudera Manager server host as the root user.
Run the following command:
```
# curl --upload-file path_to_file/cm-deployment.json
-u admin_uname:admin_pass http://cm_server_host:7180/api/v13/cm/deployment?deleteCurrentDeployment=true
```
Where:
admin_uname is a username with either the Full Administrator or Cluster Administrator role.
admin_pass is the password for the admin_uname username.
cm_server_host is the hostname of the Cloudera Manager server.
path_to_file is the path to the file containing the JSON configuration file.

## 备份数据库

## 参考资源
1. http://www.cloudera.com/documentation/enterprise/latest/topics/cm_intro_api.html