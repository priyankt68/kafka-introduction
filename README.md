# kafka-introduction
These are notes from a webinar by Jankiram MSV organised by DigitalOcean.

# Introduction

  Kafka - High throughput pub-sub based messaging engine.

 * Enterprise messaging architecture. 
 * Data can come in from a variety of data sources.
 * Dealing with multiple consumers is extremely easy and configurable.
 * High reliable and fault tolerant - If one of the servers/kafka brokers responsible for forwarding incoming messages fail - there are multiple nodes which take care of it. 
 * Its messaging broker for heavy usage. 

# General Usecases

- Activity tracker - Click stream analysis 
- Metric collection - Gather metric from variety of servers
- Streaming Application - Kafka Streaming - This can become the gateway for IoT system.
- Enterprise Message Bus - exchanging messages - Kafka becomes the central piece which is sending messages around different services.

# Main components of Kafka Deployment.

 * <b>Source</b> - Anything which is generating data.

 * <b>Producers</b> - Responsible for acquiring data and ingesting into Kafka. This is required when source directly cannot install kafka client on it. Producers sends that data into the kafka system.

 * <b>Zookeeper</b> - Responsible for overall health of the kafka deployment. Co-ordinator for maintaining health of kafka deployment. Will take care of which brokers are up and which are not. Very important for the health of the overall system.

# Implementation Pattern

  Based on a popular pattern called <i>(Lambda Architecture)[http://lambda-architecture.net]</i> - Prominent pattern in IoT systems.

# Usecase - Connected Cars

Let's say set of connected cars are already on the road which are sending data parameters like - current acceleration, current velocity. There are other data parameters which are important to track in real time - availability of engine oil, temperature of engine and other health parameter, availability of fuel oil etc.

Let’s say a company is interested in determining 2 things :-
      
- Which cars are most fuel efficient and  wants to do route optimisation ?
- Which car needs immediate attention in terms of health ?

All of these cars’ data will eventually hit on to an Apache Kafka and then through Apache Kafka go to further 2 paths 
- Hot Path Analytics - From Kafka to Apache Storm - (Useful for “car which need immediate attention”)
- Cold Path Analytics  - From Kafka to Apache Hadoop for batch processing  - (Useful for “determining which car is most fuel efficient / how best to route optimisation”

<img src='https://github.com/priyankt68/kafka-introduction/blob/master/hot_path_cold_kafka.png' /> 
(image source)[https://thenewstack.io/apache-kafka-cornerstone-iot-data-platform/] 
￼
In such a usecase Kafka acts as a gateway for deciding on where which sort of data goes.

Kafka’s job is to efficiently broker the data. Every piece of data will be sent to the consumers efficiently without an overload on the system.

# Core concepts of Apache Kafka

* <b> Producers </b> - Responsible for taking messages from devices which are low powered and cannot be sent by low powered devices. Written in high level languages like Py, JS. Interacts and interfaces with Apache Kafka instances.

* <b> Consumers </b> - Written in high level languages like Py, JS . Receives data which is sent by Kafka instance. Each Kafka instance is nothing but a Kafka Broker. 

* <b> Messages </b> - Chunks of data points sent by Source/ Producers.

	 - Database Analogy : Think of each message as a row in a table of a database. 1 atomic unit or atomic transaction.

* <b> Topics </b> - Central hubs responsible for collecting and sending all the messages. These are contracts between producer and consumers.
  - Database Analogy : These are like tables of databases. Stores messages based on the retention policies 

* <b>Partitions </b>  - To handle scale, Kafka stores messages in different partitions. Each consumer reads these messages from one of the partitions. The method in which a message determines as to what partition to go in is evaluated from the key of the message produced by a producer. Consumer will pick the messages from one of these shards.

  - Database Analogy : These are like database shards. Data will be stored in one of the available partitions. These bring in durability. Every message will be stored in one of the partitions.

* <b> Offsets </b> - Bookmarks for consumers. Let’s say 10 messages are published on a topic. A consumer has received 5 messages and then the consumer goes offline. There is where offsets come in action. Kafka will track how many messages were received by the consumer and then will send remaining 5 messages.

 If they are configured in consumer, the when the consumer connects back, it will receive remaining 5 messages. 

* <b> Consumer Groups </b>  - Messages are coming into a topic - One consumer group is responsible storing data in raw format in database for Apache Storm. Other consumer group’s responsibility is to create aggregated metrics.

 For this you will create two consumer groups. One of let’s say hot path analytics and other one for cold path analytics.

 Every message will receive all the messages in both the consumer group and then consumers in each of these

 NOTE - I am not too clear about consumer groups yet.

* <b>Broker </b> - Every instance of Kafka is a broker. Broker is nothing but a JVM listening on a port. We can have multiple brokers on the same machines. These brokers are co-ordinated by ZooKeeper. Zookeeper manages their health and ensure that they are running properly

* <b> Clusters </b> - Collection of brokers is a cluster. This cluster of multiple brokers could infact exist on a single machine or several machines.


Some miscellaneous points
1. Kafka internally takes care that each message cannot be written to two partitions. 
2. Every Kafka client - Either point to installation of zookeeper or one of the bootstrap server (primary installation of kafka server in a cluster which will receive a message )
3. Producer is ignorant of the partitions in which messages will be stored.

Comparison with MQTT
MQTT used with low powered devices based on a pub/sub model whereas Kafka is a full blown pub/sub messaging engine . Details [here](https://thenewstack.io/apache-kafka-cornerstone-iot-data-platform/).






