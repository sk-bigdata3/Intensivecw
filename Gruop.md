Group


```
ssh -i ./pem파일이름 계정명@ip
```

## Pre-qualification

### Disable SELinux
https://www.lesstif.com/pages/viewpage.action?pageId=6979732
~~~
sudo vi /etc/sysconfig/selinux
SELINUX=enforcing 을 SELINUX=disabled 로 변경후 저장한다.

#selinux 활성화 여부 확인
getenforce 

sudo -i
reboot

# putty 재접속 이후 활성화 여부 확인하면 disabled 적용되어 있음
~~~
* selinux = disabled  
![1-1](https://user-images.githubusercontent.com/17976251/60863328-526b5400-a25b-11e9-9bf7-c387f7a222f0.JPG)

* disabled 확인  
![1-3](https://user-images.githubusercontent.com/17976251/60863344-6020d980-a25b-11e9-8b3d-45bddcf87055.JPG)


### /etc/hosts/setting

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
