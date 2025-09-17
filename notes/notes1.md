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
