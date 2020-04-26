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
15. Create a EC2 instance with **Amazon Linux 2**, select the IAM role you create already (**dswlabrole**), ensure you have internet access on that machine. If you are new to AWS, follow this steps to create a new instance: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html

From the AWS Service menue choose **EC2**  
16. Select **Launch instance**  
17. Select **Amazon Linux 2** 
18. Select **t2.micro** and click on **Next:Configure instance Details**
19. Select the IAM role you created: **dswlabrole**, select **Next:Add Storage**
20. Change the size to 50GB and select **Review and Launch**
21. Select **Next:Configure Security Group**
22. Create a new security group. Group name:**dswlabSG**
23. Select **Review and Launch** --> select **Launch**
24

