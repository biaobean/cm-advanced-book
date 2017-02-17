To correct this issue, manually download the RPMs and copy them over to the new nodes and install the the packages.
Download and copy the following files over to the new node:
http://archive.cloudera.com/cm5/redhat/6/x86_64/cm/5.5.1/RPMS/x86_64/oracle-j2sdk1.7-1.7.0+update67-1.x86_64.rpm
http://archive.cloudera.com/cm5/redhat/6/x86_64/cm/5.5.1/RPMS/x86_64/cloudera-manager-daemons-5.5.1-1.cm551.p0.8.el6.x86_64.rpm
http://archive.cloudera.com/cm5/redhat/6/x86_64/cm/5.5.1/RPMS/x86_64/cloudera-manager-agent-5.5.1-1.cm551.p0.8.el6.x86_64.rpm
Issue the following rpm commands to install the packages:
$ rpm -i oracle-j2sdk1.7-1.7.0+update67-1.x86_64.rpm
$ rpm -i cloudera-manager-daemons-5.5.1-1.cm551.p0.8.el6.x86_64.rpm
The cloudera-manager-agent-5.5.1-1.cm551.p0.8.el6.x86_64.rpm will have dependencies.  To find the dependencies run the following:        
$ rpm -i cloudera-manager-agent-5.5.1-1.cm551.p0.8.el6.x86_64.rpm
warning: cloudera-manager-agent-5.5.1-1.cm551.p0.8.el6.x86_64.rpm: Header V4 DSA/SHA1 Signature, key ID e8f86acd: NOKEY
error: Failed dependencies:
	libxslt is needed by cloudera-manager-agent-5.5.1-1.cm551.p0.8.el6.x86_64
	cyrus-sasl-plain is needed by cloudera-manager-agent-5.5.1-1.cm551.p0.8.el6.x86_64
	cyrus-sasl-gssapi is needed by cloudera-manager-agent-5.5.1-1.cm551.p0.8.el6.x86_64
	fuse is needed by cloudera-manager-agent-5.5.1-1.cm551.p0.8.el6.x86_64
	portmap is needed by cloudera-manager-agent-5.5.1-1.cm551.p0.8.el6.x86_64
	cloudera-manager-daemons = 5.5.1 is needed by cloudera-manager-agent-5.5.1-1.cm551.p0.8.el6.x86_64
	fuse-libs is needed by cloudera-manager-agent-5.5.1-1.cm551.p0.8.el6.x86_64
	/lib/lsb/init-functions is needed by cloudera-manager-agent-5.5.1-1.cm551.p0.8.el6.x86_64
	httpd is needed by cloudera-manager-agent-5.5.1-1.cm551.p0.8.el6.x86_64
	mod_ssl is needed by cloudera-manager-agent-5.5.1-1.cm551.p0.8.el6.x86_64
	openssl-devel is needed by cloudera-manager-agent-5.5.1-1.cm551.p0.8.el6.x86_64
	python-psycopg2 is needed by cloudera-manager-agent-5.5.1-1.cm551.p0.8.el6.x86_64
	MySQL-python is needed by cloudera-manager-agent-5.5.1-1.cm551.p0.8.el6.x86_64
       4. Install the dependencies using yum.  For example:
$ yum install libxslt cyrus-sasl-plain cyrus-sasl-gssapi portmap fuse-libs /lib/lsb/init-functions httpd mod_ssl openssl-devel python-psycopg2 MySQL-python

      5. Install the agent package:
$ rpm -i cloudera-manager-agent-5.5.1-1.cm551.p0.8.el6.x86_64.rpm
Note:  The above command might report additional dependencies. If so, use yum install, like you did in the earlier step, to install those dependencies as well.

Continue only if you are successful using the rpm commands to install the packages:

     6.  Update "server_host=" in /etc/cloudera-scm-agent/config.ini so it points to the Cloudera Manager Server host.
vi /etc/cloudera-scm-agent/config.ini

      7. Start the agent:
service cloudera-scm-agent start
      8. From Cloudera Manager, click Hosts from the top Navigation menu.  

      9. Click Add A Host, then Continue. A Specify hosts for your CDH cluster installation dialog opens.

      10. Click Managed Hosts and follow the instructions to continue the installation.
