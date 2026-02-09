AWS
 ``` 	Explain how you will design a highly scalable and available multi-tier app? ```
I will create a virtual private cloud (VPC) depending on the applications that need to be running on the virtual private load. I will define a CIDR block. in the VPC, I will create a public subnet and a private subnet. also define the CIDR range for both public and private subnets.
in the public subnet, I will place a load balancer. In the private subnet, I will create the virtual machines. and these virtual machines will be configured through AWS Auto Scaling Groups. So that these virtual machines will be scalable within the virtual machines, I will deploy an application server that the applications will be deployed. these applications will interact with the database that I will create in the private subnet or I can create a separate private subnet for the data base 
for the load balancer in the public subnet we will also grant access for it to the private subnet. my load balancer will be both public-private load balancer. and for this load balancer, the target groups are assigned via the virtual machines. Or auto scaling group
 <img width="396" height="539" alt="image" src="https://github.com/user-attachments/assets/9310ed82-e4a0-4dbc-baaa-364d9f7e73c8" />

 <img width="809" height="211" alt="image" src="https://github.com/user-attachments/assets/bcb305aa-09b2-48a0-8b98-805710c74192" />

availability and scalability are achieved through auto-scaling group.

	``` What is AWS NAT and when is it used? ```
NAT in AWS is network address translation
we have a virtual machine deployed in private subnet,  where the back-end application is deployed and it needs to access the GitHub or internet. in this case, we use NAT.  we create NAT and we create a route which will have in the route table address of the destination that is 0.0.0/0 instead of pointing out to the internet it will point to the NAT. so when the request is sent from the backend application to the internet, first it will go to the NAT gateway. Then from NAT gateway it will go to the GitHub or the internet. 
what NAT does here is when the packet reaches the Github. NAT source would be the NAT IP, not the Source IP of the application, and destination would be the internet or GitHub.
 NAT gateway masks the IP of the private ip, and it allows access to the external that is Internet.
1.External access to the internet and 
2, the request is completely secure
 <img width="945" height="502" alt="image" src="https://github.com/user-attachments/assets/5b3b59d2-a2f4-4b68-bb2f-383a6c35bbeb" />


	 ``` how to enable internet access to the application deployed in a private subnet of VPC?``` 
when you create a subnet, we define the destination route of that subnet. for a public subnet, the destination route would be 0.0.0.0, which is the internet. when we create a private subnet, we don't define the destination route. So the applications placed in the subnet are absolutely secure
NAT gateway is used we create a NAT and route the private subnet request Is sent to route and in the route the the NAT ip will be the destination. What NAT does is it performs Source network address translation masks ip of the private subnet application and replaces with NAT ip  and in the packet of tcp layer both source ip and destination ip is stored. And transfer secure and provide the internet access



	``` Can applications in different subnets of a VPC interact by default? If not, why not? ```
Yes applications with in different subnets of the VPC can interact with each other, you login one of the application ping to the other application in different subnet it connects because they are in same VPC
When we create a VPC the aws will create a main route with CIDR and range and the target is set to local suppose the cidr range is 10.0.0.16local so by default the subnets can communicate with in the VPC any external communication is disabled where aws provides security groups, NACL  and can block the requests

	``` Explain NACL vs security groups, which one do you use commonly in your organization? ```
NACL(Network Access Control List) subnet level
Security group  instance level 
The default behavior of NACl is deny any traffic at the subnet level
Rules in NACL where we want to allow traffic on a specific port or deny the traffic
DENAY ALL rule denays all the traffic to the application
When we want the rules at instance level not the subnet level then we use Security group and majority of times we go for combination of NACL and Security groups
NACl is stateless in nature if you allow port 80 in subnet ingress u need to allow egress also  when u define port 80 in inbound traffic rule u need to define the outbound traffic rule for 80
NACl is stateful in nature if you allow ingress by default egree is also allowed when you define port 80 in the Inbound Traffic rule, no need to define the outbound port 80 

	``` EC2 instance terminated unexpectedly. How will you troubleshoot the issue? ```
In AWS there is a service called cloud trail using cloud trail we can find out what can be the potential issue there is an API called "TerminateInstances". through this API call, I can find whether it is an issue with the auto scaling group automation scripts or lifecycle management. If not, I will check the instance whether it is aSPOT instance or not because for the critical EC2 instances we should not use SPOT instance.
	``` Lambda functions fail randomly how will you fix the issue? ```
lambda functions performs a HTTP requests or it interacts with any AWS service like RDS, S3. Then the lambda function is invoked  When the lambda function is invoked, sometimes it fails randomly and there are no erros in log
I will follow the troubleshooting steps
1.To understand the complete journey of the request I will enable tracing for this service. To enable it, we need the AWS X-Ray service.
2. I will check if there is any call latency (if latency is there, I will check which availability zone the service is connecting from. Suppose EBS,s3 service the which availability zone or which region these services are there
3.i will try to increase the retries through try catch or through loop
4.can increase the time out 
Rewrite the lambda function if 3rd and 4th steps didn’t work

	``` What will you do when the AWS RDS storage is full? ```
We can  increase the storage, but it solves only 10% of the problem as an instant solution.
1.	take the snapshot of the RDS and increase the storage a more compared to the previous storage.
2.	We can check any auto scaling supported by RDS
3.	I will go to RDS and I will check the database tables and Objects that are being used.
4.	I will check any  any database table or object that is consuming a large space on the RDS  the below query gives the large space occupied in the database and share that with the developers and do similarly with tables,, and objects  not required objects we can delete in the cache and duplicate objects
 <img width="822" height="44" alt="image" src="https://github.com/user-attachments/assets/19951e9b-7470-4657-aa98-5ee20e33f6d7" />

5.	Apart from that, I will head to CloudWatch and create a metrics that keeps monitoring the RDS storage.



``` 	Developer deleted a critical resource like S3, EC2, or RDS. What will you do? ```
1.	because the critical resources are deleted, I will try to create the resources again. Mostly all of these have backups comings to
- s3 where s3 has versioning, if versioning is enabled I will create from previous version
-RDS has something like  point-in-Time (PIT) Restore Solution so I will head  to RDS and restore the RDS
-EC2  has something like snapshots  if they are easy to instance is created from a particular AMI. I will use AMI to recreate the EC2 , I will use snapshots for the volume, and through AMI, I will create the EC2 instance and attach the snapshot volume to the EC2.

2.	My long term solution is RBAC, IAM  it is always a good practice like  provide zero privilege,  least privilege or give access to the right people, as the developer should not have access to delete the resources.
--IAM 
--using infrastructure as code terraform or cloud formation template
--it should be integrated with git versioning for proper auditing 

	``` Explain a cost optimization activity that you performed in your organization? ```
cost optimization is my day-to-day part of role recently, I worked on a cloud cost optimization task. where to find the unused EBS volumes and delete those unused volumes.
Lot of developers create EBS volumes and at sometimes ask us to create the EBS at time they use and after that remain unused our task is to identify that unused EBS. and delete them.
The step I use AWS cli to run aws ec2 describe volumes  but then I realized the better approach is using lambda functions 
I used lambda functions where python as scripting language  to remove unused volumes with in the python code I have used Boto3 and got unused voumes I have deleted also with boto3. I have set up this lambda function periodically so that  it would be executed every month once.

similar example is like in the EBS volumes we have GP2 and GP3, where GP2 is a high cost and GP3 is low cost and with high iops  we have volumes that are created for the last two years, and I wrote a lambda function I have upgraded the GP2 to GP3 with a single python command. For this also I have used boto3

	``` Explain a recent challenge you faced with AWS. How did you resolve that ```
the challenge I faced with AWS is regarding the code commit service. as Adam just announced, it was the end of code commit. we have nearly 100 repository source codes in the code commit. So we have to look for migration strategies where  we have evaluated GitHub and GitLab. And we decided to move to github and led this effort of comparison of different version control systems the reason to move to github is its features and ai related to copilot and ecosystem of github
I have prepared dashboards with: 
•	Less critical repositories
•	Critical repositories
•	Most important repositories
In first 1 month we have migrated the less critical ones implement the CI/CD through aws code pipeline the next month critical ones and finally most important ones and after 4 months we have migrated all the repo to with zero impact 

	``` Auto scaling group is not launching EC2. What is the issue? ```
-I will check the launch configuration and what is written in that launch configuration.
-I will check if a valid AMI and instance type are used or not in the launch configuration. 
- is there any subnet or any issue with the availability zone or capacity
- it can be you are exhausted of the EC2 quota for that availability zone or using spot instances. The spot instances also gives us the interruption errors.
- finally, I will check the permissions with IAM rules. Is there any policy that was changed recently


	``` Which AWS service you use in day to day life? ```
Take a look at the JD and speak only relevant to the job description only 
VPC- network and related things
Cc2- compute
EKS- managed Kubernetes
ECR- container registry
S3- storage objects
And services aligned for this are 
RDS- for data base services
EBS- for volumes
AWS code pipeline- for ci/cd
AWS Code deploy- for deployment

	``` Have you used AWS EFS? If yes, what issues have you run into? ```
EFS: elastic file storage: it is useful in shared storage
When you are looking for shared storage and, scalable and serverless in nature then EFS is the right solution so for that 
Regarding the issue with one of the development team they came up with mount target is unreachable with EFS when I try to mount EFS, the mount was failing , I have checked  is EFS available in the mount target is available on all the ec2 instances that are deployed in the availability zones and the security group attached to the EFS and dns resolution problem and finally checked the IAM that is attached to the EFS access point, and the issue is with IAM Access point

 ```	When will you go to EFS over EBS in real-time? ```
EFS: Elastic file system: it is used in shared file system: when you have multiple h2o instances and all these instances have to share a common file system or concurrently Access the data the we go for EFS
Example:  in eks pods are deployed in different nodes for an image processing application the pods access the image process the image and upload the image to common file system in this different pods uploading to the same system and same time as well we will be using the EFS
 <img width="395" height="222" alt="image" src="https://github.com/user-attachments/assets/9fe7d88e-797c-446a-9e41-51fede120840" />

Advantage: highly scalable
EBS: Elastic block storage:Dedicated to a single Ec2 instance may be for high performance then we are looking for EBS
Example: when working with databases where we need high performance and the volume is directly attached to the database instance so that we have high throughput and read and write operations then we use EBS, where the instance directly has access to the  block or storage 

	``` Disable AWS console access to the IAM user? ```
We have 3 different ways of creating the users 
1.CLI
2.Infrastrcture as code (Terraform)
3.console
In console there is field to provide user access to the Aws console, just uncheck that so the user manually have to download the cli and work to access the 
<img width="975" height="280" alt="image" src="https://github.com/user-attachments/assets/b0271012-b9aa-418d-b503-3299be175ce1" />

If we are using terraform there is module for terraform where we disable the field console access to the users 
If we are writing custom script to the CLI then we should perform this 

	``` How can an AWS Lambda function within one account connect to the S3 bucket of the other account? ```
Consider two aws accounts as A And B One aws account connects with s3 bucket of other AWS account
1.	In A There will be a lambda function if not create a lambda function
2.	Head to the lambda execution role- policy attached to the role and provide name of the s3 bucket
3.	In other Aws account B go to s3 
4.	Go to bucket policy and in and make sure lambda function of Aws Account “A” has access to it. Paste the ARN in the bucket policy of A in then it can access
“To connect to other AWS account in the principal  ARN account id is added.”	
 <img width="784" height="343" alt="image" src="https://github.com/user-attachments/assets/d0f550c1-7f29-4d8d-a08e-4e7a5fdb660f" />

	``` What is AWS STS and how does it work? ```
In AWS STS is a service that stands for security token service
Sts Allows for service, user or principal a temporary IAM role access so that they can perform the activity which I am role capable off
 <img width="620" height="392" alt="image" src="https://github.com/user-attachments/assets/0f52fad9-9409-4733-93b9-67a75dba6bf2" />

Suppose a lambda function need to get the details from dynamo db but it doesn’t have access in this case STS is used will provide temporary access as the IAM role and access the dynamo DB
  <img width="975" height="613" alt="image" src="https://github.com/user-attachments/assets/c945bdc0-90eb-453d-88d0-0cfb46c8353c" />

IAM roles are created by devops engineers but they can assign to service and principals not particular users
So services like lambda function during their execution wants temporary access as  iam role for that STS is used in the lambda function required credentials like accesskeyid, secret access key id, sessionToken.
If STS want to access different aws account then lambda function need to update with trustPolicy.

	``` What is trust policy in AWS and why is it used? ```
AWS allows users, services or principals to temporarily assume an IAM role this can be achieved  by STS service, all services should not get STS access and all services shouldn’t get information from dynamodb for this with in the IAM role we will define a Trust policy where this trust policy tells which services can assume the IAM role and fetch the information from DynamoDB temporarily. Where trust policy works in different aws accounts also, It doesn’t has to be a service it can be user also
Aws---IAM—Roles—trust relationships where you will find a policy and in principal we define which service can assume the IAM role access
 <img width="794" height="570" alt="image" src="https://github.com/user-attachments/assets/8dd3167d-09d8-4b0c-805e-ac9d1a006e74" />


	``` How a Lambda function in AWS account A can interact with DynamoDB of  AWS account B? ```
1.In aws account B—in IAM role- a policy attached and  this policy will have access to DynamoDB Get Items
2. in AWS account A, I will create a Lambda function where I will use the STS service. To fetch temporary credential service of role created in Account B
3. in AWS account B, I will look into the IAM Trust policy. And update it to grant the lambda function access of account A
 <img width="761" height="473" alt="image" src="https://github.com/user-attachments/assets/0fdb2338-5cc5-4360-bf96-4445126e1d1d" />


	``` What are the disadvantages of EBS volumes in Kubernetes? ```
EBS volumes comes with disadvantages, especially when you are dealing with stateful applications
1.	EBS volumes are tied to an availability zone. where in Kubernetes pod rescheduling is common. shuffling between the nodes suppose  we have a 2-node Kubernetes cluster where one node is in availability zone 1a and the other node is in availability zone 1b. we have pod and that pod is attached to an ebs volume for storage purpose
If the pod is  we schedule to another node availability zone, then the EBS volume attached to it is not automatically available. In 1B
 <img width="625" height="208" alt="image" src="https://github.com/user-attachments/assets/7fe7fd2d-d517-45fc-b2ee-552ba214d68b" />

2.	EBS volumes are block storage. They are mounted to a particular node at once. suppose a database is having two ports where they are interconnected or having same data when they try to access the single volume connected to both. It is not possible  because block storage can be attached to a single EC2 instance.
 <img width="742" height="195" alt="image" src="https://github.com/user-attachments/assets/e1137a8f-0dcf-468e-ad0f-3b7c2c59ed4e" />

we can go for EFS or FSX on AWS FSX on tap solution

	``` AWS Secret Manager vs  AWS system manager Parameter store? ```
they both are used to store the sensitive information.
If we are looking for basic security not like automatic rotations, cross account support we can use System manager parameter store, free cost
if we are looking for rich security good to change password, API token so they are highly sensitive it also supports cross-account sharing like dev, stage, prod, if  we have the same DB password. It is possible to share that across the account through secret manager, higher costing 

	``` Explain the day-to-day activities related to databases? ```
I provision and configure the databases through- Infrastructure as Code. I used Terraform, and AWS RDS service and spin up databases like MySQL.
I also track on monitoring databases understand performance of databases like CPU, Memory, latency, no of connections
I also work on taking backups of databases like snapshots, backup of the voulmes and store that on compute.
security, like IAM roles, access  DB. I'll take care once the user leaves the organization, I'll make sure to delete the DB related access for the user
it is also my responsibility to implement the disaster recovery on the database  so if something goes wrong with the production db instance or the staging  make sure to set up disaster recovery with the concept of multi-availability zone. or at least replicas are configured.

	``` Have you worked with Lambda functions in AWS? explain your activities in Lambda? ```
1.in current organization, I have implemented lambda function for cost optimization.
2. I have also used lambda functions to enforce compliance in the organization.
1. I have implemented a Lambda function if any unattached EBS volumes for quite a long time through SNS I will trigger a notification using the Lambda functions, and it would be sent to the developers. I also will send reminders, and they would be taking timely action.
along with that, I have also written the functions for any stale snapshot. Any unused AMI with in the organization
2. enforcing Compliance: resources created in AWS without any tags long back. With lambda function I have identified the AWS resources without tags again through the SNS notification. I have informed certain development teams and they have took necessary actions.

	``` What is the difference between IAM user and role? ```
in AWS, IAM is a service that is used for authentication and authorization.
In which  IAM user is for long-term identity.-- when a new DevOps Engineer or User joins the organization, he will be provided with specific IAM user access. After he leaves the organization, the user will be deleted.
IAM Role:Short term identity-- administrators set up IAM roles with set of permissions, these roles can be assumed by users, services, and Applications running 
example 1: one AWS service wants to connect to the other service (e.g., EC2 instance wants to connect to S3  we can create an IAM role and attach that role to the EC2 instance 
