# kafka-python

In amazon linux 2 ec2:
-----------------------

wget https://archive.apache.org/dist/kafka/3.3.1/kafka_2.12-3.3.1.tgz
tar -xvf kafka_2.12-3.3.1.tgz

Installing Java:
-----------------------

java -version
sudo yum install java-1.8.0-openjdk
java -version
cd kafka_2.12-3.3.1


Start Zoo-keeper:
-------------------------------
bin/zookeeper-server-start.sh config/zookeeper.properties

Open another window to start kafka
But first ssh to to your ec2 machine as done above


Start Kafka-server: (we are specifically allocating some memory for the kafka server)
----------------------------------------
Duplicate the session & enter in a new console --
export KAFKA_HEAP_OPTS="-Xmx256M -Xms128M"  
cd kafka_2.12-3.3.1
bin/kafka-server-start.sh config/server.properties

PLAINTEXT://ip-172-31-21-73.ec2.internal:9092 we look for this for identifidying the DNS of our kafka server
![image](https://github.com/user-attachments/assets/efe8646d-230f-4f1c-b437-135bd83c552d)

It is pointing to private server , change server.properties so that it can run in public IP 

To do this , you can follow any of the 2 approaches shared belwo --
Do a "sudo nano config/server.properties" - change ADVERTISED_LISTENERS to public ip of the EC2 instance

![image](https://github.com/user-attachments/assets/d10dd91c-a163-4d40-b468-6c41e0e282b5)

![image](https://github.com/user-attachments/assets/85f65e77-3049-473d-a42c-cabe35268802)

I didn't have enough memory so i had to run

free -m

export KAFKA_HEAP_OPTS="-Xmx512M -Xms512M"

![image](https://github.com/user-attachments/assets/1dce7213-b242-44d3-8ef4-51a6c69bbbca)

Create the topic:
-----------------------------
Duplicate the session & enter in a new console --
cd kafka_2.12-3.3.1
bin/kafka-topics.sh --create --topic demo_test --bootstrap-server {Put the Public IP of your EC2 Instance:9092} --replication-factor 1 --partitions 1
bin/kafka-topics.sh --create --topic demo_test --bootstrap-server 54.235.0.205:9092 --replication-factor 1 --partitions 1

Start Producer:
--------------------------
bin/kafka-console-producer.sh --topic demo_testing2 --bootstrap-server {Put the Public IP of your EC2 Instance:9092} 
bin/kafka-console-producer.sh --topic demo_testing2 --bootstrap-server 54.235.0.205:9092 

Start Consumer:
-------------------------
Duplicate the session & enter in a new console --
cd kafka_2.12-3.3.1
bin/kafka-console-consumer.sh --topic demo_testing2 --bootstrap-server {Put the Public IP of your EC2 Instance:9092}
bin/kafka-console-consumer.sh --topic demo_testing2 --bootstrap-server 54.235.0.205:9092
