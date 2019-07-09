
```
ssh -i ./pem파일이름 계정명@ip
```
* putty 설정  
![puttyconf](https://user-images.githubusercontent.com/52270521/60864156-b4c55400-a25d-11e9-963a-f7f13d9265db.PNG)

## Pre-qualification

### Disable SELinux
https://www.lesstif.com/pages/viewpage.action?pageId=6979732
~~~
sudo vi /etc/sysconfig/selinux
SELINUX=enforcing 을 SELINUX=disabled 로 변경후 저장한다.

#selinux 활성화 여부 확인
getenforce 

#Root 계정으로 변경
sudo -i
reboot

# putty 재접속 이후 활성화 여부 확인하면 disabled 적용되어 있음
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

* vi /etc/hosts 파일 변경  
![2-1](https://user-images.githubusercontent.com/17976251/60864294-26050700-a25e-11e9-9d7d-a79b922005e2.JPG)

### Hostname modification for each node
~~~
# 각각의 node
sudo hostnamectl set-hostname <노드별명>
예) sudo hostnamectl set-hostname mn1

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
sudo status sshd.service
~~~

* PasswordAuthentication -> yes 로 변경  
![4-1](https://user-images.githubusercontent.com/17976251/60864724-5e591500-a25f-11e9-86b7-30877be269a1.JPG)


### Install dependencies using yum
uilt에만 설치하는 것 같지만 혹시 몰라서 5대 모두에 설치했음  
시간 오래걸림

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

* ntp 설치 성공  
![6-1-ntpinstall](https://user-images.githubusercontent.com/17976251/60867034-d413af80-a264-11e9-93dd-f2eab7744300.JPG)

* ntpd 실행 성공 확인  
![6-3](https://user-images.githubusercontent.com/17976251/60866988-bb0afe80-a264-11e9-8c17-25f28d7ff71d.JPG)


### Repository settings for CDH 5.15 installation (util node에서 진행)

#### config repositry CM

* util 서버에 cm 설치
* 참고 : https://www.cloudera.com/documentation/enterprise/5-15-x/topics/configure_cm_repo.html

```
[centos@util ~]$ sudo wget https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/cloudera-manager.repo -P /etc/yum.repos.d/
--2019-06-30 12:45:17--  https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/cloudera-manager.repo
Resolving archive.cloudera.com (archive.cloudera.com)... 151.101.72.167
Connecting to archive.cloudera.com (archive.cloudera.com)|151.101.72.167|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 290 [binary/octet-stream]
Saving to: ‘/etc/yum.repos.d/cloudera-manager.repo’

100%[======================================>] 290         --.-K/s   in 0s      

2019-06-30 12:45:17 (13.2 MB/s) - ‘/etc/yum.repos.d/cloudera-manager.repo’ saved [290/290]
```

#### baseurl 수정
```
[centos@util ~]$ sudo vi /etc/yum.repos.d/cloudera-manager.repo
baseurl=https://archive.cloudera.com/cm5/redhat/6/x86_64/cm/5.15.2/
```

#### rpm 에 key 추가
```
[centos@util ~]$ sudo rpm --import \
> https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/RPM-GPG-KEY-cloudera
```

### MariaDB Installation / DB Setting modification

### MySQL Connector / JDK file download for each node

### If the reason for each step is specified, additional points

## CM settings on Web UI

https://www.cloudera.com/documentation/enterprise/5-15-x/topics/cm_ig_host_allocations.html#host_role_assignments

### Select appropricate Hadoop Services (Core+Impala)

### Assign roles on correct nodes

### Screenshot of CM main page with green marks for all services

### Install sqoop service

## Data handling(Hive/Impala/Sqoop)

### Create user 'training' on HDFS and Linux for each node

### Make tables on MySQL using the data in all.zip file

### Import data for MySQL to HDFS using sqoop

### Create tables using Hive of Impala
