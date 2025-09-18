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


Terraform Providers
---


<img width="687" height="722" alt="image" src="https://github.com/user-attachments/assets/aefb9e4f-2bdc-4a8c-9e3b-3ca75cfdc1a0" />
