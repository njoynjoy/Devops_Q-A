Terraform

```	 What is the difference between for_each and for in terraform? ```
for_each is used to create a multiple instances of a resource.
<img width="556" height="134" alt="image" src="https://github.com/user-attachments/assets/3eed6d6f-bb37-49af-9456-89f86bd4c5bc" />
 
each.key ir
each.key iterates how many times you want to create over the set for above I will take 3 times iterates over the set 

for is the loop we use in the shell script – it will loop over the variables or values 
 <img width="744" height="92" alt="image" src="https://github.com/user-attachments/assets/223b942e-068f-4eaa-af0b-cd05f97eecec" />

For is used as data transform in terraform, where as for each used for creating multiple resource 

```	What are modules in terraform? How do Terraform modules work? Why should we use them? ```
Terraform modules are the reusable groups same as like functions in python how a piece of code is used again and again in a program, similarly modules in terraform if we want to create same resource multiple times we use the modules
Suppose every development need the vpc module instead of writing it everytime I used modules 
Modue “vpc”{  
Source = “./modules/vpc”
Cidr_block = “10.0.0.0/16”
}
<img width="452" height="144" alt="image" src="https://github.com/user-attachments/assets/b308544e-dcac-4758-906b-801ebe1ac56f" />
 

In the source block will provide the location of the module 

```	Explain what is the state file in Terraform.? ```
In terraform statefile is heart(brain) of the terraform
Whenever Terraform creates a resource in AWS it stores that information in statefile , in future if it want to update resource it created the statefile know what is the current state of resource and what it needs to be updated.
When we use terraform plan, terraform apply commands it shows the resources creating,resources deleting and resources updating that is because of the terraform statefile only
 <img width="898" height="323" alt="image" src="https://github.com/user-attachments/assets/b28c2d23-77e6-417c-b973-d1d2b83fb02f" />


```	You can see the storing state file in Git other than AWS S3 and Azure Blob? ```
In terraform statefile holds the important information such as 
resource id
meta data
ip address
dependency b/w of resource
last known state of the resource.
All these are sensitive data so not stored in git repository because many people will be having access to the git repo whether it is private or public to avoid this state files are stored in remote backend like aws S3 or azure blob
It is very simple we will create a backend.tf file in that create a s3 and provide region all the details so terraform can upload the statefile to the backend.  And the best part with this remote back-end is that it can enable versioning. Enable security, enable strict RBAC and polices, enable backups,  locking of the statefile Two devops  engineers tries to update one resource created on the AWS same time, so it throws an error like "which one to take first or resource already exists locking the statefile makes this flexibility if one engineers starts working it will lock in his name and wont allow until the changes are done.

```	Explain Terraform State File Management? ```
Statefile is the heart of the terraform, statefile has the information of  resources that created –resourceID , dependencies, last state of the infrastructure
Last state means if the ec2 instance is created if I want to change the configuration to that instance it will check the resource id, dependencies if anything required and the last state of the infrastructure and will update the configuration accordingly.
As the state file has all the information we should not place that anywhere, it needs to be in a centralized location so that every can access the location and it should be secured. For this terraform supports a file called remote backend such as S3 bucket in AWS, BLOB In AZURE, GCS in google, the remote backends are capable when running the hcl file the statefile is updated. And also ensure that it is locked with name of the person executing the hcl file so that no one can make the change at the same time
And the state file that is stored in s3 has versioning, locking and security 

```	Two DevOps engineers try to execute the Terraform Apply command at once. What will happen? ```
In terraform there is state locking, in our organization we store statefile in s3 when both devops engineers requested at same time there is a race condition mechanism may be a latency based in them request one request may fast and other may reach late the s3 buckets creates the lock based the first reached request so that his code and his vpc or other resource is created first and after lock is released other devops engineer sees an error called vpc or other resource already exists.
Imagine there is no latency and s3 bucket reach same time then race mechanism decides which one to lock first

```	We don’t have a cloud account where we can store the state file? ```
The entire infrastructure is on premises, no cloud company using the openstack,vmware, other on premises
Remote backend concept of terraform is not restricted to cloud account. In the hasicrop page there are many remote backend consoles including the major ones are cloud s3,blob,gcs 
Terraform also supports hashicrop console, within the on premises or openstack we can install hashicrop console we can integrate hashicrop vault and can store the state file
Or terraform enterprise also the option with licensing

```	Do you use Terraform Enterprise or Community version? ```
In our organization we evaluated both terraform enterprise and community version
We are aware that enterprise has features like drift detection, statefile and rbac we have a strong devops team so we implemented these features by our own
Remote backend we have s3 where versioning, security and locking and also backup is maintained.
RBAC—we have implemented a strong RBAC and stored on a git repository and restricted the access to few devops engineer
Drift detection: means something updating manually on cloud. we implemented least privilege on the cloud provider that no one can access the cloud if they want to modify the resources they need to go to terraform to delete also they need to go to terraform and delete from there 
In a nutshell terraform is our single source of truth or single point of contact no developer can manually update or delete the infrastructure on the terraform.

```	Have herad of opentofu ? do you think it is better than terraform? ```
Both terraform and opentofu share there root, opentofu is created from terraform, It was created from terraform because of the licensing change from terraform. In our organization we just use terraform to create infrastructure and selling it






```	Write a Terraform code to create any resource on aWS? ```
Will create the terraform block first and download the required providers and will intiliazes the providers Instead of hardcoding the value I will use variable.tf file and if the resource block based on the resource will create the file examples3.tf, ec2.tf like wise if it is small will create in the main.tf itself.
Resource block syntax
Resource “----” ‘—‘{
----
----	
}
<img width="358" height="352" alt="image" src="https://github.com/user-attachments/assets/e28f3696-a87b-42df-9bc9-2eb16d117cdc" />
 

```	What is the difference between resource and data source in Terraform? ```
Resources is used to create the resource in terraform whereas the data source is used to read the data from the terraform. Created resource.
 
<img width="721" height="253" alt="image" src="https://github.com/user-attachments/assets/8af63b1f-b489-4010-a6d2-6a6d8ef12676" />
