```
https://www.studyblue.com/notes/note/n/cdh-study-cards-for-ccah-exam/deck/14983883#flashcard/view/14983883
```

hadoop
```
1. To input a file into HDFS, the client application passes the data to the NameNode, which then divides the data into blocks and passes the blocks to the DataNodes (T/F)
   > false
2. What component (Master node) of HDFS is reponsible for maintaining the namespace of the distributed file system?
   > name node
3. What is the default file replication factor in HDFS?
   > 3
4. The NameNode maintains the namespace of the file system using which two sets of files?
   > fs image, edits
5. Which type of Hadoop cluster nodes provide resources for data processing?
   > Namenode
6. Which service manages cluster CPU and memory resources?
   > Yarn
7. How does a NameNode determine DataNode availability
   > Heartbeat
8. What construct is granted CPU and memory resources?
   > Container
9. What daemon/service is responsible for application fault tolerance?
   > Zookeeper
10. By default, the various tasks for a single aplication / job will run on the same node if sufficient memory and CPU resources are available on that node (T/F)
   > False
11. Using hadoop's default settings, how much data will you be able to store on your Hadoop cluster if it has 12 nodes wigh 4TB of raw disk space per node allocated to HDFS storage?
() Approximately 3TB
() Approximately 12TB
(*) Approximately 16TB
() Approximately 48TB

12. Which two describe functions of the Nodemanager in YARN?
[] Tracking heartbeats from the Node Managers -> resource manager
[] Running a scheduler to determine how resources are allocated -> resource manager
[*] Monitoring and reporting container status for map and reduce tasks
[] Archiving the job history information and meta-data
[] Negotiating cluster resouce containers from the scheduler, tracking container status, and monitoring job progress -> application master
https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html
[*] Monitoring the status of the Application Master container and restarting on failure

13. You decide to create a cluster which runs HDFS in High Availability mode with automatic failover, using Quorum-based Storage. Which service keeps track of which nameNode is active at any given moment?
1) Zookeeper (*)
2) Standby Namenode
3) Secondary Namenode
4) Quorum Journal manager
5) YARN ResourceManager
6) Individual JournalNode daemons

14. Identify the function performed by a secondary namenode daemon configured to run with a single Namenode
() It acts as a standby namenode, providing a high availability profile for clients.
() It performs realtime backups of the NameNode.
(*) It combines the fsimage and edits files produced by the NameNode
() It provides an alterante HDFS endpoint when the NameNode is too busy.

15. You have a cluster running 32 slave nodes and three master nodes, running MapReduce v1 (MRv1). You execute the command:
$hdfs fsck / (please execute this command in your terminal!!!)
which two of the following pieces of information will be returned to you?
[] the location of each block in the cluster
[*] the number of Datanodes in the cluster
[] the number of nodes in each rack in the cluster
[*] the number of under-replicated blocks in the cluster
[] A list of all the files in the cluster

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

Q. resource manager
A.   
1) tracking heartbeats from the nodemanagers
2) running a scheduler to determine how resources are allocated

Q. The impala metastore requires an underlying sql database
A. T (Impala keeps its table definitions in a traditional MYSQL or PostgreSQL database known as the metastore, the same database where Hive keeps this type of data.

Q. Which two daemons typically run on each master node in hadoop cluster yarn
A. RM, NodeManager

Q. The resourcemanager will mark the map task attempt as failed and ask the nodemanager to terminate the container for the map task.
A. X -> Application manager 라고 한다.
 reduce fail -> application master 가 check하고 node manager가 중지시킴

```

spark

```
1. What is RDD

2. What is spark context

3. What are the 2types of operations in spark?
> transformation : 기존 rdd 데이터를 변경하여 새로운 rdd를 생성해내는 operation으로 filter, map 등이 있음
> action : rdd값을 기반으로 계산을 하여 결과를 생성해 내는 것으로 count() 같은 operation이 있음

4. what is lazy evaluation and how is it related to the 2 types of operations in question 3
> transformation에 해당하는 operaion이 수행될 때에는 실제 메모리에 로딩되지 않다가,
action operation이 rdd생성 요청을 하는 시점에 데이터 로딩 및 변환이 실제 수행된다.

5. what are the 3 ways that an rdd can be created?
> a file of set of files
from data in memory
from another rdd

6. what is key-value pair rdd

7. what are 3 things you need to do when creating a spark app?
> import (from pyspark inmport spark context ..)
spark context 생성 (sc=sparkcontext())
sc.stop()

8. what is set configuration parameters for a spark app
> sparkconf

9. what is stage in spark?
> spark실행 계획의 물리적 단위다.
동일 파니션 내에 수행되는 작업은 동일 stage내에서 실행되고, 스테이지 내의 작업들은 파이프 라인으로 연결되어 있음.
파티션이 달라지는 셔플 작업이 발생하면 repartition이 발생하고 stage가 달라짐

10. waht is rdd persistence? when we use?
> persist는 rdd를 저장하는 방법으로 persist()메소드를 통해 중간 결과를 저장함으로써 계산 오버헤드 줄임

11. compare micro-batch vs event driven in streaming process
event driven은 도착한 데이터에 응답하도록 설계되어 있다. 따라서 지속적으로 모니링하는 이벤트 중심 아키텍처를 구현해야 함.
반면 micro-batch는 새 데이터를 주기적으로 확인하고 다으 배치가 발생할때만 데이터를 처리한다.
12. what is the main advantage of using dataframe api compared to rdd core api
> data frame에 rdd보다 뛰어난 메모리와 쿼리 optimizer가 있기 때문에 데이터 프레임이 빠름

13. you are analyzing streaming data you want to count on a key value over the last 10 minutes of data, every 2 minutes, assuming that you are batching your data every 1 minutes
what function/method would you use?
> see = new StreamContext(new SparkConf(),60)
logs = ssc.socketTextStream(hostname,port)
reqcountsByWindow = logs.map(lambda line:(line.split(' ')[2],1).reduceByKeyAndWindow(lambda v1,v2:v1+v2,10*60,2*60)
14. what are data frame?

15. what are the services(daemon) utilized for hdfs ha mode?
> active name node, stand by name node, zookeeper, journal node

16. what is schema evolution in avro?
> backward : 새 스키마로 이전 데이터 읽음
forward : 스키마는 변하지 않고 새로운 스키마의 데이터가 들어오면 ignore
full : 필드 추가 시에는 default를 무조껀 넣어야 함

17. what is kudu?
> 일반 dbms처럼 primary key를 제공해 랜덤 엑세스 속도가 빠름
> 데이터라 컬럼 기준으로 저장돼있어 특정 컬럼만 읽을 때 성능 높일 수 있음

18. what is defference between srf and drf in pool management?
srf(single resource fairness) 는 메모리 리소스에 근간해 application 조정
drf(dominant ..)는 메모리와 cpu모두에 근간해 schedule함

19. issue command to save the name node metadata to disk
> hdfs dfsadmin -safemode enter
hdfs dfsadmin -saveNamespace
hdfs dfsadmin -safemode leave

20. what is hadoop client? issue a command to copy a file in your local linux directory to your home directory in hdfs. change the block size to 64MB in a chd installation
> export HADOOP_CLIENT_OPTS="-Xms64m -Xmx1024m"

```
