# 大规模集群CM部署


Recommendations for Cloudera Manager (CM) Deployments running ~1000 node clusters
(revised for CM 5.5.0)

Hardware/Topology

Provision CM over 4 separate hosts (+ 1 host if not already using external database)
Put SCM server on its own host
SMON on a 2nd host, 
ReportsManager on 3rd host
Remaining Cloudera management services roles (navigator/alert publisher) on 4th host
Always dedicate CM hosts for Cloudera Manager processes; never co-locate CDH service roles onto CM hosts.
SCM database server should not be on any of the above hosts, but located “close” to the base SCM server ( < 1ms ping latency from scm-server host)
Size that machine similar to how you size the scm-server host

Machine characteristics
CPU 
16 physical cores at minimum for SMON and SCM server hosts; preferably 24 physical cores. Standard Xeon chassis (Intel(R) Xeon(R) Ivy Bridge or later, at 2.50Ghz or better)
ReportsManager and the other service roles aren’t very CPU intensive, but we wouldn’t recommend going below 8 physical cores each on those machines.
Disk
Provision disk carefully
RAIDed or enterprise-class SSD deployments recommended at this scale - specifically for SMON and SCM server hosts
Configure your disks with LVM, so if you run out of space, you can quickly increase space by expanding your disks.
ReportsManager index file can get big - 8x-16x the size of the HDFS NameNode fsimage - make sure you have 16x NN fsimage size for disk space at minimum on that host.
Free disk space of 100GB at minimum for /var/log, /var/run partitions should be sufficient for all hosts hosting CM.
RAM & Heap allocation
Give 16GB heap at minimum to SCM server, SMON and HMON
ReportsManager heap memory requirements depend on HDFS NameNode FSImage size. So you'll need to add more resources as you add more files to HDFS.
16GB is a good start, but expect to retune as you grow your cluster.
RAM sizing should be at least 50% more than the heap sizing recommended above, to allow for Java management overhead and for the operating system - implying you really want to have at least 24GB RAM on each of these hosts.
Why we believe CM 5.5.0 behaves better at scale
Service restart and “first run” for a new cluster has been parallelized across independent services
Command timeout behavior in general, is more graceful - command timeouts are more accurate with respect to actual start time.
Service and role start/stop, staleness checking have had significant performance improvements
DR replication performance has been significantly improved for clusters with many millions of files; UI fixes many pressing issues.
Several UI pages have pagination and filtering enhancements to work better with large numbers of entities.
Known Issues

Parcel distribution will be slow; this is a known issue because of the single CM server being a bottleneck; addressed in 5.7 (OPSAPS-25550)

Some pages on the UI will likely be slow (we know these are problem areas)
Home Page w/ Monitoring, this shows a lot of information so it has the potential to be really slow.
Config Issues, Health Issues as the # of issues presented is unbounded.

Some pages on the UI which might be slow
Hosts & Role Instances (Large # of entries, but we do pagination already it shouldn't be too bad)
Command Details  (When you drill down enough, it could get sluggish even though we already do pagination and showing 100 sub-steps at a time)
Agent Install - no pagination, so could be slow if you attempt to install a large number of hosts in parallel from the agent UI
Parcel Distribution and Usage pages can be slow to load (though better than 5.4.0 due to the significant redesign)
