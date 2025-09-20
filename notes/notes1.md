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


Input Variabels
---
Input variables in Terraform a

Here we can see that a variable has been created called instance_type which is a string.
<img width="520" height="193" alt="image" src="https://github.com/user-attachments/assets/09a0f07f-09dc-45e2-8625-3fba2b6b6157" />

Then we reference the variable by doing a var.instance_type
<img width="1364" height="402" alt="image" src="https://github.com/user-attachments/assets/bb407e10-b774-4b7e-99fb-86c826b8b6fd" />

Now after running terraform plan we can see it expects a value. 
<img width="1609" height="264" alt="image" src="https://github.com/user-attachments/assets/46696f5e-c5c7-4ebd-8cc1-6dfa90aac3ab" />

Or we can set a default tag within the variable for it to automatically take that as the result. 
<img width="322" height="36" alt="image" src="https://github.com/user-attachments/assets/167bd0ab-3c3b-49d1-b68d-315bb68a90cd" />


Local Variables
---
Local variables in Terraform are for value you set or calculate once and then resuse in multiple places in your terraform configuration. THey are mainly there to avoid repeating yourself and to keep your terraform configuration **DRY**.

So in a case where you want to keep the AMI id the same throughout your infrastructure, you can set a local variable and reference that throughout your configuration. 

Here we set a local variable for instance_ami which will be the default machine image we want to use
<img width="391" height="94" alt="image" src="https://github.com/user-attachments/assets/3b51773d-0000-4bb9-a511-56f547cd6fa6" />

Now in our EC2 resource block we reference that local variable by doing a local.instance_ami to call it.
<img width="425" height="136" alt="image" src="https://github.com/user-attachments/assets/1dfe28f2-3dea-496b-9440-ee1b4e3a720c" />


Output Variables
---
THe output variable in Terraform is used to display a desired result when running a terraform apply e.g. I can have a output variable displaying what the instance id is, or the instance ip, or the region the instance is deployed. 

Here I have created an output block and referenced a EC2 instance I have created earlier by using it's variable name and adding ID to display the instance ID
<img width="393" height="92" alt="image" src="https://github.com/user-attachments/assets/2d0a95e9-56be-4214-8137-d13e7dfa3b04" />

Here is the EC2 instance that was created I am referencing in that output block named "this"
<img width="420" height="97" alt="image" src="https://github.com/user-attachments/assets/d11788d7-c8cc-462a-89fd-8ad3b3dd9f78" />

Now when running a terraform plan and apply there should be no changes as I have created that instance already but am now just asking terraform to display a output in the terminal.
<img width="1172" height="198" alt="image" src="https://github.com/user-attachments/assets/ee3ed2d1-8b5c-4a5c-bd26-48f70573a92d" />

Terraform apply:
<img width="1017" height="350" alt="image" src="https://github.com/user-attachments/assets/b77b58ab-d8d1-4e1e-8a9d-39cc3ed41d00" />


Terraform Modules
---
Terraform Modules are a collection of tf files in a single directory. Using terraform modules is a way to keep your terraform configuration resuable and consistent e.g. I have used a module already in my terraform setup called a root module which basically is the directory where I can a terraform init/plan/apply and this is where I was describing my ec2.tf variables.tf and terraform.tfvar.

A chold module is anything I split off and call whereas a root module is where I run directory. In simple terms the root module is where I initially run my terraform commands (init/plan/apply) and the child module is a subdirectory within my project folder or a remote source like github or terraform registry which contains its own tf files. This child module is then called within my root module using a module block like below.

The folder structure would be like the below:
```
project/
├── main.tf          # root module
├── variables.tf     # root module vars
└── modules/
    └── ec2/         # child module
        ├── main.tf
        ├── variables.tf
        └── outputs.tf
```

And it would be called in my main.tf file in the root module as the below:
```
module "ec2" {
  source        = "./modules/ec2"
  instance_type = "t3.micro"
  region        = var.region
}
```

Then within the child module directory it would be defined as this
```
resource "aws_instance" "this" {
  ami           = "ami-123456"
  instance_type = var.instance_type
}
```
**Key thing to note when it comes to calling a child module is that it is called in the root directory meaning you could have a main.tf file in root and call it within that file as the below**

```
project/
├── main.tf          # ec2 module would be called here
└── modules/
    └── ec2/         # child module
        ├── ec2.tf # where the module is defined e.g. your ec2 configuration
        ├── variables.tf 
```

The main.tf file in root:
<img width="273" height="105" alt="image" src="https://github.com/user-attachments/assets/b5787df8-890f-45a3-a245-8eb08816e229" />

The ec2.tf file in module/ec2/:
<img width="552" height="325" alt="image" src="https://github.com/user-attachments/assets/aa8c6d95-bb30-4387-8aff-e2c60020f6cc" />


Terraform Providers
---


<img width="687" height="722" alt="image" src="https://github.com/user-attachments/assets/aefb9e4f-2bdc-4a8c-9e3b-3ca75cfdc1a0" />
