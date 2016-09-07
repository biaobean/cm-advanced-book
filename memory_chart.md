# 如何添加可以查看内存的图表？

缺省的主页页面中不带内存的图表，可以通过在Cloudera Manager顶部的菜单栏“**图表**”->“**图表生成器**”中手工生成然后添加至仪表盘。


其他可以添加的内存相关指标包括：

| 名称 | id | 说明 |
| -- | -- | -- |
| 使用的物理内存 | physical_memory_used | 使用的内存总量，不包括缓冲区或缓存。 |
| 可用物理内存 | physical_memory_memfree | 系统未使用的物理内存量。这是 /proc/meminfo 的“MemFree”字段。 |
| 提交限制 | physical_memory_commit_limit | 目前可用于在系统上分配的内存总量。这是 /proc/meminfo 的“CommitLimit”字段。 |
| 物理内存 Mapped | physical_memory_mapped | 用于使用 mmap 命令映射设备、文件或库的内存总量。这是 /proc/meminfo 的“Mapped”字段。 |
| 物理内存写回 | physical_memory_writeback | 被主动写回磁盘的内存总量。这是 /proc/meminfo 的“Writeback”字段。 |
| 物理内存容量 | physical_memory_total | 可用总物理内存。 |
| 物理内存缓冲区 | physical_memory_buffers | 用于临时存放原始磁盘块的物理内存量。这是 /proc/meminfo 的“Buffers”字段。 |
| 物理内存脏 | physical_memory_dirty | 等待被写回磁盘的内存总量。这是 /proc/meminfo 的“Dirty”字段。 |
| 物理内存高速缓存 | physical_memory_cached | 用于从磁盘读取的文件的物理内存量。这通常称为 pagecache。这是 /proc/meminfo 的“Cached”字段。 |
| 脏物理内存比率 | physical_memory_dirty_ratio | 在其时间段内，进程本身被强制写入脏缓冲区，而不允许执行更多写入操作之前，可填充脏页的最大物理内存百分比。这从 /proc/sys/vm/dirty_ratio 中读取。 |
| 虚拟内存 | mem_virtual | 已使用的虚拟内存 |
| 驻留内存 | mem_rss | 使用的驻留内存 |
