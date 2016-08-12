# 如何后台修改CM端口

可以通过CM的Rest API进行修改。参数名称为*http_port*，接口地址为*api/v8/cm/config*，可以跟一个可选的*message*参数用来记录此次修改的信息。
```
curl -X PUT -H "Content-Type:application/json" -u <用户名>:<密码> -d '{ "items": [ { "name": "http_port", value: 8198 } ] }' 'http://<CM服务器地址>:<CM端口>/api/v8/cm/config?message=<版本消息>'
```
例如：
```
curl -X PUT -H "Content-Type:application/json" -u admin:admin -d '{ "items": [ { "name": "http_port", value: 8198 } ] }' 'http://localhost:7180/api/v8/cm/config'
```