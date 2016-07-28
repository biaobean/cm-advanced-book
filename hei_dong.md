# 黑洞

## 神秘的hostid
CM如何决定和管理host的内部标识符hostid，对我还是个谜。前段时间发现：

1. 相同IP和name的机器可以用不同的hostid在数据库里有多条记录，但只有一条是能够heartbeat的，其它记录在主机页面里形同陌路。
2. CM自动产生的hostid有些风格是UUID的（比如“65747d3-4496-4445-b8aa-dda76617f298”），有些是i-xxxx（比如“i-20107787”）风格的。用户也可以自己指定机器的hostid。
3. hostid记录在CM数据库的HOSTS表以及安装了agent的服务器的/var/lib/cloudera-scm-agent/uuid文件中。但发现有时候服务器上没有/var/lib/cloudera-scm-agent/uuid文件。