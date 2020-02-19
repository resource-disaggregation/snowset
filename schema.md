## Main dataset

_Note: Some of the columns in the dataset have missing values which are denoted by "\N" in the CSV version of the dataset_

Each row corresponds to one unique query with the columns representing various characteristics pertaining to that query. The queryId column contains a unique 64-bit identifier for each query. For descriptions of the various columns along with details like units and datatypes, please refer to the following table:

Column | Description | Units | Datatype
:------:|:-----:|:-----:|:-----:
**queryId**|Query identifier (anonymized). Uniquely identifies a given query. ||int64
**warehouseId**|Identifier of warehouse (anonymized) in which the query ran. Queries that ran in the same warehouse will have the same warehouseId||int64
**databaseId**|"Unique identifier of database that this query is associated with. (Note: missing values ""\N"" possible )"||string
**createdTime**|Timestamp at which query is created / enters the system||UTC Timestamp
**endTime**|Timestamp at which query is fully complete||UTC Timestamp
**durationTotal**|Total end-to-end duration of query|Milliseconds|int64
**durationExec**|Time spent for actual query execution (worker processes running)|Milliseconds|int64
**durationControlPlane**|Time spent in control plane operations|Milliseconds|int64
**durationCompiling**|Time spent for query compilation|Milliseconds|int64
**compilationTime**|""""||int64
**execTime**|Query compute execution duration.||int64
**scheduleTime**|Time spent for query to start executing after it entered the system||int64
**serverCount**|Number of servers used for query execution.||int32
**warehouseSize**|Size of warehouse in which this query is executing||int32
**perServerCores**|Maximum number of cores this query is allowed to use for compute||int32
**persistentReadBytesS3**|Persistent data bytes read from S3 (size post-compression)|Bytes|int64
**persistentReadRequestsS3**|Total number of Persistent data read requests to S3||int64
**persistentReadBytesCache**|Persistent data bytes read from cache (persistent data is opportunisitically cached in the eph. storage system) (size post-compression)|Bytes|int64
**persistentReadRequestsCache**|Total number of Persistent data read requests from cache (persistent data is opportunistically cached in the eph. storage system)||int64
**persistentWriteBytesCache**|Persistent data bytes written to cache (persistent data is opportunistically cached in the eph. storage system) (size post-compression)|Bytes|int64
**persistentWriteRequestsCache**|Total number of Persistent data write requests to cache (persistent data is opportunistically cached in the eph. storage system)||int64
**persistentWriteBytesS3**|Persistent data bytes written to S3 (size post-compression)|Bytes|int64
**persistentWriteRequestsS3**|Total number of Persistent data write requests to S3 (persistent data is opportunistically cached in the eph. storage system)||int64
**intDataWriteBytesLocalSSD**|Intermediate data bytes spilled to Local SSD (size post-compression)|Bytes|int64
**intDataWriteRequestsLocalSSD**|Total number of intermediate data write requests to Local SSD||int64
**intDataReadBytesLocalSSD**|Intermediate data bytes read from Local SSD (size post-compression)|Bytes|int64
**intDataReadRequestsLocalSSD**|Total number of intermediate data read requests from Local SSD||int64
**intDataWriteBytesS3**|Intermediate data bytes spilled to S3 (size post-compression)|Bytes|int64
**intDataWriteRequestsS3**|Total number of intermediate data write requests to S3||int64
**intDataReadBytesS3**|Intermediate data bytes read from Local SSD (size post-compression)|Bytes|int64
**intDataReadRequestsS3**|Total number of intermediate data read requests from S3||int64
**intDataWriteBytesUncompressed**|Intermediate data bytes spilled to Local SSD / S3 (size pre-compression)|Bytes|int64
**readBytesRemoteExternal**|?||int64
**readRequestsRemoteExternal**|?||int64
**intDataNetReceivedBytes**|Intermediate data exchange over network recveived bytes (size post-compression)|Bytes|int64
**intDataNetSentBytes**|Intermediate data exchange over network sent bytes (size post-compression)|Bytes|int64
**intDataNetSentRequests**|Intermediate data exchange total number of network requests||int64
**intDataNetSentBytesUncompressed**|Intermediate data exchange over network sent bytes (size pre-compression)|Bytes|int64
**producedRows**|Number of produced rows in output set.||int64
**returnedRows**|Number of rows returned by this job||int64
**fileStolenCount**|Total number of files stolen by worker nodes (work stealing).||int64
**remoteSeqScanFileOps**|?||
**localSeqScanFileOps**|Sequential scan file operations.||int64
**localWriteFileOps**|Local write file operations.||int64
**remoteSkipScanFileOps**|Number of times fdn files were scanned||int64
**remoteWriteFileOps**|?||
**filesCreated**|Number of files created by query.||int64
**scanAssignedBytes**|Should be equivalent to scanBytes|Bytes|int64
**scanAssignedFiles**|Total number of files to be scanned after pruning|Bytes|int64
**scanBytes**|Total number of bytes scanned from files|Bytes|int64
**scanFiles**|Same as scanAssignedFiles|Bytes|int64
**scanOriginalFiles**|Total number of files in the table before pruning||int64
**userCpuTime**|User CPU time summed across all worker processes|Microseconds|int64
**systemCpuTime**|Kernel CPU time summed across all worker processes|Microseconds|int64
**serverMemoryUsed**|Memory used (?)|Bytes|int64
**totalMemory**|Memory used (?)|Bytes|int64
**totalMemoryMax**|Memory used (?)|Bytes|int64
**workerProcessMemory**|Memory used (?)|Bytes|int64
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
