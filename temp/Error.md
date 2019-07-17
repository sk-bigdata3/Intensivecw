hive/Oozie
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
