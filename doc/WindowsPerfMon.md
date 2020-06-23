# Perfmon-Windows / SQL Server Counters
  The following set of counters will give you a good indication of any issues that could be affecting any of these areas. The red indicators are the ones that should be used when investigating the root causes of performance issues for the first time. 

## CPU:
**1.[Processor] "% Processor time"** : This is the percentage of total elapsed time that the processor was busy executing.  This counter provides a very general measure of how busy the processor is and if this counter is constantly high, say above 90%, then you'll need to use other counters (described below) in order to further determine the root cause of the CPU pressure.

**2.[Processor] "% Privileged time"**:
This measures the percentage of elapsed time the processor spent executing in kernel mode.  Since this counter takes into account only kernel operations (eg. memory management) a high percentage of privileged time, anything consistently above 25%, can usually point to a driver or hardware issue and should be investigated. 

**3.[Processor] "% User time"**:
The percentage of elapsed time the processor spent executing in user mode.  To describe this simply, it's really the amount of time the processor spent executing any user application code.  Generally, even on a dedicated SQL Server, I like this number to be consistently below 65% as you want to have some buffer for both the kernel operations mentioned above as well as any other bursts of CPU required by other applications.  Anything consistently over 65% should be investigated and this might mean digging deeper into exactly what process is using the CPU (if it's not SQL Server) by using the "[Process] % User time" counter for the individual processes.

**4.[Processor] "Queue Length"**:
This is the number of threads that are ready to execute but waiting for a core to become available.  On single core machines a sustained value greater than 2-3 can mean that you have some CPU pressure.  Similarly, for a multicore machine divide the queue length by the number of cores and if that is continuously greater than 2-3 there might be CPU pressure.
 
## Disk:
**1.[Physical Disk] "Current Disk Queue Length" & "Avg. Disk Queue Length"**:
The "Current Disk Queue Length" a direct measurement of the disk queue length at the time it is sampled so in most cases it is better to monitor "Avg. Disk Queue Length" as this value is derived using the (Disk Transfers/sec)*(Disk sec/Transfer) counters.  Using this calculation gives a much better measure of the disk queue over time, smoothing out any quick spikes.  Having any physical disk with an average queue length over 2 for prolonged periods of time can be an indication that your disk is a bottleneck.

**2.[Physical Disk] "% Idle Time"**:
This is a measure of the percentage of time that the disk was idle. ie. there are no pending disk requests from the operating system waiting to be completed.  Ideally you would want this value to be as high as possible but even low values are acceptable assuming the queue length counters mentioned above are low.

**3.[Physical Disk] "Avg. Disk sec/Read" & "Avg. Disk sec/Write"**:
These both measure the latency of your disks, that is, the average time it takes for a disk transfer to complete.  Although this value is displayed in seconds it is actually accurate down to milliseconds. Good and bad values for these counters are really dependent on your hardware.  For example, you would expect lower values for an SSD as compared to any spinning disk.  For counters like this I find it best to get a baseline after the hardware is installed and use this value going forward to determine if you are experiencing any latency issues related to the hardware.

**4.[Physical Disk] "Disk Reads/sec" & "Disk Writes/sec"**:
These counters each measure the total number of IO requests completed per second.  Similar to the latency counters, good and bad values for these counters depend on your disk hardware but values higher than your initial baseline don't normally point to a hardware issue in this case.  Rather the high values usually mean that there are queries, new or old, that are missing indexes and are reading more data than required.
 
## Memory:
**1.[Memory] "Available MBs"**:
The total amount of available memory on the system.  Usually you would like to keep about 10% free but on systems with a really large amounts of memory (>50GB) there can be less available especially if it is a server that is dedicated to just a single SQL Server instance.  If this value is lower than this then it's not necessarily an issue provided the system is not doing a lot of paging.  If you are paging then further troubleshooting can be done to see which individual processes are using most of the memory.

**2.[Memory] "Pages/sec"**: 
This is actually the sum of "Pages Input/sec" and "Pages Output/sec" counters which is the rate at which pages are being read and written as a result of pages faults.  Small spikes with this value do not mean there is an issue but sustained values of greater than 50 can mean that system memory is a bottleneck.

**3.[Memory] "Paging File(_Total)\% Usage"**:
The percentage of the system page file that is currently in use.  This is not directly related to performance, but you can run into serious application issues if the page file does become completely full and additional memory is still being requested by applications.   

## Network:
**1.[Network interface] "Bytes total/sec"**:
This counter measures the number of bytes being transferred through your network adaptor.  You want to make sure that your network adaptor can handle the amount of traffic flowing in and out of your server and ideally you would not want to see this value go over 75% (ie. 75MB/s for a 100MB/s network adaptor) for prolonged periods of time.  If you do see values above this you should consider adding another network adaptor or you can use the "Bytes sent/sec" and "Bytes received/sec" counters to determine if it's incoming or outgoing traffic that is causing the bottleneck.
 
## SQL:
**1.[SQLServer:Access Methods] "Full scans/sec"**:
The number of unrestricted full table or index scans per second.  This value should also be very close to 0.  While full scans, especially index scans, are not completely unavoidable and are not always bad, if this number is high you might want to look into any tables that may be missing an index or look for poorly written queries.

**2.[SQLServer:Access Methods] "Forwarded Records/sec"**:
The number of rows per second fetched from a heap through forwarded record pointers.  Having many forwarded record pointers means that there were many updates to rows that no longer fit on the original page.  In almost all instances, this number should be very close to 0.  If your table has a clustered index and you have the fill factor set correctly you should very rarely encounter any forwarded records.  If for some reason you do have a heap table that has a lot of forwarded record pointers the only options for fixing them are to rebuild the heap (must be SQL Server 2008+) or add a clustered index to the table and set the fill factor correctly.

**3.[SQLServer:Access Methods] "Page Splits / Sec"**:
The number of page splits per second.  Determining what is a good number of page splits depends on many factors.  How many writes you have, fill factor settings, etc.  In my experience, you encounter a lot of page splits when you have a workload with a lot of inserts/updates and having indexes with a fill factor that is set too high.  When fixing this by setting the fill factor lower be careful not to set it too low since this will cause the data to become very spread out and use more disk space than necessary.

**4.[SQLServer:Buffer Manager] "Buffer Cache hit ratio"**:
This ratio is a measure of the percentage of pages that were found in memory (SQL buffer pool) without having to be read from disk.  After server startup this value will be very low as all data will have to be read from disk but over time it should level off so it should not really be measured until the server has had some time to warm up.  Generally the higher the better and in order to get a better hit ratio your only real option is to increase the amount of memory available to SQL Server.   Some editions of SQL Server starting with SQL Server 2014 have the buffer pool extension feature which can use SSDs to increase the size of the SQL Server buffer pool.

**5.[SQLServer:Buffer Manager] "Page life expectancy"**:
The number of seconds that a page should be able to stay in memory without being referenced.  As with a lot of performance counters, good values for this counter depend a lot on your workload but most best practices say this should be at least 300 seconds.  Keep in mind that this measure is an instantaneous measure and not a rolling average so if you have a time of day where a lot of big report queries are run then you could expect this value to be lower given the amount of data that will have to be read.

**6.[SQLServer:Buffer Manager] "Checkpoint Pages / Sec"**:
This counter shows the number of dirty pages that are moved from the SQL buffer pool to disk during a checkpoint.  The number of pages that you would expect to be moved per second depends a lot on your system and its usage.  If this counter is higher than normal you can use indirect checkpoints to reduce the number of pages flushed per second.

**7.[SQLServer:Memory Manager] "Memory Grants Pending"**:
This is defined as the total number of SQL Server processes that are waiting for workspace memory to be granted.  If you are not experiencing any memory pressure then this value should almost always be zero.  Any value that is consistently above zero could be an indication that you are experiencing memory pressure and you should consider allocating more server memory to SQL Server.

**8.[SQLServer:Memory Manager] "Total Server Memory (KB)" & "Target Server Memory (KB)"**:
"Total Server Memory (KB)" specifies the amount of memory the server has committed using the memory manager while "Target Server Memory (KB)" indicates the amount of memory that SQL Server can potentially consume.  If after some amount of time the "Total Server Memory (KB)" is consistently lower that "Target Server Memory (KB)" then you could potentially move this SQL Server instance to a server with less memory since it doesn't actually need all the memory you have allocated to it.  What you want to look out for is when these two values are equal and you have a low "Buffer Cache Hit Ratio" or the "Memory Grants Pending" is consistently above zero.  In these cases you could be experiencing memory pressure.

**9.[SQLServer:Locks] "Average Wait Time (ms)" & "Lock Waits / Sec"**:
"Average Wait Time (ms)" is the average amount of time waited in milliseconds for each lock request that resulted in a wait.  "Lock Waits / Sec" is the number of lock requests per second that required the lock requestor to wait.  "Average Wait Time (ms)" should be very low, under 50ms and "Lock Waits / Sec" should ideally be very close to 0.  If either value is over these thresholds you should start looking into your queries as usually this means there are poorly written queries or tables missing indexes that result in more locking/blocking than is necessary.

**10.[SQLServer:SQL Statistics] "Batch Requests/Sec"**:
This counter tells us the number of T-SQL commands that are being received by the server per second.  There are no good or bad values for this counter, it is more a measure of how busy the SQL Server instance is.  As we've done with other counters if you have a regular baseline value for this counter you can refer back to it to see if your server is more or less busy than normal.

**11.[SQLServer:General Statistics] "User Connections"**:
This measures the number of current connections to SQL Server.  As with the "Batch Requests/Sec" counter this counter is simply a good indicator for how busy your SQL Server instance is, more users usually leads to more queries which leads to more resource usage.

**12.[SQLServer:SQL Statistics] "SQL Compilations/Sec" & "SQL Re-Compilations/Sec"**:
"SQL Compilations/Sec" measures the number of times per second a SQL statement was compiled.  After a SQL Server instance has been running for some amount of time this value should stabilize (it will be high after any restart as the plan cache is rebuilt).  "SQL-Recompilations/Sec" is the number of times per second a SQL statement had to be recompiled.  These occur after a large number of inserts/updates/deletes causes the statistics to be updated or after a schema change.  This value should usually be very low.

