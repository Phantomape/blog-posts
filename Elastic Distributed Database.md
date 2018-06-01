---
title: Elastic Distributed Database
date: 2017-08-28 19:08:04
tags: 
- Distributed System
categories: Distributed System
---
This project aims to develop an elastic distributed database using MySQL replication to guarantee high availability and dynamic scalability and use it to serve as the backend for a multi-tier web application running TPC-W benchmark.
##	Configuration on single ec2
- Config on AWS and install necessary tools on ec2(master node). password for mysql is : TigerBit!2016(default)
```
sudo apt-get update
sudo apt-get install -y openjdk-7-jdk ant gcc links git make
sudo apt-get install mysql-server
sudo apt-get install tomcat7
```
on slave nodes and candidate node, only mysql and java needs to be installed. 

-	Clone repository
```
git clone https://github.com/jopereira/java-tpcw.git
git clone https://github.com/peterbittiger/elasticDB.git
```
-	Initialize Database
```
mysql -uroot -p < /home/ubuntu/elasticDB/tpcw/mysql.sql
```
-	Deployment
```
cd java-tpcw
patch -p1 < /home/ubuntu/elasticDB/tpcw/tpcw.patch	//	apply patch
ant dist	
sudo rm -rf /var/lib/tomcat7/webapps/tpcw*	//	not necessary for the first time
sudo cp /home/ubuntu/java-tpcw/dist/tpcw.war /var/lib/tomcat7/webapps	
ant gendb	//	populate database
sudo ant genimg	//	presentation tier
```
-	Log in the web page
```
http://<ip address>:8080/tpcw/TPCW_home_interaction
```

##	Configuration on multiple ec2
Use our own laptop as client emulator, which is respond for sending different request.

-	Set root password to: 0307
```
sudo su
passwd
```

-	Change config to allow access from outside
```
vi /etc/ssh/sshd_config
```
change $PermitRootLogin$ to yes and $PasswordAuthentication$ to yes and then restart ssh service.
```
service ssh restart
```
-	Copy key-pairs between machines
```
sudo su
cd ~
ssh-keygen -t rsa
ssh-copy-id root@<ip address>
```
my computer copy to 4 ec2 instances, master copy to 4 ec2 instances(including itself), slaves should copy to the other 3 ec2 instances. Copying key of $a$ to $c$ means that $c$ grants access to $a$

-	Clone repo
```
git clone https://github.com/peterbittiger/elasticDB.git
```
change the ip address in set_env.sh and run testConnection.sh to test communications

##	MySQL Replication Test
Run prepareMasterSlaves.sh and then log in to master node and run mysql
```
mysql -uroot -p
#pwd: TigerBit!2016
```
inside mysql enter the following commands:
```
mysql> use canvasjs_db;
mysql> select * from datapoints;
```

download related dependency for this project:
```
mvn dependency:resolve
mvn eclipse:eclipse
```
and then change ipaddress in tpcw.properties in our project:
```
writeQueue = <MASTER_IP>
readQueue = <MASTER_IP>,<SLAVE1_IP>,<SLAVE2_IP>
candidateQueur = <CANDIDATE_IP>
```


Before we run the project, we must re-run the prepareMasterSlaves.sh, which would take approximately 5 minutes. Next, run enableMonitors.sh, and we will see new tabs showing performance.