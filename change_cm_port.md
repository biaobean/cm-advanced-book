# 如何后台修改CM端口

可以通过CM的Rest API进行修改。参数名称为*http_port*，接口地址为*api/v8/cm/config*，可以跟一个可选的*message*参数用来记录此次修改的信息。

```
curl -X PUT -H "Content-Type:application/json" -u <用户名>:<密码> -d '{ "items": [ { "name": "http_port", "value": "<修改后的port>" } ] }' 'http://<CM服务器地址>:<CM端口>/api/v8/cm/config?message=<版本消息>'
```

注意copy时不要丢下任何一个引号。例如，下面是登陆到CM本机，使用缺省的用户名和密码将端口修改为8198：

```
curl -X PUT -H "Content-Type:application/json" -u admin:admin -d '{ "items": [ { "name": "http_port", "value": "8198" } ] }' 'http://localhost:7180/api/v8/cm/config'
```

PS，另外一个不需要修改端口的方式是在访问Web页面时使用使用ssh tunel等方式：
```
ssh -Nfl <ip>:<new port>:<ip>:7180 <user>@<ip>
```

比如：

```
ssh -Nfl 10.255.255.15:7555:10.255.255.15:7180 root@10.255.255.15
```
