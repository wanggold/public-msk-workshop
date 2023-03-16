# MSK Operations

[BACK TO MAIN](/README.md)

## Content

* [MSK Cluster Creation Lab](#msk-cluster-creation-lab-15-mins)
* [MSK Cluster Expansion Lab](#msk-cluster-expansion-lab-30-mins)
* [Cluster Storage Expansion Lab](#cluster-storage-expansion-lab-10-mins)

## MSK Cluster Creation Lab (15 mins)

1. Overview MSK creation lab architecture [Link to MSK workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/clustercreation/overview) (5 mins)

2. Create MSK cluster with AWS console [Link to MSK workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/clustercreation/console) (10 mins)

## MSK Cluster Expansion Lab (30 mins)

1. Overview MSK cluster expansion lab [Link to MSK workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/addingbrokers/overview) (5 mins)

2. Preparation for cluster expansion [Link to MSK workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/addingbrokers/prep) (5 mins)

3. Add brokers to MSK cluster using AWS console [Link to MSK workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/addingbrokers/console) (5 mins)

4. Check MSK cluster expansion using CLI. *Reference on [Adding brokers to a cluster using the CLI](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/addingbrokers/cli)* (5 mins)

    1. Go to Cloud9 EC2 instance terminal

    2. Get configuration hash for the cluster

    ```
    export CLUSTER_VERSION=$(aws kafka describe-cluster --cluster-arn $CLUSTER_ARN --output json | jq ".ClusterInfo.CurrentVersion" | tr -d \")

    ```

    3. List MSK cluster operatioins

    ```
    aws kafka list-cluster-operations --cluster-arn $CLUSTER_ARN
    ```

    4. If OperationState has the value of `UPDATE_IN_PROGRESS`, wait a while, then run the command again. When it is complete, the status will be `UPDATE_COMPLETE` and the new brokers are ready for partitions to be assigned.

5. Re-assign partitions after changing cluster size [Link to MSK workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/addingbrokers/reassignpartitions) (10 mins)

    * Correction on **Build a new partition configuration**  

    ```
    bin/kafka-reassign-partitions.sh --bootstrap-server $MYBROKERS --topics-to-move-json-file topics-to-move.json --broker-list "1,2,3,4,5,6" --generate --zookeeper $MYZK
    ```

    * Copy `Current partition replica assignment` to `original-state.json`.

    * Copy `Proposed partition reassignment configuration` to `expand-cluster-reassignment.json`

    * Correction on **Execute the change**

    ```
    bin/kafka-reassign-partitions.sh --bootstrap-server $MYBROKERS --reassignment-json-file expand-cluster-reassignment.json  --zookeeper $MYZK --execute
    ```

    * Correction on **Monitor the progress**

    ```
    bin/kafka-reassign-partitions.sh --bootstrap-server $MYBROKERS --reassignment-json-file expand-cluster-reassignment.json --zookeeper $MYZK --verify

    ```

## Cluster Storage Expansion Lab (10 mins)

1. Overview of storage addition process [Link to MSK workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/addingdisk/overview) (5 mins)  

2. Expand storage using the AWS Console [Link to MSK workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/c2b72b6f-666b-4596-b8bc-bafa5dcca741/en-US/addingdisk/console) (5 mins)  

[BACK TO MAIN](/README.md)