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
   > (*) node manager
6. Which service manages cluster CPU and memory resources?
   > Yarn
7. How does a NameNode determine DataNode availability
   > Heartbeat
8. What construct is granted CPU and memory resources?
   > Container
9. What daemon/service is responsible for application fault tolerance?
   > AM(Application Master)
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
[*] Archiving the job history information and meta-data
[] Negotiating cluster resouce containers from the scheduler, tracking container status, and monitoring job progress -> application master
https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html
[] Monitoring the status of the Application Master container and restarting on failure = ??

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
  > RM, name node, 저널노드, zookeeper
  
Q. block placement other two replica
   > (현실 답.. 4 -> 아마 rack2에 이미 있을꺼고, 1과3중 하나 노드 2개에 복사하는게 답)
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


```
1.
-- solution.sql (경로 : /home/training/problem1/solution.sql)
select a.id as id, 
       a.type as type,
       a.status as status,
       a.amount as amount,
       round(a.amount - w.average,2) as different
from problem1.account a,
     (select type, avg(amount) as average from problem1.account group by type) w 
where a.status='Active'
and a.type = w.type
;

hive -f solution.sql

A100000	Basic Checking	Active	44539	-7402.48
A100002	Basic Checking	Active	11483	-40458.48
...

2.
create database if not exists problem2;
use problem2;

create external table if not exists solution (
id int,
fname string,
lname string,
address string,
city string,
state string,
zip string,
birthday string,
hireday string
)
comment "employee"
row format delimited fields terminated by "\t"
stored as parquet location "/user/hive/warehouse/employee"

hive> dfs -cp /user/training/problem2/data/employee/*.parquet /user/hive/warehouse/employee/
hive> select * from solution;

10000	Rigel	Shaw	Ap #124-4664 Vulputate, Rd.	Cannole	OH	83380	07/12/80	05/30/18
10001	Chancellor	Bond	Ap #702-9298 Pretium Street	Piana degli Albanesi	AZ	38128	03/02/95	04/12/18
...

3.
use problem3;

create external table if not exists solution (
id int,
fname string,
lname string,
hphone string
)
comment "problem3"
row format delimited fields terminated by "\t"
stored as orc location "/user/training/problem3/solution"
;

insert into problem3.solution
select a.id
, a.fname
, a.lname
, a.hphone
from problem3.customer a
     inner join
     problem3.account b
     on a.id = b.custid
where 1=1
  and b.amount < 0
;

hive> select * from solution ;

10001	Sybil	Wiley	(504) 780-0366
10010	Brittany	Martinez	(341) 462-0222

4.

create database if not exists problem4;
use problem4;

create external table if not exists employee1 (
cust_id int,
fname string,
lname string,
adress string,
city string,
state string,
zip string
)
COMMENT "employee1"
ROW FORMAT DELIMITED FIELDS TERMINATED BY "\t"
STORED AS TEXTFILE LOCATION "/user/training/problem4/data/employee1/"
;

create external table if not exists employee2 (
cust_id int,
digit string,
fname string,
lname string,
adress string,
city string,
state string,
zip string
)
COMMENT "employee2"
ROW FORMAT DELIMITED FIELDS TERMINATED BY ","
STORED AS TEXTFILE LOCATION "/user/training/problem4/data/employee2/"
;

create external table if not exists solution (
cust_id int,
fname string,
lname string,
adress string,
city string,
state string,
zip string
)
COMMENT "problem4"
ROW FORMAT DELIMITED FIELDS TERMINATED BY "\t"
STORED AS TEXTFILE LOCATION "/user/training/problem4/solution/"
;

insert into solution
select *
from employee1
where 1=1
and state = 'CA'
union all
select cust_id
, fname
, lname
, adress
, city
, state
, zip
from employee2
where 1=1
and state = 'CA'
;

hive> select * from solution ;

10000063	Burton	Hayes	Ap #720-4012 Vivamus Avenue	San Diego	CA	96066-0000
10000068	Ria	Herman	2974 Cras St.	San Francisco	CA	95310-0000
..

5.
-- solution.sql (경로 : /home/training/problem5)

use problem5;

select concat_ws('\t',fname , lname, city, state)
from customer c
where c.city = 'Palo Alto'
and   c.state = 'CA'
union all
select concat_ws('\t',fname , lname, city, state)
from employee e
where e.city = 'Palo Alto'
and   e.state = 'CA'
;

hive -f ./solution.sql

Farrah	Preston	Palo Alto	CA
Brielle	Hudson	Palo Alto	CA
...

6.

use problem6 ;

create external table solution
(
  id int , 
  fname string,
  lname string, 
  address string,
  city string,
  state string,
  zip string,
  birthday string
 )
;

insert into table solution 
select id,
       fname,
       lname,
       address,
       city,
       state,
       zip,
       substr(birthday,1,5)
from employee;

hive> select * from solution limit 10;

10000000	Deanna	Lane	900-1514 Vitae, Rd.	Lafayette	LA	97827	08/31
10000001	Hall	Garrett	9656 Urna Avenue	Tucson	AZ	86511	08/24
...

7.
-- solution.sql (경로 : /home/training/problem7)

select concat(a.fname,' ' , a.lname)
from (
select e.*
from employee e
where e.city = 'Seattle'
order by fname,lname
) a
;

hive -f ./solution.sql

8.

create database if not exists problem8;
use problem8;

$ sqoop export \
 --table solution \
 --connect "jdbc:mysql://localhost/problem8" \
 --username cloudera \
 --password cloudera \
 --export-dir "/user/training/problem8/data/customer/" \
 --fields-terminated-by "\t" \
 --columns "id, fname , lname , address , city, state , zip , birthday" ;

$ mysql -u cloudera -p
Enter password: cloudera

mysql> show databases;
mysql> use problem8;
mysql> select * from solution limit 10;

9.
use problem9;

create external table solution 
( 
 id string,
 fname string,
 lname string,
 address string,
 city string,
 state string,
 zip string,
 birthday string
);

insert into solution
select distinct concat('A',id) as id,
       fname,
       lname,
       address,
       city,
       state,
       zip,
       birthday
from customer;

hive> select * from solution limit 10;

A1000000	Medge	Roach	P.O. Box 799, 6865 Nec Rd.	Racine	WI	56336	08/10/2016
A1000001	Nasim	Stone	P.O. Box 975, 759 Scelerisque Street	Tuscaloosa	AL	36696	08/16/2016
...

10.

use problem10;

create view solution as
select c.id as id , 
       c.fname as fname, 
       c.lname as lname,
       c.city as city,
       c.state as state,
       b.charge as charge, 
       substr(b.tstamp,0,10) as billdate
from customer c , 
     billing b
where c.id = b.id 
;

select * from solution limit 10;

1000000	Medge	Roach	Racine	WI	15.79	2017-03-05
1000001	Nasim	Stone	Tuscaloosa	AL	57.73	2016-09-05
...

11.
-- solution.sql(경로 : /home/training/problem11)

a)
use default;

select b.name 
    , count(*)
  from order_details a,
       products b
 where a.prod_id = b.prod_id
   and b.brand = 'Dualcore'
group by b.name
limit 3
;

1.5 TB SATA3 Disk	3956
16 GB Micro SD	3279
2 GB Micro SD	1979

b)
select to_date(o.order_date), 
       sum(p.price) as revenue , 
       sum(p.price - p.cost) as profit
  from orders o,
       order_details d,
       products p
 where o.order_id = d.order_id
   and d.prod_id  = p.prod_id
   and p.brand='Dualcore'
group by to_date(o.order_date);

2008-06-01	170742	14018
2008-06-02	270806	26356
...

c)
select o.order_id,
       sum(p.price) as total
  from orders o,
       order_details d,
       products p
 where o.order_id = d.order_id
   and d.prod_id  = p.prod_id
 group by o.order_id
 order by total desc
 limit 10;

5605465	940577
5997571	702157
...

hive -f solution.sql
```
