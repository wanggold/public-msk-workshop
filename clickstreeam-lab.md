# Clickstream Lab

[BACK TO MAIN](/README.md)

## Content

* [Lab Overview](#lab-overview) (3 mins)
* [Producer Overview](#producer-overview) (2 mins)
* [Run Producer](#run-producer) (10 mins)
* [Configure Amazon KDA for Java Application](#configure-amazon-kda-for-java-application) (15 mins)
* [Consume From Amazon MSK](#consume-from-amazon-msk) (5 mins)
* [Create Kibana Dashboard](#create-kibana-dashboard) (10 mins)

## Lab Overview

Follow [link to workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/mskkdaflinklab/overview)

## Producer Overview

Follow [link to workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/mskkdaflinklab/producer)

## Run Producer

Follow [link to workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/mskkdaflinklab/runproducer)

### Connect to a new Kafka client having public IP

* Download kaypair following the step in [**Connect to KafkaClientInstance**](./pre-introduction.md#login-to-event-aws-account)

* Open a terminal in your local machine (Mac/Window)

* Locate to download keypair, and run the following commands

```
chmod 400 ee-default-keypair.pem
eval `ssh-agent`
ssh-add -k ee-default-keypair.pem
```

* Copy the IP address from the outputs of stack

![stackOutput](./pics/Screen%20Shot%202023-03-16%20at%206.37.16%20PM.png)

* SSH to Kafka client by running

```
ssh -i ee-default-keypair-2.pem ec2-user@<KAFKA_CLIENT_PUBLIC_IP>
```

### Create topics

1. Find Apache ZooKeeper connection 
![zooConn](./pics/Screen%20Shot%202023-03-16%20at%206.52.38%20PM.png)

2. On Kafka client EC2 terminal, create variable 

```
export MYZK=<ZOOKEEPR_CONNECTION_STRING>
```

3. Create the ExampleTopic topic in the MSK Kafka cluster.

```
/home/ec2-user/kafka/bin/kafka-topics.sh --create --zookeeper $MYZK --replication-factor 3 --partitions 3 --topic ExampleTopic
```

4. Create the output topics in the MSK Kafka cluster where the Kinesis Data Analytics Apache Flink application would send the clickstream analytics to.

```
/home/ec2-user/kafka/bin/kafka-topics.sh --create --zookeeper $MYZK --replication-factor 3 --partitions 3 --topic Departments_Agg
```

```
/home/ec2-user/kafka/bin/kafka-topics.sh --create --zookeeper $MYZK --replication-factor 3 --partitions 3 --topic ClickEvents_UserId_Agg_Result
```

```
/home/ec2-user/kafka/bin/kafka-topics.sh --create --zookeeper $MYZK --replication-factor 3 --partitions 3 --topic User_Sessions_Aggregates_With_Order_Checkout

```

5. Run clickstream client following [link in workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/mskkdaflinklab/runproducer#:~:text=Run%20the%20KafkaClickstreamClient%2D1.0%2DSNAPSHOT.jar%20program.)


## Configure Amazon KDA for Java Application

Follow [link to workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/mskkdaflinklab/configurekdajava)

* Note: [Click on **Configure**](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/mskkdaflinklab/configurekdajava#:~:text=Application%20details.-,Click%20on%20Configure,-Scroll%20down%20to) like

![clickConfig](./pics/Screen%20Shot%202023-03-16%20at%207.00.06%20PM.png)

* Note: [Modify the Values as follows](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/mskkdaflinklab/configurekdajava#:~:text=Modify%20the%20Values%20as%20follows%3A) like

![changeParam](./pics/Screen%20Shot%202023-03-16%20at%207.02.30%20PM.png)

* Note: [Run the Amazon KDA Application.](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/mskkdaflinklab/configurekdajava#:~:text=Run%20the%20Amazon%20KDA%20Application.) like

![runWithoutSnapshot](./pics/Screen%20Shot%202023-03-16%20at%207.07.17%20PM.png)

* Note: [see the Application graph.](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/mskkdaflinklab/configurekdajava#:~:text=see%20the%20Application%20graph.) like 

![click1](./pics/Screen%20Shot%202023-03-16%20at%207.12.49%20PM.png)

![click2](./pics/Screen%20Shot%202023-03-16%20at%207.13.10%20PM.png)

![click3](./pics/Screen%20Shot%202023-03-16%20at%207.13.24%20PM.png)

## Consume From Amazon MSK

Follow [link to workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/mskkdaflinklab/consumefrommsk)

1. Open another terminal on your local machine

2. SSH to Kafka client

![ssh](./pics/Screen%20Shot%202023-03-16%20at%207.20.41%20PM.png)

3. Get boostrap server string

![boostrap](./pics/Screen%20Shot%202023-03-16%20at%207.18.18%20PM.png)

4. Create variable by running 
```
export MYBROKERS=<BOOTSTRAP_SERVER_STRING>
```

5. Use the Apache Kafka console consumer to consume records from the 3 clickstream analytics Topics that you created earlier.

```
/home/ec2-user/kafka/bin/kafka-console-consumer.sh --bootstrap-server $MYBROKERS --topic Departments_Agg --from-beginning

```

6. Please Ctrl-C out of it and read the next 2 topics

7. Read next 2 topics

```
/home/ec2-user/kafka/bin/kafka-console-consumer.sh --bootstrap-server $MYBROKERS --topic ClickEvents_UserId_Agg_Result --from-beginning
```

```
/home/ec2-user/kafka/bin/kafka-console-consumer.sh --bootstrap-server $MYBROKERS --topic User_Sessions_Aggregates_With_Order_Checkout --from-beginning
```

## Create Kibana Dashboard

Follow [link to workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/mskkdaflinklab/createesdashboard)


* Note: [For Mac](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/mskkdaflinklab/createesdashboard#:~:text=local%20port%20forwarding.-,For%20Mac,-%3A)

![mac](./pics/Screen%20Shot%202023-03-16%20at%207.38.38%20PM.png)

[BACK TO MAIN](/README.md)






