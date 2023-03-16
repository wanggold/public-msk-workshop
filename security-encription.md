# Security and Encryption

## Content

* [TLS Encryption in transit](#tls-encryption-in-transit-15-mins)
* [SASL/SCRAM with AWS Secrets Manager](#saslscram-with-aws-secrets-manager-15-mins)
* [TLS mutual authentication](#tls-mutual-authentication-15-mins)

## TLS Encryption in transit (15 mins)

1. Start from [Go to the AWS ACM Console](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/securityencryption/setup#:~:text=Go%20to%20the%20AWS%20ACM%20Console%C2%A0)

2. Setup Keystore and Truststore in the Apache Kafka client EC2 instance. Start from "[Enter the following commands to setup the Amazon MSK environment variables"](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/securityencryption/setup#:~:text=Enter%20the%20following%20commands%20to%20setup%20the%20Amazon%20MSK%20environment%20variables.) 

    * Go to CloudFormation and copy the **stack name**
    ![StackName](./pics/Screen%20Shot%202023-03-16%20at%204.00.19%20PM.png)

    * Create an stack name variable
    ```
    export stack_name="mod-c6ca52ffa84841e0"
    ```

    * Setup the Amazon MSK environment variables by running the following
    ```
    export region=$(curl http://169.254.169.254/latest/meta-data/placement/region)
    cd /tmp/kafka
    python3 ./setup-env.py --stackName $stack_name --region $region 
    . ./setup_env $stack_name
    ```

    * Run the **AuthMSK-1.0-SNAPSHOT.jar** jar file like [link to workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/securityencryption/setup#:~:text=Now%20run%20the,.%20Parameters%3A)
    ```
    cd /tmp/kafka
    java -jar AuthMSK-1.0-SNAPSHOT.jar -caa arn:aws:acm-pca:us-east-1:536094702778:certificate-authority/478bbe01-4e88-4b6d-944e-32e6ea632c1c -ksp password -ksa msk -pem

    ```

## SASL/SCRAM with AWS Secrets Manager (15 mins)

* Follow the [link to workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/securityencryption/saslscram)

* Correction on [**Authorization Section**](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/securityencryption/saslscram/authorization#client-setup)
    
    * Copy **SASL/SCRAM Bootstrap servers** string like
    ![sasl-boostrap](./pics/Screen%20Shot%202023-03-16%20at%205.27.10%20PM.png)

    * In Kafka client EC2 terminal, run the following command after replacing **<SASL_BOOTSTRAP_SERVER_STRING>** by copied **SASL/SCRAM Bootstrap servers** string
    ```
    export brokerssasl="<SASL_BOOTSTRAP_SERVER_STRING>"
    ```

    * Paste and run the command in [link to workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/securityencryption/saslscram/authorization#client-setup:~:text=Go%20to%20the%20/home/ec2%2Duser/kafka%20dir%20and%20and%20setup%20user%20alice%20to%20have%20permissions%20to%20create%20or%20delete%20topics%2C%20create%20or%20delete%20Topic%20ACLs)

## TLS mutual authentication (15 mins)

* Follow the [link to workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/securityencryption/tlsmauth)

* Note for ["You can run the following command to get the Distinguished-Name..."](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/securityencryption/tlsmauth/authorization#:~:text=You%20can%20run%20the%20following%20command%20to%20get%20the%20Distinguished%2DName
    * The password used with `keytool` is created in [link](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/securityencryption/setup#:~:text=%3E%20%2Dksp-,password,-%2Dksa%20msk%20%2Dpem)

* Note for ["Run the following commands to give Read and Write access to the test topic."](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/securityencryption/tlsmauth/authorization#:~:text=Run%20the%20following%20commands%20to%20give%20Read%20and%20Write%20access%20to%20the%20test%20topic.)
    






