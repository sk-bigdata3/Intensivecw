## Hadoop 이해와 활용

* BigData
```
하둡으로 처리할 데이터. Velocity, Variety, Volume
```
* Hadoop
```
데이터를 분산해서 저장하고, 분산된 데이터를 병렬처리 하는 것(플랫폼)
```
* HDFS 3종 SET
```
Namenode, Secondary Namenode, DataNode
```

- MasterNode
```
 Supervised and manage the work. 관리자역할. 한 클러스터에 두개이상. 자원관리. SinglePoint of Failure 대비
```
```
   - Namenode, Secondary Namenode, JobTracker 로 구성되어있음.
   - Namenode : SlaveNode에 일을 시킴. 파일 시스템, 메타데이터 관리.
   - JobTracker : 사용자 Application 관리. 하둡클러스터에 하나만 존재
```
```
   - Secondary Namenode : Stand-by 아님. 항상 메타데이터 최신본 유지. 장애발생시 Secondary 정보로 Namenode 복원
   - HA(High Availability)가 되면 Secondary Namenode는 없음. 대신 Stand-by Namenode 가 그 역할을 하게됨
   - Stand-by Namenode가 죽으면 fs image 업데이트 X.
   - 저널노드 : 에딧로그를 자신이 실행되는 서버의 로컬디스크에 저장
```
- Slave/Worker Node
```
do the actual work. 필요 시 다수의 Worker 확장 가능.
```
```
   - DataNode, TaskTracker 로 구성되어있음
   - DataNode : 블록의 백업 저장소. 실제 데이터를 가지고있으며 Namenode에 지속적 보고.
                하나가 깨져도 사용자는 계속 데이터 읽을 수 있음. 하나의 파일은 여러 노드에 블록으로 분리되어 보관 (3copy)
   - TaskTracker : 각 Slavenode에 할당된 작업 실행담당
```

- Namenode는 장애복구를 위해
```
   1) 메타데이터의 지속상태를 보완해주는 파일 백업
   2) Secondary Namenode운영. 주 Namenode 실패 시 사용될 네임스페이스 이미지복제본 유지
```
```
   - 메타데이터 이미지 : fs image(전체 Block 스냅샷 가지고있음. Namenode가 서비스), edits (변경 분 Transaction)
```

* Resource Management
```
 [cpu, 메모리] -> Container 지원
```
* Hadoop 1.0 <-> 2.0 차이 : Yarn 유무

* Yarn
```
  - HDFS 상단에서 빅데이터용 어플리케이션을 실행하는 대용량 분산 운영체제 역할
  - Resource Manager, JOb Histroy Server, NodeManagers
```
```
  - Yarn-MapReduce 차이점
     . 여러개를 동시에 돌릴 수 있음.
     . JobTracker 외에 AM이 Job을 관리하게 함
```
- Resource Manager
```
   - 클러스터당 하나. 클러스터 전체를 관리하는 마스터서버 역할. 프로그램 요청 처리.
     리소스매니저는 클러스터에서 발생한 작업을 관리하는 어플리케이션 매니저 내장. 자원(CPU, Memory) 사용 경쟁 조율
   - Node Manager로부터 Heartbeat를 받음
   - Run a Scheduler
```
- Node Manager
```
   - AM과 Container(CPU, Memory)로 구성. 노드당 하나씩 존재. SlaveNode의 자원을 모니터링하고 관리하는 역할
     RM의 지시를 받아 작업요구사항에 따라 Container 생성
  - 데이터 노드가 있으면 노드 매니저도 있어야함
  - NodeManager가 죽으면 AM이 Falut tolerance
```
- ApplicationMaster
```
   - 작업당 하나씩 생성. Container를 사용해 모니터링과 실행 관리.
     RM과 작업에 대한 자원 요구사항 협상
```
```
참고 ) Container : CPU, Memory. 필요한 자원의 요청은 AM이 담당. 승인은 RM이 담당.
    - Fault Tolerant : 리소스가 죽어도 다른 노드에서 작업 계속할 수 있도록 함
```
* Zookeeper
```
   - 분산 병렬 서비스 조정자/ 분산시스템 구성 서비스/ 동기화 서비스 / 분산 시스템을 위한 Naming 등록
   - Hadoop Namenode 에 대한 장애 조치
      : Active node가 죽는 경우, Stand-by Node가 자동기능
   - 현재 Active Node가 누구인지 정함
   - HA 상태정보를 저장하는 장소. 어떤 서버가 Active인지 Stand-by인지 저장
```

* Flume(Source, Channel, Sink), Kafka
```
   - 외부 데이터 소스들로부터 데이터 통합. 여러 시스템 로그 수집에 적합.
```
* Sqoop
```
   - RDMS와 HDFS간 데이터 Import, Export . JDBC로 DB연결
```
* Data Analysis
```
  - Hive, Pig, Impala
```
* Workflow Managemnet : oozie
```
   - 여러 처리단계에 대한 워크플로우 수행
   - Job 사이의 의존 관계를 정의
```
* NoSQL

* Spark
```
   - In Memory Data Computing
```

* HDFS
```
   - Hadoop 플랫폼의 저장 시스템. 확장성, 결함허용
   - 로그분석에는 좋지만 update가 안되는 단점이 있음
   - 파일업로드시 64 or 128 MB Block 에 저장.
   - 장애대처를 위해 3대 이상의 서버를 둠
   - 네임노드는 데이터노드 메타데이터 정보 저장
   - 데이터 노드는 네임노드에 하트비트를 주기적으로 보냄
```

* MapReduce
```
   - 분산 병렬 처리 방식
   - Mapper 단계 : HDFS 블록으로 나눠 분산처리
   - Shuffle, Sort : Mapper의 중간결과 통합, 정렬해서 Reducer에 전달
   - Reducer : Mapper 중간 결과로 최종 결과 수행시켜 HDFS에 저장
```
## NoSQL 이해와 활용

* NoSQL
```
   - Transaction 에서 자유로움
   - 고정되지 않는 스키마. Join 이 없음. 인덱싱 없음
```
```
   - Key-Value 방식으로 저장
   - ColumeStore : Storage Inefficient . 데이터 압축 가능성 높으며 다 저장할 필요 없음(같은관계 데이터끼리 들어가기 때문)
     * 기존 RDBMS는 Raw 단위
```

* RDBMS
```
   - ACID (A : Atomicity  C : COnsistnecy  I : Isalation  D : Durability)
```

* CAP Theorem
```
   - DB의 특징 중 2가지를 가지는 것
   - NoSQL은 Partition Tolerance 는 포기할 수 없음. 필수항목
   - A / P or C / P
```
```
   - Consistency : 병렬이라는 가정
   - Availability : 동시작동으로 항상 그 값을 줄 수 있어야 함
   - Partition Tolerance   
```

* HBase
```
   - Single transaction 가능. row Colume. 스키파 Flexible
   - 인덱싱 가능. Delete 시 물리적 삭제 X. 메타데이터 Mark. OLTP 대행 안됨
   - 데이터 돌 때 하나의 Region Server 필요. 다수의 데이터 관리 가능
      * Region : Block 개념
```

* Cassandra
```
   - Elastic Scalability 가능.
   - High Availability and Fault Tolerance
   - Tunable COnsistency : High Available, Strong Consistency 를 Tuanble 가능
```

## Flume/Kafka

* Sqoop
```
   - Sqoop : Client-side application using MapReduce.
   - 하둡 API를 사용하며 JDBC Connector를 씀
   - RDB > HDFS, HDFS > RDB
```  

* Flume
```
   - 패킷같은 개념으로 스트리밍데이터를 처리함
   - Key-Value 형식
   - 소스에서 데이터 받아 채널에 던져주기 전에, 인터셉터(처리, 메타정보 끼워넣기)에서 연산가능
   - Flume Arch. 3가지 요소 : Source, Sink, Channel  > 이것들을 작동시키는건 Agent.
   - Source나 Channel 이 여러개 있을 수 있기 때문에 연결을 bind에서 넣어줘야 함
```
```
   - Channel
      . Memory Channel : 속도가 중요할 때 사용. RAM에 저장
      . JDBC Channel : Debug 가 까다로움
      . File, Kafka..
   - Source : command 의 결과치를 소스로 쓰는 것. 데이터를 잃어버릴 수도 있음.
   - Sink
      . sink processor 가 sink group 안에 logic을 짤 수 있음.
      . backoff : sink한테 보냈는데 ack가 없을때 다시 rollback해서 보내는 옵션
      . 로드밸런싱을 위해서 / fault tolerance를 위해서 다음 prilrty로 넘어갈 수 있도록 함

   - Contextual Routing
      . interceptor : 인터셉터에서 식별 프로세스를 거침
      . Channel Selector : 어느 채널로 갈지 결정
    * 에러 발생한 경우 번호를 알아내서 다른 채널로 가게 하고, 정상적인 경우 또 다른 채널로 가게 함
```

* Kafka
```
   - distributed commit log > Transaction system
   - 클러스터 시스템 따로 생성 가능
   - 장점
      . Scalable : 클러스터기 때문에 노드 추가 하면 scale out 가능
      . Fault Tolerant : 복사를 해놓기 떄문에 high Availability
      . Fast
      . Flexible : 데이터 만드는 producer들과 받는 consumer들을 분리 시킴. 완전 분리되어있기때문에 누가 메세지를 가져가는지도 알 수 없음
   - 단점
      . 코딩이 힘들고 모니터링이 어려움
```
```
   - topic생성 > producer에 메세지 전달 > consumer가 메세지 수신 > 클러스터에서 kafka 실행  

   - broker : 클러스터 뒤에서 일하는 노드
   - Topic : 메세지이며 파티션이 가능하기 때문에 순서가 있음. Key-value 형식. Leader - Follower 방식. 누구나 L/F가 될 수 있음
      * immutable : 분산 병렬 처리에서는 값이 변하면 안되기 때문에 불변
      * Lambda Arch. : 스트리밍/ 배치 조인하는 Arch.
   - Message : 읽었든, 읽지 않았든 메세지를 유지. stream 처럼 처리

   - Consumer
      . Consumer는 늘려서 받을 수도 있고, 줄여서 받을 수도 있음
      . 한 파티션 안에서는 순서 보존을 함
      . 한 파티션 안에 한 consumer가 존재. max는 파티션 개수
      . 파티션안에서는 순서가 유지되기때문에 N개 날라가도 N-1가 복구 가능함
```
## Hive/oozie

* Hive / Impala
```
   - Impala Deamon
      . 각 워커노드에서 수행
      . master node -> State Store (캐시 Update) / Catalog Server ( one per cluster)
      . Satestore가 캐시를 Update하여 Impala Daemon으로 보내줌. daemon은 coordi역할을 함
   - Impala 의 메타스토어는 기본 SQL DB가 필요함.
   - Hive와 Impala는 메타데이터를 공유함
   - Hive Service
      . 하둡 에코시스템 중에서 데이터를 모델링하고 프로세싱하는 데이터 웨어하우징용 솔루션
      . RDB의 데이터베이스, 테이블과 같은 형태로 HDFS에 저장된 데이터의 구조를 정의하는 방법을 제공
      . HiveQL 쿼리를 이용하여 데이터 처리
   - Hive Table : Hive 메타스토어에 저장된 스키마 / hdfs에 저장된 데이터
```
```
   - dfs.block.size : HDFS에 저장된 파일의 블록크기를 지정하는데 사용되는 속성
```
* oozie
```
   - Hadoop 내 job에 대해 workflow작성 및 scheduling을 위해 사용되는 서비스
```
* Sqoop
```
   - HDFS에서 관리되는 DB를 import 혹은 export하는 Java Class를 생성하는 tool
```

* 참고
```
   - Yarn의 MapReduce가 수행되는 Hadoop cluster의 각 마스터노드에서 실행되는 2가지 daemon
      . RM, Node Manager

   - replica에 대한 block 배치
      1) replica 1
         클라이언트가 클러스터 밖에 있는 경우 블록의 첫 복제는 임의 랙 과 노드를 선택하여 수행
         하지만 클라이언트가 클러스터내의 데이터 노드가 수행되는 노드에서 실행 된다면 로컬 노드에 저장
      2) replica 2
         replica1과 다른 rack에 쓰여짐
      3) replica 3
         replica 2와 같은 rack의 다른 노드에 쓰여짐
      4) replica 4
         추가적인 replica가 있을 경우 다른 rack에 replica 수행

   - Mapreduce가 작업을 수행하는 동안 노드의 Mapper가 멈추면, AM이 알아차리고 전송함
```

## Spark

* Spark Context
```
   - 모든 spark 프로그램은 spark context object가 필요

```
* RDD
```
   - 언제든지 복구 가능함
   - 분산된 데이터 셋
   - Spark의 가장 기초적인 Unit
   - serialization한 특성을 가지며, type이 섞여서 들어갈 수 있음
   - Pair RDD (key-value)/ Double RDD
```
```
   - create RDD
     . 데이터 소스에서 read
     . 메모리 안에서 만들 때
     . RDD A -> RDD B 변형 (DataFrame을 transformation)

    * RDD는 Immutable한 특성을 가짐. 원본의 RDD는 그대로 있고 아예 새로운 RDD가 나오는 것
```
```
   - RDD Opreation
      . Action : 즉시 수행. 모든 계산된 결과를 제공/저장
      . Transformations : Immutable. linegae에 기록. lazy operation. 변환을 통해 새로운 RDD생성
```
```
   -  Memory Persistence
     . in-memory 에 있는건 보장은 아님. 보장되려면 disk에 저장이 필요함
       메모리에 꽉찼거나 최근에 사용하지 않은걸 지우면 저장이 될수도, 안될수도 있기 때문
```
```
   - When and Where to Persist
      . Memory only / disk : disk traffic 이 발생하는 시간에 따라 memory or disk로 결정 (연산 시간때문)              
      . disk / Replication : Disk rdplication 은 다른 노드에서 읽어오기 때문에 network traffic이 발생하기 떄문에 고민
```
```
   - Checkpoint 하면 persist할 때 linegae를 없앰 (infinte linegae를 없앰)
```
* Pair RDD
```
   - key-value Pair
   - Key, Value는 어떤 type도 가능
   - pair rdd생성 시, Key를 뭘로 할건지 먼저 설정
   - 각 key 에 대해 병렬처리 하거나 re grouping 할 때 유용함
   - key를 가지는 데이터로 동작하는 함수를 고려해 tuple로 구성한 return
```
```
   - Pair RDD operation
      . countByKey > action
      . groupByKey > transformation
      . sortByKey > transformation
      . join
```

* Stage
```
   - 한 stage안에 repartitioning이 있었다가 shuffle전에 끝

   - How Spark Calculates Stage
      . Narrow dependencies : linear기 때문에 이전 것만 Dependency
         ex ) map, filter, union
      . Wide(or shuffle) Dependency : Dependency가 여러개. wide Dependency 기점으로 stage가 구분됨
         ex ) reduceByKey, join, groupByKey
```
* Spark Streaming
```
   - Scalability and efficient falut tolerance
      . Thread 2개 중 하나는 receiver 역할을 해야함. 여러개 복사해서 fault tlerance 함
   - Once and only once Processing
   - Integrate batch and real-time processing

```
```
   - on Kafka  
      . Direct 면 병렬 / 이미 union 되어 있음
```

* Spark Streaming Context
```
   - 소스 (DStream, RDD) 생성과 스트리밍 처리 시작, 종료 등을 수행
```
* DStream
```
   - 데이터를 끊어서 연속된 RDD로 만들어 처리
   - 데이터를 아주 짧은 주기로 처리 (ex: 1초마다 처리)
```
* Spark Application
```
   - create Spark Application
      . spark context 가 필요함. 먼저 정의 필요
      . 대용량 데이터 처리. python, scala, java등으로 작성
```
```
   - set configuration
      . spark.master
      . spark.app.name
      . spark.local.dir
      . spark.ui.port  : spark application UI가 수행되는 port
      . spark.executor.memory : executor 당 메모리 할당
      . spark.driver.memory : dirver 안에 spark context가 돌게 됨. 각 executor들의 memory size
```

## Big data 아키텍처

* Data format
```
   - Avro : 효율적인 데이터 serialization 프레임워크로 광범위한 hadoop ecoystem에서 지원하는 파일 유형. Flexible함
   - Parquet : Columnar 데이터 포맷. spark는 Parquet이 기본 포맷
```
* HDFS HA Mode
```
   - HA Mode 구성 : NN, SNN, Zookeeper, failover Controller, JN

   - HA Mode에서는 Shared fsimage, edits 가지고 있음. 특정 노드가 가지고 있는게 아님
     구성 시 location 어디로 할지 정해야 함(저널노드가 관리하는게 좋음)
   - Namenode 죽으면 failover안오니까 Zookeeper 가 다른 노드에 토큰을 던져줌
     (failover controller는 NN, SNN에 항상 떠있음)
   - NN는 Zookeeper에 heartbeat를 계속 보냄. Zookeeper는 항상 떠있음
   - fs image : 메타정보 저장해 놓은 것
   - edits : 마지막 저장 후 발생한 일을 edits가 가지고 있음
   - SNN가 fsimage와 edits 를 합쳐서 가지고있음. 이걸 다시 fsimage로 만들어놓음. new generation edits를 관리함   
```

* Kudu
```
   - Cloumnar Store.
   - sql엔진은 아닌데 spark나 impala와 바로 작업 가능.

   - High throughput / Low latency / CPU performance
```
* Yarn Scheduler - SRF / DRF diff
```
   - Single Resource Fairness (SRF)
      . memory resource 를 기반으로 하는 Schedule
      . fair 하기 때문에 더 없는 쪽으로 할당
   - Dominant Resource Fairness (DRF)
      . memory와 cpu 기반으로 하는 schedule
      . 각 memory와 각 cpu 대비 비율 계산 후 다음 pool 은 비율이 낮은 쪽으로 할당
```
```
    * minimum resource 의 경우
    min 설정된 쪽에 먼저 할당해주고 나머지에 fair 하게 할당
    다만, demand가 0인 경우 min만큼도 할당하지 않음.

    * pools with weight
    각 pool에 높이를 맞춰 줌
```
