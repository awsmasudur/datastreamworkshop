# Real-time data processing core concepts and use cases - LAB1

In this lab you will create a data stream using AWS Management concolse and will use AWS CLI to perform some stream operations.

1. Go to AWS Management console:
  https://console.aws.amazon.com/
2. If you don't have AWS account please follow the steps mentioned here: https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/
3. Switch the region to N. Virginia: https://console.aws.amazon.com/console/home?region=us-east-1
4. From the service home page click on **Kinesis**
5. Click on **Data Streams**
![Image of KDS](/images/KinesisDSHome.PNG)
6. Select **Create data stream** 
7. Create a data stream as follow.   
	Data stream name: **dswlab1**  
	Number of open shards: 1
![Image of KDS](/images/Lab1-Image2.PNG)
8. Wait for the stream to be in **Active** status
9. From the Services menu select IAM and open it in a new window
![Image of EC2](/images/Lab1-Image3.PNG)
10. Go to Roles --> Create role  
![Image of EC2](/images/Lab1-Image4.PNG)
11. Select type of trusted entity - **AWS Services**  
Select **EC2**  
Select **Next:Permissions**  
![Image of EC2](/images/Lab1-Image5.PNG)
12. Select **AdministratorAccess** policy and click Next
13. Click next on Add tags page
14. Enter role name: **dswlabrole** and create the role

## Configure EC2  
1. Create a EC2 instance with **Amazon Linux 2**, select the IAM role you have created already (**dswlabrole**), ensure you have internet access on that machine. If you are new to AWS, follow this steps to create a new instance: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html  
2. Use a SSH client to connect with the EC2 instance: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html or you can also use EC2 Instance Connect mentioned here: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Connect-using-EC2-Instance-Connect.html
3. After connecting using SSH enter: ```aws configure```  
	Press **enter** for all settings except region. Enter **us-east-1** as default region  
4. Run the below command to ensure AWS CLI has been configurred correctly.  
```aws kinesis list-streams```  
**Sample output:  
{
    "StreamNames": [
        "dswlab1"
    ]
}**  

## Performing straming operations with CLI  
1. Run below command to see the configuraiton of your previously created stream  
```aws kinesis describe-stream --stream-name dswlab1```   
2. Run below command to insert data to stream  
```aws kinesis put-record --stream-name dswlab1 --partition-key P001 --data testdata```
3. Copy the **SequenceNumber** from the output of the previous command
4. Lets read data from the stream. To do this run the below command first  
```SHARD_ITERATOR=$(aws kinesis get-shard-iterator --shard-id shardId-000000000000 --shard-iterator-type AT_SEQUENCE_NUMBER --stream-name dswlab1 --starting-sequence-number REPLACE_WITH_SEQUENCENUMBER_YOU_COPIED --query 'ShardIterator')```  
4. Now run this command  
```aws kinesis get-records --shard-iterator $SHARD_ITERATOR```  
5. This will return a result as below:  
{
    "Records": [
        {
            "Data": "dGVzdGRhdGE=",
            "PartitionKey": "P001",
            "ApproximateArrivalTimestamp": 1587951917.165,
            "SequenceNumber": "49606373519072809783930760965737010169802318611644153858"
        }
    ],
    "NextShardIterator": "AAAAAAAAAAFhGBdelerzLp3cCzreWKlkGgl6Yor+gUoGMgWDaL37en+W9qqhIy3Iv6U1xOIYv8f8qn/jwwQ1lUGhurwh0UJC3Mlgl5nHpZfFfbs33xpnr7kTyr1fCCIIpTr6ju9oyxiMLHEbfPAqxXvHQR3xIMorXwPWpsHfx2CpuhO9hzQztqnaITvoWQyzXTsPILn5LmUZ9VvaP/clC9s2Ox3aDBZU",
    "MillisBehindLatest": 0
}  
6. The **Data** in the output is Base64 encoded. If you are using KCL it will decode automatically. But for this lab we will decode it manually. To do this go to https://www.base64decode.org/ and paste the value of **Data**  you got on your get-records api call. You will see the exact data that you enterred using put-record api.  

To learn more about PutRecord and GetRecords API read the below documentations:  
[1] https://docs.aws.amazon.com/kinesis/latest/APIReference/API_PutRecord.html
[2] https://docs.aws.amazon.com/kinesis/latest/APIReference/API_GetRecords.html
