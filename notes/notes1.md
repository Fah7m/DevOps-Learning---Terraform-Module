Terraform
---
Terraform is a tool that can be utilized to deploy resources in a Infrastructure environment such as AWS. It is a cloud agnostic tool that can deploy to any cloud and different providers. 

It also can deploy to Kubernetes resources and this is all done through the Terraform registry where certain plugins are installed and it will use certain API calls in order to be able to deploy to different providers. 


Infrastructure Orchestration vs Config Management
---

Infra orchestration would be the purpose of provisioning and managing cloud resources e.g. using Terraform to deploy EC2 instances, A VPC with subnets and a ALB connected to the instances.

Config Management would be configuring software and maintaing system state on already provisioned servers e.g. using Ansible to install Apache/NGINX to the EC2 isntances, deploy WordPress code, and ensure a service is running. 

In simple terms **Orchestration** is the deployment of resources in a cloud provider and **Config management** is the configuring of those servers to do specific tasks depending on what's deployment on them.


The process of Terraform
---

The process would normally go like this when working with Terraform

-First we would use Terraform to deploy a resource
-We then use a Terraform plan to cross check what your about to do
-Once that's fine we do a Terraform apply which puts your plan into motion in the Cloud Environment


Tips for using Terraform
---

-Terraform Docs
-Terraform Registry - this is where you will decipher what resources are essential for your deployments
-Testing and validation - It's important to know what your doing when working in a production environment so this is where thoroughly checking your Terraform plan comes into play and making sure it aligns with what you're trying to do.
-MVP - Minimum viable product meaning a minimal working infrastructure setup that proves the concept. E.g. isntead of writing a huge terraform project with VPCs, ALBs, ASGs, RDS, Cloudwatch etc.. you might want to start small first by creating on Single VPC, verifying it works then move on.
=Implement DRY software engineer principle - this means not repeating yourself as Terraform has modules that can be used as templates so making your utilizing modules and making your Terraform as DRY as possible is key. 


Terraform State
---
The terraform state file is the blueprint of the infrastructure meaning the state file is the record of your existing infrastructure. It's the up to date record of your current infrastructure setup. 

**Idempotency** in terraform means applying the same configuration multiple times and the infrastructure won't keep changing e.g. if my terraform file says create 1 EC2 instance and I run a terraform apply will create it, but now if I run a terraform apply again, it shouldn't create another EC2 instance because the desired state config file already matches the real state in AWS. **Terraform only makes changes when your config changes**.

```

resource "aws_instance" "web" {
  ami = "ami-123456"
  instance_type = "t2.micro"
}
```

Now running a terraform apply against that will create the EC2 instance but re running terraform apply again won't make a change to the infrastructure. 

However, if I change the instance type to "t2.small" and apply, then it will just update that field meaning it will destroy the existing t2.micro instance and create a new t2.small instance. 

**Desired state and Current state**

If I deployed two EC2 instances, all the details about those instances should be in my Terraform State file now. The desired state file is the terraform configuration of maybe changes I will be implementing or trying to deploy. 

So what it does is it compared this to your actual infrastructure (the state file) and from there it is able to decide what to do so if I added in an additional EC2 in my desired state file, then it would compared against the current state file and see there is two existing EC2 instances and I am trying to deploy an extra one. From there, the current state file will be updated to match my desired state file - so there will now be 3 EC2 instances now in the state file which is the blueprint of my infrastructure. 


Terraform Import
---
Terraform import comes into play when you've already deployed a resource in AWS without using terraform. 

Terraform import is a powerful command that allows you to take existing resources in your cloud environmment, and bring them under your Terraform management. This is a essential command when you want to integrate Terraform into an existing infrastructure setup without having to recreate each resource from scratch. 

**How this works**

Firstly, Terraform doesn't do all the magic for us if we run just the import command. A resource block is still needed with the relevant information and then we can run the import command.

Here I created a EC2 instance called import manually and have added that in the resource block with the correct AMI, instance type and name.

<img width="613" height="218" alt="image" src="https://github.com/user-attachments/assets/a98e51b5-ffb9-4e90-bd1d-f3317017d2dc" />

Then after saving the file I ran the below command with the correct instance id added at the end.

<img width="898" height="208" alt="image" src="https://github.com/user-attachments/assets/c81439de-cee1-45cd-a371-2baf91c9aafb" />


After running a terraform plan against that, the below returned.

<img width="1013" height="443" alt="image" src="https://github.com/user-attachments/assets/d2dd5379-6d59-47d3-b921-e8bd062c8ce7" />

Here terraform is telling me that the imported ec2 instance has a Name tag = "import" in AWS and my terraform configuration does not define any tags block. So running a plan here shows that the desired state doesn't match the current state and when we do an import we want to make sure the desired and current state is exactly the same without having to apply any changes.

I also needed to add in a "user_data_replace_on_change = false" in the resource block for that imported EC2. 

Here is the updated resource for the imported instance. 

<img width="418" height="180" alt="image" src="https://github.com/user-attachments/assets/7bb9429a-5e50-4c4c-a6d0-a984a9982f23" />

Now running a plan against that will return nothing as the desired state matches the current state - the terraform config file matches what's in the infrastructure currently and this is our desired result when doing an import of manually added instances. 


Local state and Remote state
---

A local state file is created by default and stored locally on your machine when the terraform apply command is ran. The local state file setup is ideal for small projects as there is no additional configuration requred to store the state file locally and it keeps everythings contained and simple. 

Remote state files address many of the challenges that come with local state files  by storing the state in a central, secure location that allows for a collaborative approach.

By storing state files remotely, multiple team members can access and update the same infrastructure state without risking conflicts or inconsistencies. Also, remote backends like AWS S3 buckets, Terraform cloud offer state locking which prevents users from making changes at the same time which reduces state corruption. 

Another benefit for remote state files is that they have automatic backups and security. Remote backends can automaticaly backup the state file and apply encryption which ensures the state file to be secure and recoverable.

Here I have already manually created the the S3 bucket where I'll be remotely storing my state file and I've now referenced that in my provider.tf file with the Bucket name, what I'll be saving the bucket as, and the region in which this bucket is deployed.

<img width="498" height="362" alt="image" src="https://github.com/user-attachments/assets/dd9150c6-0fc5-492c-bac1-561725ab955c" />

After saving that file and running terraform init, the below is returned. This shows that the provider file is setup and our IAM access is correct in order to deploy a object within that S3 bucket. 

A 403 error would be returned if we didn't have the correct permissions for that AWS account.

<img width="653" height="299" alt="image" src="https://github.com/user-attachments/assets/d464f3a4-a38a-4788-b527-fffc44feaa27" />




Terraform Providers
---


<img width="687" height="722" alt="image" src="https://github.com/user-attachments/assets/aefb9e4f-2bdc-4a8c-9e3b-3ca75cfdc1a0" />
