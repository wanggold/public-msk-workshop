# MSK Workshop introduction and preparation
[BACK TO MAIN](/README.md)
## Loging AWS account 
## Preparation

### Connect to KafkaClientInstance

1. Download the keypair for SSH connect to Kafka client EC2 instance
![SSH-KeyPair](/pics/Screen%20Shot%202023-03-16%20at%209.23.05%20AM.png)

2. Open environment in Cloud9
![OpenCloud9](./pics/Screen%20Shot%202023-03-16%20at%209.28.30%20AM.png)

3. Upload downloaded keypair to Cloud9 environment
![UploadKeypair](./pics/Screen%20Shot%202023-03-16%20at%209.31.55%20AM.png)

4. Go to the **bash** pane at the bottom and type in the following commands to setup the ssh environment so that you can access the KafkaClientEC2Instance.
```
chmod 600 ee-default-keypair.pem
eval `ssh-agent`
-add -k ee-default-keypair.pem
```
![SetupKeyPair](./pics/Screen%20Shot%202023-03-16%20at%209.35.24%20AM.png)

5. Find Kafka Client EC2 instance IP address. Copy the IP address.
![FindClientIP](./pics/Screen%20Shot%202023-03-16%20at%209.39.54%20AM.png)

6. In your bash pane, you will use standard ssh to connect to the IP address of your instance. For most labs, this will be your KafkaClientInstance. 
```
ssh -i ee-default-keypair.pem ec2-user@<INSTANCE-IP>
```
![SshToClient](./pics/Screen%20Shot%202023-03-16%20at%209.44.18%20AM.png)

