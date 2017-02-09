OS: Ubuntu 16.04 Desktop
Spark Version: 2.0.2

Oracle VM VirtualBox

## Install Spark on master

Prerequisites: Install Java and Scala on the Master node

### Install Java on master

```markdown
$ sudo apt-get install python-software-properties
$ sudo add-apt-repository ppa:webupd8team/java
$ sudo apt-get update
$ sudo apt-get install oracle-java8-installer
```

### Install Scala
$ sudo apt-get install scala

## Configure hosts file
To edit the host file, use the following commands,

$ sudo vim /etc/hosts

MASTER-IP master
SLAVE01-IP slave01
SLAVE02-IP slave02

Replace the MASTER-IP with the IP address of master and similarly for slaves. 
And, make sure you have the vim or you can use any editor.
(Note: Comment the localhost already present in the hosts file if it gives problem)

## Configure SSH

### Install OpenSSH
Install openssh using following command as,

$ sudo apt-get install openssh-server openssh-client

### Generate Key-Pairs
$ ssh-keygen -t rsa -P ""

### Configuring password-less SSH
Copying the content of ~/.ssh/id_rsa.pub file to authorized_user file of ~/.ssh folder in master as well as slave nodes.

$ ssh-copy-id ubuntu@master
$ ssh-copy-id ubuntu@slave01
$ ssh-copy-id ubuntu@slave02

Note: Make sure you have same username in all the machines. It will be easy to configure.

You can check whether password-less login is working or not by using following commands.

$ ssh master
$ ssh slave01
$ ssh slave02


## Install Spark 2.0.2 on Master and Slave (Worker) nodes

You can either download from http://spark.apache.org/downloads.html or can directly download using wget command.

$ wget http://d3kbcqa49mib13.cloudfront.net/spark-2.0.2-bin-hadoop2.7.tgz  

$ tar xvf spark-2.0.2-bin-hadoop2.7.tgz

### Configuration of .bashrc file
Add the following lines to configure scala and spark in .bashrc file.

#scala
export SCALA_HOME=/usr/local/src/scala/scala-2.10.4
export PATH=$SCALA_HOME/bin:$PATH

#Spark
export SPARK_HOME=/home/ubuntu/spark-2.0.2-bin-hadoop2.7
export PATH=$SPARK_HOME/bin:$PATH

Restart .bashrc configuration
To restart the `.bashrc` configuration:

$. ~/.bashrc 


## Configuring Spark configuration files:

### Configure spark-defaults.conf

$ cp spark-defaults.conf.template spark-defaults.conf

$ vim spark-defaults.conf

Edit spark-defaults.conf file with the following line

 spark.master                     spark://master-pc:7077

### Configure spark-env.sh
Add the following lines in spark-env.sh file as,

export JAVA_HOME=/usr/lib/jvm/java-8-oracle
export SPARK_WORKER_CORES=8
export SPARK_MASTER_HOST=MASTER-IP
export SPARK_LOCAL_IP=MASTER-IP


### Configure slaves file

Add the list of slaves in this file as,

slave01
slave02

Note: Name should be same as that added in the /etc/hosts file.

## Installation on Spark Worker nodes (Slaves)

Setup prequisities on all slaves as done in master nodes,

- Entries to hosts file
- Install scala and java

Copy the downloaded spark file to all the slaves

$ scp spark-2.0.2-bin-hadoop2.7.tgz slave01:~
$ scp spark-2.0.2-bin-hadoop2.7.tgz slave02:~

Now ssh into the slave01 and slave02 separately to unzip the spark file by using following command,

$ tar xvf spark-2.0.2-bin-hadoop2.7.tgz

Configuration is all set.


## Running Spark Clusters

### Start Spark master (Master node)
$ sbin/start-master.sh

### Start Spark slaves (on Slave nodes)
~/spark-2.0.2-bin-hadoop2.7/sbin$ ./start-slave.sh spark://master-pc:7077

### Check daemons on Master

$ jps
Master

### Check daemons on Slaves
$ jps
Worker

