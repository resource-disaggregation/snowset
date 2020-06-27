## Profiling Stats

The dataset contains high-fidelity, coarse-grained profiling statistics at a per-query level which can potentially give insight into where queries are spending their time. 

### Overview

A profiler thread is forked in every worker process to monitor the activity of all the threads of the worker process. Every 10msec, the profiler thread wakes up and scans all active worker threads to see their states and records various statistics / metrics.

The profiling time breakdowns are available along two dimensions: 1.) by resource and 2.) by operator

### By Resource (prof\*)

These statistics provide a breakdown of CPU time into the following: 1.) Time during which CPU is busy (**profCpu**) 2.) Time during which CPU is idle (**profIdle**) and 3.) Time during which CPU is blocked on various resources/activities (**prof\*** other than **profCpu** and **profIdle**). The accounting for these stats is done as follows:

Every time the profiler thread wakes up (every 10msec) it will detect how many threads are running vs waiting for a resource (like disk I/O) and compute metrics like CPU usage and wasted CPU time waiting on resources or CPUs idle. Every active worker thread will update its state to report if it is __running__ or __waiting for a resource__ (and if so what resource it is waiting on). Therefore looking at the state itself doesn't say if a thread is currently scheduled on a CPU and running. The OS may have it context switched out even if it thinks it is running. Computing wait time is therefore done by estimations, assuming the worker process is given 100% of the CPUs of its DOP (maximum number of cores a worker process of the query can use. This number is available in the **perServerCores** field in the dataset).

A worker forks more threads than its DOP. A thread waiting for a resource is not necessarily counted as a blocked state if all the CPUs are in use by other worker threads. In that case the workload is still CPU bound.

* If there's more threads claiming they are running than the DOP, then we can assume they are sharing the CPUs and just context switch. In that case we report 100% busy. (profCpu is incremented to account for the full time slice)
* If there's fewer threads claiming they are __running__ than the DOP, then we assume some CPUs are not used. In that case, the threads in __waiting__ state are examined, all the wait types (prof\* other than profCpu/profIdle) are tallied, and equally assigned a portion of the unused CPUs. The logic is that if these wait states did not exist, these threads would grab a portion of that unused CPU.
* If there's fewer threads __waiting__ or __running__ than the DOP, then there's more CPUs than can be used: These are reported as IDLE CPU time.

The full list of __by resource__ breakdown fields are given below:

Column | Description | Units | Datatype
:------:|:-----:|:-----:|:-----:
**profIdle**|CPU Idle time|Milliseconds|int64
**profCpu**|CPU busy time|Milliseconds|int64
**profPersistentReadCache**|Time blocked on persistent data read from cache|Milliseconds|int64
**profPersistentWriteCache**|Time blocked on persistent data write to cache|Milliseconds|int64
**profPersistentReadS3**|Time blocked on persistent data read from S3|Milliseconds|int64
**profPersistentWriteS3**|Time blocked on persistent data write to S3|Milliseconds|int64
**profIntDataReadLocalSSD**|Time blocked on intermediate data read from local SSD|Milliseconds|int64
**profIntDataWriteLocalSSD**|Time blocked on intermediate data write to local SSD|Milliseconds|int64
**profIntDataReadS3**|Time blocked on intermediate data read from S3|Milliseconds|int64
**profIntDataWriteS3**|Time blocked on intermediate data write to S3|Milliseconds|int64
**profRemoteExtRead**|?|Milliseconds|int64
**profRemoteExtWrite**|?|Milliseconds|int64
**profResWriteS3**|Time blocked on writing result to S3|Milliseconds|int64
**profFsMeta**|Time blocked on filesystem metadata operations|Milliseconds|int64
**profDataExchangeNet**|Time blocked on network for intermediate data exchange|Milliseconds|int64
**profDataExchangeMsg**|Time blocked on network for intermediate data exchange|Milliseconds|int64
**profControlPlaneMsg**|Time blocked on network for communication with control plane (cloud services)|Milliseconds|int64
**profOs**|CPU time spent in kernel mode processing|Milliseconds|int64
**profMutex**|CPU time spent contending for mutexes|Milliseconds|int64
**profSetup**|Time spent in worker process setup|Milliseconds|int64
**profSetupMesh**|Time spent in setting up the mesh of network connections among worker processes|Milliseconds|int64
**profTeardown**|Time spent in teardown operartions|Milliseconds|int64


### By Operator (prof\*Rso)

These statictics give a breakdown of time based on the operator being processed (scan, filter, join etc..). The full list of these fields is given in the table below. Note that within each operator different resources may be consumed. (for example within profScanRso, some of that time may be spent on CPU, some of it may be spent blocked on disk I/O etc..) Unfortunately and internal breakdown of how much time is spent doing what __within each operator__ is not available in the dataset.

Column | Description | Units | Datatype
:------:|:-----:|:-----:|:-----:
**profScanRso**|Scan operators profiled time|Milliseconds|int64
**profXtScanRso**|External scan operators profiled time|Milliseconds|int64
**profProjRso**|Projection operators profiled time|Milliseconds|int64
**profSortRso**|Sort operators profiled time|Milliseconds|int64
**profFilterRso**|Filter operators profiled time|Milliseconds|int64
**profResRso**|Result operators profiled time|Milliseconds|int64
**profDmlRso**|DML operators profiled time|Milliseconds|int64
**profHjRso**|Hash-join operators profiled time|Milliseconds|int64
**profBufRso**|Buffer operators profiled time|Milliseconds|int64
**profFlatRso**|Flatten operators profiled time|Milliseconds|int64
**profBloomRso**|Bloom filter operators profiled time|Milliseconds|int64
**profAggRso**|Aggregate operators profiled time|Milliseconds|int64
**profBandRso**|Band-join operators profiled time|Milliseconds|int64
**profPercentileRso**|Percentile operators profiled time|Milliseconds|int64
**profUdtfRso**|User defined table operators profiled time|Milliseconds|int64
**profOtherRso**|Other operators profiled time|Milliseconds|int64



