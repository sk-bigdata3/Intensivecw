
```
ssh -i ./pem파일이름 계정명@ip
```
* putty 설정  
![puttyconf](https://user-images.githubusercontent.com/52270521/60864156-b4c55400-a25d-11e9-963a-f7f13d9265db.PNG)

## Pre-qualification

### 해야 할 것 같은데 뭔지 모르겠어요 ~~ 
~~~
sudo sysctl vm.swappiness=1 sudo sh -c "echo 'vm.swappiness=1'>> /etc/sysctl.conf" 
sudo sh -c "echo never > /sys/kernel/mm/transparent_hugepage/defrag" sudo sh -c "echo never > /sys/kernel/mm/transparent_hugepage/enabled" sudo sh -c "echo 'echo never > /sys/kernel/mm/transparent_hugepage/defrag' >> /etc/rc.local" sudo sh -c "echo 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' >> /etc/rc.local" sudo cat /etc/rc.local 
sudo sed -i 's/^\(SELINUX\s*=\s*\).*$/\1disabled/' /etc/selinux/config getenforce 
sudo yum -y install nscd sudo systemctl enable nscd sudo systemctl start nscd sudo systemctl status nscd 
sudo yum -y install ntp sudo chkconfig ntpd on sudo systemctl enable ntpd sudo systemctl start ntpd 
~~~

### Disable SELinux [전체  Node]
https://www.lesstif.com/pages/viewpage.action?pageId=6979732
~~~
sudo vi /etc/sysconfig/selinux
#SELINUX=enforcing 을 SELINUX=disabled 로 변경후 저장한다.
SELINUX=disabled

#Root 계정으로 변경
sudo -i
reboot

# putty 재접속 이후 활성화 여부 확인하면 disabled 적용되어 있음
#selinux 활성화 여부 확인
getenforce
~~~
* selinux = disabled  
![1-1](https://user-images.githubusercontent.com/17976251/60863328-526b5400-a25b-11e9-9bf7-c387f7a222f0.JPG)

* disabled 확인  
![1-3](https://user-images.githubusercontent.com/17976251/60863344-6020d980-a25b-11e9-8b3d-45bddcf87055.JPG)


### /etc/hosts/setting
~~~
sudo vi /etc/hosts

# 아래와 같이 내용 추가 private ip <모든 node>
172.31.11.29 mn1.cdhcluster.com mn1
172.31.8.22 util1.cdhcluster.com util1
172.31.6.188 dn1.cdhcluster.com dn1
172.31.2.189 dn2.cdhcluster.com dn2
172.31.14.162 dn3.cdhcluster.com dn3
~~~

* version 2
~~~
10.146.0.2 node1.sk.com node1 
10.146.0.5 node2.sk.com node2
10.146.0.3 node3.sk.com node3
10.146.0.4 node4.sk.com node4
10.146.0.6 node5.sk.com node5
~~~

* vi /etc/hosts 파일 변경  
![2-1](https://user-images.githubusercontent.com/17976251/60864294-26050700-a25e-11e9-9d7d-a79b922005e2.JPG)

### Hostname modification for each node
~~~
# 각각의 node ( 노드 명은 약어 말고 full name 으로 해주는 게 좋음)
sudo hostnamectl set-hostname <노드명> 
예) sudo hostnamectl set-hostname node1.sk.com

# 변경된 hostname 확인
hostname
~~~

* 변경된 hostname 확인  
![2-2](https://user-images.githubusercontent.com/17976251/60864323-3ae19a80-a25e-11e9-8c66-72121db448a9.JPG)

### sshd_config setting for each node

SSH를 사용하여 EC2 인스턴스에 로그인할 때 키 페어 대신에 암호 로그인을 활성화  
패스워드 인증 허용
~~~
sudo vi /etc/ssh/sshd_config
# PasswordAuthentication -> yes 로 변경 후 저장

# sshd 재시작 및 상태확인
sudo systemctl restart sshd.service
sudo systemctl status sshd.service [not found 인 경우도 있음]
~~~

* PasswordAuthentication -> yes 로 변경  
![4-1](https://user-images.githubusercontent.com/17976251/60864724-5e591500-a25f-11e9-86b7-30877be269a1.JPG)


### Install dependencies using yum - all node

~~~
sudo yum update
sudo yum install -y wget

# 전체 y 선택
~~~

* yum update/install wget 완료  
![5-1](https://user-images.githubusercontent.com/17976251/60865622-5e5a1480-a261-11e9-9c34-6d13b7d39dd1.JPG)


### Ntp setting
https://webdir.tistory.com/120?category=607029  

~~~
sudo yum install ntp

sudo vi /etc/ntp.conf
# 내용 변경 : 위에 네줄 주석처리, 아래 세줄 추가
#server 0.centos.pool.ntp.org
#server 1.centos.pool.ntp.org
#server 2.centos.pool.ntp.org
#server 3.centos.pool.ntp.org
server kr.pool.ntp.org
server time.bora.net
server time.kornet.net

위 주소가 안될 경우, 아래 주소로 추가해서 해볼 것
server 0.asia.pool.ntp.org
server 1.asia.pool.ntp.org
server 2.asia.pool.ntp.org
server 3.asia.pool.ntp.org
서버 주소는 http://www.pool.ntp.org/ 에서 확인 가능

sudo chkconfig ntpd on
sudo systemctl start ntpd
# 성공 확인
ntpq -p
~~~

* 간단한 방법. 아직 검증 안됨
~~~
[dltjsals71@node1 ~]$ sudo chkconfig ntpd on
Note: Forwarding request to 'systemctl enable ntpd.service'.
[dltjsals71@node1 ~]$ sudo systemctl enable ntpd
[dltjsals71@node1 ~]$ sudo systemctl start ntpd
[dltjsals71@node1 ~]$ ntpq -p
     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
*metadata.google 71.79.79.71      2 u    1   64    1    0.179   -0.196   0.115
[dltjsals71@node1 ~]$
~~~

* ntp 설치 성공  
![6-1-ntpinstall](https://user-images.githubusercontent.com/17976251/60867034-d413af80-a264-11e9-93dd-f2eab7744300.JPG)

* ntpd 실행 성공 확인  
![6-3](https://user-images.githubusercontent.com/17976251/60866988-bb0afe80-a264-11e9-8c17-25f28d7ff71d.JPG)


### Repository settings for CDH 5.15 installation (util node에서 진행)

#### config repositry CM (모든 node)

* util 서버에 cm 설치
* 참고 : https://www.cloudera.com/documentation/enterprise/5-15-x/topics/configure_cm_repo.html

```
[centos@util ~]$ sudo wget https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/cloudera-manager.repo -P /etc/yum.repos.d/
```

#### baseurl 수정 (모든 node)
```
[centos@util ~]$ sudo vi /etc/yum.repos.d/cloudera-manager.repo
baseurl=https://archive.cloudera.com/cm5/redhat/6/x86_64/cm/5.15.2/
```

#### rpm 에 key 추가
```
[centos@util ~]$ sudo rpm --import \
> https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/RPM-GPG-KEY-cloudera
```
~~~
# cloudera install
[centos@util ~]$ sudo yum install cloudera-manager-daemons cloudera-manager-server
~~~

* CM 설치 완료 확인  
![7-2-cm-success](https://user-images.githubusercontent.com/17976251/60868276-a2e8ae80-a267-11e9-97d1-1a0e048389b1.JPG)


### MariaDB Installation / DB Setting modification : util

* repository 먼저 확인
* yum 설정은 yum.conf 에서 하고있으며, yum.repos.d 에 있는 파일에 지정된 서버주소로부터 패키지들을 설치하고 관리할 수 있음
```
[centos@util ~]$ grep -i exclude /etc/yum.conf /etc/yum.repos.d/*
[centos@util ~]$ yum repolist all
```

### maria db 설치 : util
```
[centos@util ~]$ sudo yum install -y mariadb-server
```

### (추가) maria db setting
* 생략하면 클러스터 올렸을 때 노란불 뜬다.  
cloudera 설치 홈페이지에 있는 정보 복붙

~~~
sudo systemctl stop mariadb
sudo vi /etc/my.cnf

https://www.cloudera.com/documentation/enterprise/5-15-x/topics/install_cm_mariadb.html#install_cm_mariadb

-- 아래 내용 복붙

[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
transaction-isolation = READ-COMMITTED
# Disabling symbolic-links is recommended to prevent assorted security risks;
# to do so, uncomment this line:
symbolic-links = 0
# Settings user and group are ignored when systemd is used.
# If you need to run mysqld under a different user or group,
# customize your systemd unit file for mariadb according to the
# instructions in http://fedoraproject.org/wiki/Systemd

key_buffer = 16M
key_buffer_size = 32M
max_allowed_packet = 32M
thread_stack = 256K
thread_cache_size = 64
query_cache_limit = 8M
query_cache_size = 64M
query_cache_type = 1

max_connections = 550
#expire_logs_days = 10
#max_binlog_size = 100M

#log_bin should be on a disk with enough free space.
#Replace '/var/lib/mysql/mysql_binary_log' with an appropriate path for your
#system and chown the specified folder to the mysql user.
log_bin=/var/lib/mysql/mysql_binary_log

#In later versions of MariaDB, if you enable the binary log and do not set
#a server_id, MariaDB will not start. The server_id must be unique within
#the replicating group.
server_id=1

binlog_format = mixed

read_buffer_size = 2M
read_rnd_buffer_size = 16M
sort_buffer_size = 8M
join_buffer_size = 8M

# InnoDB settings
innodb_file_per_table = 1
innodb_flush_log_at_trx_commit  = 2
innodb_log_buffer_size = 64M
innodb_buffer_pool_size = 4G
innodb_thread_concurrency = 8
innodb_flush_method = O_DIRECT
innodb_log_file_size = 512M

[mysqld_safe]
log-error=/var/log/mariadb/mariadb.log
pid-file=/var/run/mariadb/mariadb.pid

#
# include all files from the config directory
#
!includedir /etc/my.cnf.d

-- 끝
~~~

### mariadb 내렸다가 재시작 : util
```
[centos@util ~]$ sudo systemctl enable mariadb
Created symlink from /etc/systemd/system/multi-user.target.wants/mariadb.service to /usr/lib/systemd/system/mariadb.service.
[centos@util ~]$ sudo systemctl start mariadb
# 잘 떴는지 확인
sudo systemctl statuc mariadb
```

### mariadb 권한 설정 : util
* 전체 Y 선택
```
- 참고 url : https://www.cloudera.com/documentation/enterprise/5-15-x/topics/cm_ig_installing_configuring_dbs.html
[centos@util ~]$ sudo /usr/bin/mysql_secure_installation
```
```
NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none):
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

You already have a root password set, so you can safely answer 'n'.

* password 설정

Change the root password? [Y/n] Y
New password:
Re-enter new password:
Password updated successfully!
Reloading privilege tables..
 ... Success!

By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] n
 ... skipping.

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

* 원격접속여부
Disallow root login remotely? [Y/n] Y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

* test db삭제여부
Remove test database and access to it? [Y/n] Y
 ... skipping.

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

* 지금까지설정한 권한정보 적용여부
Reload privilege tables now? [Y/n] Y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

* Maria DB 설치 완료  
![8-1-successmaria](https://user-images.githubusercontent.com/17976251/60867981-f6a6c800-a266-11e9-80dc-47493fc71dbd.JPG)


### MySQL Connector / JDK file download for each node (중요!!!!!!!!!!!@@)

#### jdk 설치

* util node가 아닌 다른 node 에서 jdk package가 없다고 설치가 안되면  


* cloudera 5.15.x 가 아니라면 홈페이지에서 버전 확인 후 설치하는게 좋을듯.
```
[centos@util ~]$ sudo yum install oracle-j2sdk1.7
```

#### java version 확인
```
[centos@util ~]$ java -version
openjdk version "1.8.0_181"
OpenJDK Runtime Environment (build 1.8.0_181-b13)
OpenJDK 64-Bit Server VM (build 25.181-b13, mixed mode)
```

* -bash: java 경로잡기
https://keichee.tistory.com/11
~~~
vi ~/.bash_profile

# 아래 두줄 추가

export JAVA_HOME=/usr/java/jdk1.7.0_67-cloudera
# :$PATH가 뒤에 있어야 java -version 했을 때 정상적인 버전이 뜬다. (*)
export PATH=/usr/java/jdk1.7.0_67-cloudera/bin:$PATH

source ~/.bash_profile

java -version
~~~

#### Mysql connector 설치 : 모든 Node

### 모든 노드에 mysql-JDBC Connector install
```
[centos@util ~]$ sudo wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.47.tar.gz
[centos@util ~]$ tar zxvf mysql-connector-java-5.1.47.tar.gz
[centos@util ~]$ sudo mkdir -p /usr/share/java/
[centos@util ~]$ cd mysql-connector-java-5.1.47
[centos@util mysql-connector-java-5.1.47]$ sudo cp mysql-connector-java-5.1.47-bin.jar /usr/share/java/mysql-connector-java.jar
[centos@util mysql-connector-java-5.1.47]$ cd /usr/share/java/
[centos@util java]$ sudo yum install mysql-connector-java
```

### mysql version 확인 / login : util
version 확인은 util만 가능
```
[centos@util java]$ mysql --version
mysql  Ver 15.1 Distrib 5.5.60-MariaDB, for Linux (x86_64) using readline 5.1
[centos@util java]$ mysql -u root -p
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 19
Server version: 5.5.60-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]>


MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
3 rows in set (0.00 sec)

```
#### db, user 생성
~~~
CREATE DATABASE scm DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON scm.* TO 'scm-user'@'%' IDENTIFIED BY 'password';

CREATE DATABASE aman DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON aman.* TO 'aman-user'@'%' IDENTIFIED BY 'password';

CREATE DATABASE rman DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON rman.* TO 'rman-user'@'%' IDENTIFIED BY 'password';

CREATE DATABASE hue DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON hue.* TO 'hue-user'@'%' IDENTIFIED BY 'password';

CREATE DATABASE metastore DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON metastore.* TO 'metastore-user'@'%' IDENTIFIED BY 'password';

CREATE DATABASE sentry DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON sentry.* TO 'sentry-user'@'%' IDENTIFIED BY 'password';

CREATE DATABASE oozie DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON oozie.* TO 'oozie-user'@'%' IDENTIFIED BY 'password';

FLUSH PRIVILEGES;
~~~

* MariaDB에 db,user 생성 완료  
![9-1-successdb](https://user-images.githubusercontent.com/17976251/60871178-31abfa00-a26d-11e9-852f-30261f3769f7.JPG)

#### db 생성 확인

~~~
MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| aman               |
| hue                |
| metastore          |
| mysql              |
| oozie              |
| performance_schema |
| rman               |
| scm                |
| sentry             |
+--------------------+
10 rows in set (0.00 sec)
~~~

#### setup CM DB
```
# 명령어 날리는 현재 디렉터리 위치 상관없음
[root@util cloudera-scm-server]# sudo /usr/share/cmf/schema/scm_prepare_database.sh mysql scm scm-user password
```

#### server start, log로 서버 올라가는것 확인
```
[root@util cloudera-scm-server]# sudo systemctl start cloudera-scm-server
[root@util cloudera-scm-server]# sudo tail -f /var/log/cloudera-scm-server/cloudera-scm-server.log
```

## CM settings on Web UI

#### CM 초기화면 Login
```
<선택>
C:\Windows\System32\drivers\etc > hosts 파일 메모장에서 편집
15.164.82.177 cm.com cm <- 퍼블릭IP 로 도메인 추가
```
**(필수!)**
```
#모든 Node에 비밀번호 설정 (*중요*)
sudo passwd centos
```

```
http://<util의 public ip>:7180
admin / admin
- trial 선택
- 화면상에 Could not connect to host. 라고 보이는 경우 ssh server가 제대로 설치되었는지 재 확인
# sudo yum install openssh-server
# /sbin/service sshd status
# /sbin/service sshd start
# ssh localhost
```
* 서버 재시작 시
```
# service cloudera-scm-server restart
# service cloudera-scm-agent restart
# sudo systemctl start cloudera-scm-server
```

#### 클러스터 삭제 후 재설치 시
* 에러 화면  
![위기2](https://user-images.githubusercontent.com/17976251/60882051-054ea880-a282-11e9-8266-7f38e33303dd.JPG)

1) 클러스터 삭제  
2) 호스트 > 모든 호스트 > 전체 삭제  
3) 클러스터 설치 진행 중에 폴더명 변경 (예: nn -> nn1)  
폴더명을 바꿔주지 않으면 중복되어 에러 발생!

### Select appropricate Hadoop Services (Core+Impala)
1.  
![10-1](https://user-images.githubusercontent.com/17976251/60882157-40e97280-a282-11e9-9fc0-acda6192153f.JPG)

2.  
```
 노드 검색 시, full name으로 검색 하는 것을 권장함.
 (약어로 사용할 경우 클러스터 올린 후에 문제가 생길 수 있음)
```
![10-2](https://user-images.githubusercontent.com/17976251/60882163-42b33600-a282-11e9-9369-ce137f775d61.JPG)  

3.변경 없음  
![10-3-변경없음](https://user-images.githubusercontent.com/17976251/60882170-447cf980-a282-11e9-8a66-b464bd71d21d.JPG)  

4.jdk 설치 체크 x  
![10-3-jdk설치안함](https://user-images.githubusercontent.com/17976251/60882176-4a72da80-a282-11e9-96f6-3acb41ffec6d.JPG)

5.single mode 체크 x
![10-5-singlemode안함](https://user-images.githubusercontent.com/17976251/60882271-755d2e80-a282-11e9-8a6e-88f2d0d60aa8.JPG)

6.id/pw 설정
* 모든 노드들에 비밀번호가 공통으로 설정되어 있어야 함.
![10-6](https://user-images.githubusercontent.com/17976251/60882276-7726f200-a282-11e9-8ff7-223990e084a5.JPG)

7.
![10-7-올라감](https://user-images.githubusercontent.com/17976251/60882360-ab9aae00-a282-11e9-853e-aa598a5e58e0.JPG)  
![10-8](https://user-images.githubusercontent.com/17976251/60882365-ad647180-a282-11e9-8e2b-c334482e2726.JPG)  
![10-9-버전확인](https://user-images.githubusercontent.com/17976251/60882374-b2c1bc00-a282-11e9-9492-00a665ca2e1e.JPG)  

8.Core 3개 설치  
![10-10-초기3개설치](https://user-images.githubusercontent.com/17976251/60882404-c79e4f80-a282-11e9-9152-46eaa0aefa35.JPG)

### Assign roles on correct nodes  
https://www.cloudera.com/documentation/enterprise/5-15-x/topics/cm_ig_host_allocations.html#host_role_assignments
```
1) Cloudera Management Service 관련 서비스
   모두 util01
2) oozie
   util01
3) yarn
   RM -> master
   JobHistory -> data01
   Nodemanager -> DataNode 3대
4) zookeeper
   data01,data02,master
5) hive
   gateway -> 5대 전부
   metastore -> util
   hiveServer2 -> util
6) hue
   전부 util01
7) hdfs
   datanode -> data01,02,03
   namenode -> master
   secondary namenode -> util01
   
- 기본 서비스 설치 처음에는 HDFS, YARN, ZOOKEEPER 만 설치
- 추가 서비스 설치 (HUE, OOZIE, HIVE, SQOOP / FLUME, IMPALA, SPARK ,KAFKA )
```
![10-11-클러스터설정](https://user-images.githubusercontent.com/17976251/60882134-329b5680-a282-11e9-832d-00e1b7477a83.JPG)



9.클러스터 설정
![10-11-클러스터설정(2)](https://user-images.githubusercontent.com/17976251/60882409-c9681300-a282-11e9-922b-3e710aa7d653.JPG)  
![10-12-변경없음](https://user-images.githubusercontent.com/17976251/60882410-ca00a980-a282-11e9-9ebe-7c2f726f44e1.JPG)  
![10-13-완료](https://user-images.githubusercontent.com/17976251/60882417-ca994000-a282-11e9-823f-7f98daf04c25.JPG)



### Screenshot of CM main page with green marks for all services

![끝](https://user-images.githubusercontent.com/17976251/60881899-b0129700-a281-11e9-8aba-faf10e7a184f.JPG)

### Install sqoop service

Cluster -> 서비스 추가 -> Sqoop2 : util(Node1) 에 설치

## Data handling(Hive/Impala/Sqoop)

### Create user 'training' on HDFS and Linux for each node
```
# 모든 host에 아래 계정 생성
cat /etc/passwd | grep training
sudo useradd training
sudo passwd training
sudo usermod -aG wheel training

# 계정 그룹 설정 확인
getent group wheel

# training 계정으로 접속
su training 
```

* 2번째 방법 - 모든 Node
~~~ 
# 계정 생성
sudo groupadd skcc
cat /etc/passwd | grep training
sudo useradd training
sudo passwd training
sudo usermod -aG skcc training

# 계정 수도권한추가
sudo usermod -aG wheel training

cat /etc/passwd | grep training
cat /etc/group  | skcc

# getent 확인

$ getent passwd training
training:x:1001:1002::/home/training:/bin/bash
$getent group skcc
skcc:x:1001:training
$ getent group wheel
wheel:x:10:centos,training
~~~
### Make tables on MySQL using the data in all.zip file
~~~ 참고용
# all.zip이 있는 윈도우 경로에서 아래 실행 -> 결과 값 호스트 홈 디렉토리에 업로드
scp -i ./skcc.pem all.zip training@<util 외부 ip>:.
$ scp -i ~/.ssh/gcp_rsa all.zip training@xx.xx.xx.xxx:/home/training

scp -i ./skcc.pem all.zip training@dn1:.

# unzip: util
sudo yum install -y unzip
unzip all.zip
~~~

~~~ 참고용
# trainig user 권한 추가 :util

mysql -u root -p 
GRANT ALL ON *.* TO 'training'@'%' IDENTIFIED BY 'training';
show grants for 'training'@'%';
~~~

~~~
# setup Script실행 :data01
cd /home/training/training_materials/devsh/scripts
 ./setup.sh
~~~

### [part1] Import data for MySQL to HDFS using sqoop & Create tables using Hive of Impala

* sql 파일 util 서버로 이동
~~~
$ scp -i ~/.ssh/gcp_rsa posts23-04-2019-02-44.sql.zip training@<util의 외부ip>:.

$ scp -i ~/.ssh/gcp_rsa authors-23-04-2019-02-34-beta.sql.zip training@<util의 외부ip>:.
~~~

a. In MySQL, create a database and name it "test"

~~~
CREATE DATABASE test;
use test;
~~~

b.Create 2 table in the test databases : authors and posts. (mysql)

~~~
source /autors.sql
source /posts.sql
~~~
* check
~~~
select count(*) from posts;
select count(*) from authors;
~~~

c. Create and grant user "training" with password "training" full access to the test database.

~~~
create user 'training'@'%' identified by 'training';

grant all privileges on *.* to 'training'@'%';
~~~

#### [part1]Extract Tables authors and posts from the database and create Hive tables.

a. Use sqoop to import the data from authors and posts.

b. For both tables, you will import the data in tab delimited text format

c. The imported data should be saved in training's HDFS home directory
```
# training 계정으로 접속
su training
```
~~~
# sqoop 수행 (node 1 : util)
sqoop import \
--connect jdbc:mysql://node1.sk.com/test \
--username training \
--password training \
--table posts \
--fields-terminated-by "\t" \
--target-dir /user/training/posts
~~~
~~~
sqoop import \
--connect jdbc:mysql://node1.sk.com/test \
--username training \
--password training \
--table authors \
--fields-terminated-by "\t" \
--target-dir /user/training/authors
~~~

d. In Hive, create 2 tables:authors and posts. The will contain the data that you imported from Sqoop in above step.

e. You are free to use whatever database in Hive.

f. Create authors as and external table.
```
  # hue 에서 쿼리 수행
```
```
CREATE EXTERNAL TABLE authors (
id int,
first_name string,
last_name string,
email string,
birthdate date,
added timestamp
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY '\t'
LOCATION "/user/training/authors/"
```

g. Create posts as a managed table.
```
  # hue 에서 쿼리 수행
```
~~~
CREATE TABLE posts (
id int,
author_id int,
title string,
description string,
content string,
date date
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY '\t'
LOCATION "/user/training/posts/"
~~~

#### [part1]Create and rum a Hive / Impala query. From the query, generate the results dataset that you will use in the next step to export in MySQL.

a. Create query that counts the number of posts each author has created.

b. The output of the query should provied the following information

c. The output of the query should be saved in you HDFS home directory.
```
  # hue 에서 쿼리 수행
```
~~~
INSERT OVERWRITE DIRECTORY '/user/training/results'
ROW FORMAT DELIMITED
FIELDS TERMINATED BY '\t'
SELECT a.id AS ID
, a.first_name AS fname
, a.last_name AS lname
, count(b.title) AS num_posts
FROM authors A,
posts B
WHERE A.id = b.author_id
GROUP BY a.id, a.first_name, a.last_name;
~~~

#### [part1]Export the data from above query to MySQL

a. Create a MySQL table and name it "results"

b. The table should be created under the databse "test"

```
  # mysql 에서 수행
```
~~~
CREATE TABLE `results` (
`id` int,
`fname` varchar(500),
`lname` varchar(500),
`num_posts` int
)
~~~

c. Finally, export into MySql the results of your query
```
 # node 1 : util   node2 : master node
```
~~~
sqoop export \
--connect jdbc:mysql://node1.sk.com/test \
--username training \
--password training \
--table results \
--fields-terminated-by '\t' \
--export-dir hdfs://node2.sk.com:8020/user/training/results
~~~
