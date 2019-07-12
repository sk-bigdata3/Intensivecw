
* Visual Studio Code에서 Git 사용

https://demun.github.io/vscode-tutorial/git/


## test

~~~
gcloud compute instances create node1 --zone=asia-northeast1-a --custom-cpu=4 --custom-memory=12 --image-family=centos-7 --image-project=centos-cloud --boot-disk-size=100GB --metadata-from-file startup-script=startup.sh

gcloud compute instances create node2 node3 node4 node5 --zone=asia-northeast1-a --machine-type=n1-standard-1 --image-family=centos-7 --image-project=centos-cloud --boot-disk-size=50GB --metadata-from-file startup-script=startup.sh
~~~


* Git bash 사용해서 GCP에 ssh 연결

https://jybaek.tistory.com/611  
https://ruuci.tistory.com/6

~~~
ssh -i ~/.ssh/rsa-gcp-key dltjsals71@35.243.114.223
~~~
