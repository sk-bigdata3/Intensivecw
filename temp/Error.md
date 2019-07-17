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
  > - Impala Deamon
      . 각 워커노드에서 수행
      . master node -> State Store (캐시 Update) / Catalog Server ( one per cluster)
      . Satestore가 캐시를 Update하여 Impala Daemon으로 보내줌. daemon은 coordi역할을 함
    - Impala 의 메타스토어는 기본 SQL DB가 필요함.
    - Hive와 Impala는 메타데이터를 공유함
    - Hive Service
      . 하둡 에코시스템 중에서 데이터를 모델링하고 프로세싱하는 데이터 웨어하우징용 솔루션
      . RDB의 데이터베이스, 테이블과 같은 형태로 HDFS에 저장된 데이터의 구조를 정의하는 방법을 제공
      . HiveQL 쿼리를 이용하여 데이터 처리
   
Q. Hive table consists of a schema stored in Hive [metastore] and
   data stored in [hdfs]
   
Q. 2 daemons run on mn in hadoop cluster running MapReduce v2 on Yarn
  > RM, NodeManager
  
Q. block placement other two replica
  >  - replica에 대한 block 배치
      1) replica 1
         클라이언트가 클러스터 밖에 있는 경우 블록의 첫 복제는 임의 랙 과 노드를 선택하여 수행
         하지만 클라이언트가 클러스터내의 데이터 노드가 수행되는 노드에서 실행 된다면 로컬 노드에 저장
      2) replica 2
         replica1과 다른 rack에 쓰여짐
      3) replica 3
         replica 2와 같은 rack의 다른 노드에 쓰여짐
      4) replica 4
         추가적인 replica가 있을 경우 다른 rack에 replica 수행
 
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
