# MSK Workshop Preparation

[BACK TO MAIN](/README.md)

## Content

* [Login to Event AWS account](#login-to-event-aws-account)
* [Connect to KafkaClientInstance](#connect-to-kafkaclientinstance)
* [Setup AWS CLI on KafkaClientInstance](#setup-aws-cli-on-kafkaclientinstance)
* [Setup Clickstream Lab](#setup-clickstream-lab)

## Login to Event AWS account

1. Go to <a href="https://dashboard.eventengine.run/login" target="_blank" rel="noopener">Event Login Page</a>. Enter **EVNET HASH** and click **Accept Terms & Login**

![LoginEvent](./pics/Screen%20Shot%202023-03-16%20at%2010.21.22%20AM.png)

2. Click **AWS Console**. Click **Open Console**. You will login to AWS console. 

| | |
| - | - |
|![OpenDashboard](./pics/Screen%20Shot%202023-03-16%20at%2010.48.30%20AM.png) | ![LoginConsole](./pics/Screen%20Shot%202023-03-16%20at%2010.49.31%20AM.png) |


## Connect to KafkaClientInstance

1. Navigate to the Event Engine page - https://dashboard.eventengine.run. Download the keypair for SSH connect to Kafka client EC2 instance

| | |
| - | - |
|![SSH-KeyPair](/pics/Screen%20Shot%202023-03-16%20at%209.23.05%20AM.png) | ![DownLoad](./pics/Screen%20Shot%202023-03-16%20at%2010.31.16%20AM.png)|


2. Open environment in Cloud9

![OpenCloud9](./pics/Screen%20Shot%202023-03-16%20at%209.28.30%20AM.png)

3. Upload downloaded keypair to Cloud9 environment

![UploadKeypair](./pics/Screen%20Shot%202023-03-16%20at%209.31.55%20AM.png)

4. Go to the **bash** pane at the bottom and type in the following commands to setup the ssh environment so that you can access the KafkaClientEC2Instance.

```
chmod 600 ee-default-keypair.pem
eval `ssh-agent`
ssh-add -k ee-default-keypair.pem
```

![SetupKeyPair](./pics/Screen%20Shot%202023-03-16%20at%209.35.24%20AM.png)

5. Find Kafka Client EC2 instance IP address. Copy the IP address.

![FindClientIP](./pics/Screen%20Shot%202023-03-16%20at%209.39.54%20AM.png)

6. In your bash pane, you will use standard ssh to connect to the IP address of your instance. For most labs, this will be your KafkaClientInstance. 

```
ssh -i ee-default-keypair.pem ec2-user@<INSTANCE-IP>
```

![SshToClient](./pics/Screen%20Shot%202023-03-16%20at%209.44.18%20AM.png)


## Setup AWS CLI on KafkaClientInstance

1. Navigate to the Event Engine page - https://dashboard.eventengine.run. Click on **AWS Console**

![EventAWSConsole](./pics/Screen%20Shot%202023-03-16%20at%209.51.35%20AM.png)

2. Get the **AWS_ACCESS_KEY_ID** and **AWS_SECRET_ACCESS_KEY** from either the Mac/Linux or Windows section.

![AccessKey](./pics/Screen%20Shot%202023-03-16%20at%209.53.47%20AM.png)

3. Connect to your KafkaClientInstance using SSH and the Keypair. Confugure [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html#cli-quick-configuration) with the Access Key ID and Secret Access Key you gathered above. 

```
aws configure

```
![AWS-Config](./pics/Screen%20Shot%202023-03-16%20at%209.57.17%20AM.png)

4. Test connectivity by running `aws kafka list-clusters` and you should see a list of clusters in your account


## Setup Variables on Kafka Client

1. Go to MSK console. Click **View client information**

![ClientInfo](./pics/Screen%20Shot%202023-03-16%20at%2012.37.12%20PM.png)

2. Copy **Plaintext** MSK endpoint string

![PlainTextEndpoint](./pics/Screen%20Shot%202023-03-16%20at%2012.39.12%20PM.png)

3. SSH to Kafka client EC2 [link](#connect-to-kafkaclientinstance). 

4. Run the following command by replacing *<PLAINTEXT_MSK_ENDPOINT_STRING>* with copied MSK endpoint string. ***PLEASE DO NOT CLOSE THE TERMINAL"***. 

```
export MYBROKERS="<PLAINTEXT_MSK_ENDPOINT_STRING>"
```

5. Copy **Plaintext** for Apache ZooKeeper connection

![PlainTextZookeeper](./pics/Screen%20Shot%202023-03-16%20at%203.02.29%20PM.png)

6. Run the following command by replacing *<PLAINTEXT_MSK_ZOOKEEPER_STRING>* with copied MSK endpoint string. ***PLEASE DO NOT CLOSE THE TERMINAL"***. 

```
export MYZK="<PLAINTEXT_MSK_ZOOKEEPER_STRING>"
```

7. Go to MSK console. Copy **ARN** of MSK cluster

![MSK-ARN](./pics/Screen%20Shot%202023-03-16%20at%2012.50.27%20PM.png)

8. In terminal of **Cloud9 Basion EC2**, run the following command by replacing *<CLUSTER_ARN_STRING>* with copied MSK cluster ARN. 

```
export CLUSTER_ARN="<CLUSTER_ARN_STRING>"
```

9. Install `jq` command on Cloud9 instance by running 

```
sudo yum install epel-release
sudo yum install jq
```

## Setup Clickstream Lab 

1. [Create Service Linked Role for Amazon OpenSearch](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/mskkdaflinklab/setup#create-a-service-linked-role-for-amazon-elasticsearch) (previously amazon Elasticsearch) 

2. [Run the CloudFormation template for Clickstream lab](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/mskkdaflinklab/setup#run-the-cloudformation-template)


[BACK TO MAIN](/README.md)