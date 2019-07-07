2019-07-07 sansung.yoon 

AWS EC2
https://supdev.tistory.com/22
private key(ppk,pem)를 이용하여 ssh접근하기
puttygen 이용하여 pem->ppk 변환
auth > browse 에서 ppk 파일 선택
putty 에서 ssh 로 접속
* ec2 redhat 접속 계정 : ec2-user

Linux 인스턴스 유형은 기본 Linux 시스템 사용자 계정으로 시작됩니다. Amazon Linux의 경우 사용자 이름은 ec2-user입니다. RHEL의 경우 사용자 이름은 ec2-user 또는 root입니다. Ubuntu의 경우 사용자 이름은 ubuntu 또는 root입니다. Centos의 경우 사용자 이름은 centos입니다. Fedora의 경우 사용자 이름은 ec2-user입니다. SUSE의 경우 사용자 이름은 ec2-user 또는 root입니다. ec2-user 및 root를 사용할 수 없는 경우 AMI 공급자에게 문의하십시오.
