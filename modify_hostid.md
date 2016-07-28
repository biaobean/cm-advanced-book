# 如何修改服务器的hostid

If customer
     doesnot have old UUID file, clean up the cloudera manager database table .



 



1. Identify and copy the new UUID
for each host in /var/lib/cloudera-scm-agent/uuid



2. Stop the agent on the duplicate
hosts without roles, and delete the duplicate host entries from the Hosts page
in Cloudera Manager



3. Stop Cloudera
Manager server to prevent any heartbeats that are capable of modifying
HOSTS table in future steps



# service
cloudera-scm-server stop



4. Check database credentials
and com.cloudera.cmf.db.name (usually scm, or cm)
from /etc/cloudera-scm-server/db.properties.



5. Login to specific cm database
instance using credentials and db instance name from step #4 above



 Replace $SCM_NAME below
     with the com.cloudera.cmf.db.name)



 # psql -d $SCM_NAME -U
scm -h localhost -p 7432



 # mysql -u scm -p $scm_name



6. Confirm HOSTS table in scm
database



scm=> select * from
HOSTS;



 



mysql>SELECT
HOST_ID,NAME,IP_ADDRESS,HOST_IDENTIFIER FROM HOSTS;



IMPORTANT: Ensure to note the host_id (first
column, this is the $HOST_ROWID below) for each of the nodes being modified



7. Update host uuid in the
database. For the hosts being changed, use the command below:



scm=> update HOSTS set
HOST_IDENTIFIER = ‘new.UUID from agent' where HOST_ID = $HOST_ROWID;



 



mysql> update HOSTS set
HOST_IDENTIFIER = ‘new.UUID from agent' where HOST_ID = $HOST_ROWID;



8.  Cleanup supervisors and
agent settings on each of the affected nodes with a clean_restart



# service cloudera-scm-agent
clean_restart
