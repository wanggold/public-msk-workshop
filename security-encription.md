# Security and Encryption

## Content

## TLS Encryption in transit

1. Start from [Go to the AWS ACM Console](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/securityencryption/setup#:~:text=Go%20to%20the%20AWS%20ACM%20Console%C2%A0)

2. Setup Keystore and Truststore in the Apache Kafka client EC2 instance [Link in Workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/securityencryption/setup#setup-keystore-and-truststore-in-the-apache-kafka-client-ec2-instance) 

    * Go to CloudFormation and copy the **stack name**
    ![StackName](./pics/Screen%20Shot%202023-03-16%20at%204.00.19%20PM.png)

```
cd /tmp/kafka
python3 ./setup-env.py --stackName mod-c6ca52ffa84841e0 --region us-east-1 

java -jar AuthMSK-1.0-SNAPSHOT.jar -caa arn:aws:acm-pca:us-east-1:536094702778:certificate-authority/478bbe01-4e88-4b6d-944e-32e6ea632c1c -ksp password -ksa msk -pem

```

3. 

