
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

# 각각의 node
sudo hostnamectl set-hostname <노드별명>
예) sudo hostnamectl set-hostname mn1

# 변경된 hostname 확인
hostname
~~~

* vi /etc/hosts 파일 변경
![2-1](https://user-images.githubusercontent.com/17976251/60864294-26050700-a25e-11e9-9d7d-a79b922005e2.JPG)

* 변경된 hostname 확인  
![2-2](https://user-images.githubusercontent.com/17976251/60864323-3ae19a80-a25e-11e9-8c66-72121db448a9.JPG)


### Hostname modification for each node

### sshd_config setting for each node

### Install dependencies using yum

### Ntp setting

### Repository settings for CDH 5.15 installation

### MariaDB Installation / DB Setting modification

### MySQL Connector / JDK file download for each node

### If the reason for each step is specified, additional points

## CM settings on Web UI

### Select appropricate Hadoop Services (Core+Impala)

### Assign roles on correct nodes

### Screenshot of CM main page with green marks for all services

### Install sqoop service

## Data handling(Hive/Impala/Sqoop)

### Create user 'training' on HDFS and Linux for each node

### Make tables on MySQL using the data in all.zip file

### Import data for MySQL to HDFS using sqoop

### Create tables using Hive of Impala
