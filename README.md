Install Jenkins cloudbee (Enterprise edition) In HA in AWS 
This Guide will help to install Jenkins cloud bee enterprise version in AWS Cloud.
#Pre-requisites:
1. AWS ec2 Instance (2No)
2. AWS EFS
3. AWS Application load Balancer
4. Inbound rules should be allowed in Security group.
Source 	Destination	Port	Protocol
AWS Ec2 Instance	EFS	2049	TCP
Computer/Laptop	AWS Ec2 Instance	22	SSH
Computer/Laptop	AWS Ec2 Instance	8080	http
Computer/Laptop	AWS Ec2 Instance	9000 Customize jnlp	tcp
  
 #Steps 
 ==========
1.	Create EFS Server.
Take DNS name of the EFS Server 
fs-******.efs.us-east-2.amazonaws.com:/ /var/lib/jenkins/jobs nfs defaults   0   0
2.	Launch AWS Ec2 in multiple AZs.
Login to server and run the below command
=======================================
hostname: jenkins01
fqdn: jenkins01
manage_etc_hosts: true
runcmd:
  - add-apt-repository ppa:webupd8team/java -y
  - echo 'deb http://nectar-downloads.cloudbees.com/nectar/debian binary/' >> /etc/apt/sources.list
  - echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections
  - echo debconf shared/accepted-oracle-license-v1-1 seen true | debconf-set-selections
  - wget -q -O - http://nectar-downloads.cloudbees.com/nectar/debian/cloudbees.com.key | sudo apt-key add -
  - apt-get update
  - apt-get install oracle-java8-installer nfs-common -y
Sudo mkdir /var/lib/Jenkins
Vi /etc/fstab and add the below command and save the file.
fs-******.efs.us-east-2.amazonaws.com:/ /var/lib/jenkins/jobs nfs defaults   0   0
mount -a
df -h to verify the mount path
restart the jenkins01 server
apt-get install jenkins -y
access the http://ip-address:8080 to web browser

Perform the above steps in Jenkins node 02.
3.	Create ALB
Create the Application load balancer and add Jenkins01 and jenkins02 instance as backend server.
Configure the helath-checks
/ha/health-check
