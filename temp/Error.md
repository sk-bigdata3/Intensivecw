hadoop
```
Q. input file to HDFS, client appl. pass the data to NN, which then divides the data into blocks and passes the blocks to the DN 
   > False
   
Q. default file replication factor in HDFS 
   > 3
   
Q. what component of HDFS maintaining the namespace
   > Namenode

Q. Namespace of the file system using two set
   > fs image, edits
   
Q. which type of hadoop cluster node for data processing
   > Namenode
   
Q. manage cluster CPU and memory resource
   > Yarn
   
Q. How does a NN determine DN availability
   > Heartbeat
   
Q. what construct CPU and memory
   > Container
  
Q. responsible for application fault tolerance
   > Zookeeper
   
Q. the various task for a single appl. will run on same node if sufficeint memory and CPU resource are available on that node
   > False
   
Q. how much data will you be able to store on cluster if it has 12 node with 4TB.. per node allocated..?
   > 16 TB
```
hive/Oozie
```
Q. impala metasotre requires underlying SQL DB
  > True

Q. block size of file store HDFS
  > dfs.block.size  

Q. impala daemon / hive service
  >
   
Q. Hive table consists of a schema stored in Hive [metastore] and
   data stored in [hdfs]
   
Q. 2 daemons run on mn in hadoop cluster running MapReduce v2 on Yarn
  > RM, NodeManager
  
Q. block placement other two replica
  >
 
Q. which command see the structure of relation
  >
```
```
Q.
identify the function did not work by stand-by namenode daemon configured to run with a active namenode
A. (?)
In addition to providing checkpointing functionality, the backup node maintains the current state of all the hdfs block metadata in memory, just like the namenode. In this sense, it maintains a real-time backup of the NameNode's state

- standby-namenode combines the fsimage and edits files produced by the namenode. (?)

Q. import a portion of a relational database every day as files into hdfs and generate java classes to interact with that imported data? 
A. Sqoop

Q. yarn : mapper hangs! what happen?
A. ApplicationMaster will eventually mark the map task attemp as failed and request the node manager to terminate the map task.

Q. resource manager - 정답 맞나요?
A.   
1) tracking heartbeats from the nodemanagers
2) running a scheduler to determine how resources are allocated

Q. The impala metastore requires an underlying sql database
A. T (Impala keeps its table definitions in a traditional MYSQL or PostgreSQL database known as the metastore, the same database where Hive keeps this type of data.

Q. Which two daemons typically run on each master node in hadoop cluster yarn

Q. The resourcemanager will mark the map task attempt as failed and ask the nodemanager to terminate the container for the map task.
A. X -> Application manager 라고 한다.
 reduce fail -> application master 가 check하고 node manager가 중지시킴?

```
