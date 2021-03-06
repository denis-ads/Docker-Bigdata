FROM ubuntu:16.04
#**************************************** INSTALLLING java ******************************
RUN apt-get -y update
RUN apt-get -y upgrade 
RUN apt-get -y install openjdk-8-jdk
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64
ENV PATH $PATH:/usr/lib/jvm/java-8-openjdk-amd64/bin
RUN echo "export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64" >>~/.bash_profile
RUN echo "export PATH=$PATH:/usr/lib/jvm/java-8-openjdk-amd64/bin" >>~/.bash_profile

#**************************************** INSTALLING rsync vim sudo openssh-server ssh ******************************
RUN apt-get -y update
RUN apt-get -y upgrade 
RUN apt-get -y install rsync
RUN apt-get -y install vim sudo
RUN apt-get -y install openssh-server
RUN apt-get -y install ssh
RUN ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
RUN cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
RUN chmod 0600 ~/.ssh/authorized_keys
ADD ./config/ssh_config /etc/ssh/
EXPOSE 22
RUN echo "root:root" | chpasswd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

#**************************************** INSTALLING hadoop and configure ******************************
RUN mkdir hadoop
RUN wget -P /hadoop/ https://archive.apache.org/dist/hadoop/core/hadoop-2.7.7/hadoop-2.7.7.tar.gz
RUN tar -xzvf /hadoop/hadoop-2.7.7.tar.gz -C /hadoop
ADD ./config/core-site.xml ./hadoop/hadoop-2.7.7/etc/hadoop
ADD ./config/hdfs-site.xml ./hadoop/hadoop-2.7.7/etc/hadoop
ADD ./config/hadoop-env.sh ./hadoop/hadoop-2.7.7/etc/hadoop
ADD ./config/mapred-site.xml ./hadoop/hadoop-2.7.7/etc/hadoop
ADD ./config/yarn-site.xml ./hadoop/hadoop-2.7.7/etc/hadoop
ENV HADOOP_HOME /hadoop/hadoop-2.7.7
ENV PATH $PATH:$HADOOP_HOME/bin
RUN echo "export HADOOP_HOME=/hadoop/hadoop-2.7.7" >>~/.bash_profile
RUN echo "export PATH=$PATH:$HADOOP_HOME/bin" >>~/.bash_profile
RUN $HADOOP_HOME/bin/hdfs namenode -format

#**************************************** INSTALLING hive and configure ******************************
RUN mkdir hive
RUN wget -P /hive/ https://archive.apache.org/dist/hive/hive-1.2.1/apache-hive-1.2.1-bin.tar.gz
RUN tar -xzvf /hive/apache-hive-1.2.1-bin.tar.gz -C /hive
ADD ./config/hive-site.xml ./hive/apache-hive-1.2.1-bin/conf
ADD ./config/mssql-jdbc-7.0.0.jre8.jar ./hive/apache-hive-1.2.1-bin/lib
ADD ./config/hive-schema-1.2.0.mssql.sql ./hive/apache-hive-1.2.1-bin/scripts/metastore/upgrade/mssql/
ENV HIVE_HOME /hive/apache-hive-1.2.1-bin
ENV PATH $PATH:$HIVE_HOME/bin
RUN echo "export HIVE_HOME=/hive/apache-hive-1.2.1-bin" >>~/.bash_profile
RUN echo "export PATH=$PATH:$HIVE_HOME/bin" >>~/.bash_profile

#**************************************** INSTALLING spark and configure ******************************
RUN apt-get -y install scala
RUN mkdir spark
RUN wget -P /spark/ http://mirrors.estointernet.in/apache/spark/spark-2.4.3/spark-2.4.3-bin-hadoop2.7.tgz
RUN tar -xzvf /spark/spark-2.4.3-bin-hadoop2.7.tgz -C /spark
ADD ./config/hive-site.xml ./spark/spark-2.4.3-bin-hadoop2.7/conf
ADD ./config/core-site.xml ./spark/spark-2.4.3-bin-hadoop2.7/conf
ADD ./config/hdfs-site.xml ./spark/spark-2.4.3-bin-hadoop2.7/conf
ENV SPARK_HOME /spark/spark-2.4.3-bin-hadoop2.7
ENV PATH $PATH:$SPARK_HOME/bin
RUN echo "export SPARK_HOME=/spark/spark-2.4.3-bin-hadoop2.7" >>~/.bash_profile
RUN echo "export PATH=$PATH:$SPARK_HOME/bin" >>~/.bash_profile
RUN apt-get -y install python2.7 python-pip

#**************************************** INSTALLING sqoop and configure ******************************
RUN mkdir sqoop
RUN wget -P /sqoop/ https://www.apache.org/dist/sqoop/1.4.7/sqoop-1.4.7.bin__hadoop-2.6.0.tar.gz
RUN tar -xzvf /sqoop/sqoop-1.4.7.bin__hadoop-2.6.0.tar.gz -C /sqoop
ADD ./config/mssql-jdbc-7.0.0.jre8.jar ./sqoop/sqoop-1.4.7.bin__hadoop-2.6.0/lib
ENV SQOOP_HOME /sqoop/sqoop-1.4.7.bin__hadoop-2.6.0
ENV PATH $PATH:$SQOOP_HOME/bin
RUN echo "export SQOOP_HOME=/sqoop/sqoop-1.4.7.bin__hadoop-2.6.0" >>~/.bash_profile
RUN echo "export PATH=$PATH:$SQOOP_HOME/bin" >>~/.bash_profile

#**************************************** INSTALLING NiFi and configure ******************************
RUN mkdir nifi
RUN wget -P /nifi http://mirrors.estointernet.in/apache/nifi/1.9.2/nifi-1.9.2-bin.tar.gz
RUN tar -xzvf /nifi/nifi-1.9.2-bin.tar.gz -C /nifi
RUN /nifi/nifi-1.9.2/bin/nifi.sh install

#**************************************** INSTALLING Kafka and configure ******************************
RUN mkdir kafka
RUN wget -P /kafka http://apache.cs.utah.edu/kafka/2.2.0/kafka_2.11-2.2.0.tgz
RUN tar -xzvf /kafka/kafka_2.11-2.2.0.tgz -C /kafka
ENV KAFKA_HOME /kafka/kafka_2.11-2.2.0
ENV PATH $PATH:$KAFKA_HOME/bin
RUN echo "export KAFKA_HOME=/kafka/kafka_2.11-2.2.0" >>~/.bash_profile
RUN echo "export PATH=$PATH:$KAFKA_HOME/bin" >>~/.bash_profile

#**************************************** INSTALLING elastic search ******************************
RUN useradd -m  -s /bin/bash elastic
RUN echo "elastic:elastic" | chpasswd
RUN usermod -aG sudo elastic
RUN sudo -u elastic wget -P /home/elastic/ https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.1.1-linux-x86_64.tar.gz
RUN sudo -u elastic tar -xzvf /home/elastic/elasticsearch-7.1.1-linux-x86_64.tar.gz -C /home/elastic/
ENV ELASTIC_SEARCH_HOME /home/elastic/elasticsearch-7.1.1
ENV PATH $PATH:$ELASTIC_SEARCH_HOME/bin
RUN echo "export ELASTIC_SEARCH_HOME=/home/elastic/elasticsearch-7.1.1" >>~/.bash_profile
RUN echo "export PATH=$PATH:$ELASTIC_SEARCH_HOME/bin" >>~/.bash_profile

#********************************************* Moving ~/.bash_profile to /etc/profile.d/ to get env variables to all users ****************
RUN cp ~/.bash_profile /etc/profile.d/bash_profile.sh
#**************************************** load script to start/stop the ecosystem ******************************
ADD ./config/start-ecosystem.sh .
ADD ./config/stop-all.sh .
ADD ./config/stop-hadoop.sh .
ADD ./config/stop-hive.sh .
ADD ./config/stop-nifi.sh .
ADD ./config/stop-kafka.sh .
ADD ./config/start-all.sh .
ADD ./config/start-hadoop.sh .
ADD ./config/start-hive.sh .
ADD ./config/start-nifi.sh .
ADD ./config/start-kafka.sh .
