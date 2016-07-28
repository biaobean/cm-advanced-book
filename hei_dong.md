# 黑洞

## 神秘的hostid
CM如何决定和管理host的内部标识符hostid，对我还是个谜。前段时间发现：

1. CM自动产生的hostid有些风格是UUID的（比如“65747d3-4496-4445-b8aa-dda76617f298”），有些是i-xxxx（比如“i-20107787”）风格的。用户也可以自己指定机器的hostid。
2. 相同IP和name的机器可以用不同的hostid在数据库里有多条记录，但只有一条是能够heartbeat的，其它记录在主机页面里形同陌路。
3. hostid记录在CM数据库的HOSTS表以及安装了agent的服务器的/var/lib/cloudera-scm-agent/uuid文件中。
4. 发现有时候服务器上并没有/var/lib/cloudera-scm-agent/uuid文件。似乎和添加机器或安装方式有关。
5. 对于手工安装的机器，如果agent启动时没有uuid文件，CM将为其另分配hostid，即使HOSTS表里已经有记录。如果自己编辑了uuid，CM将用这个id*并在后面加上一个回车符*做为hostid。因此，完全做不到“自定义”hostid，不知道是否为CM的bug。
