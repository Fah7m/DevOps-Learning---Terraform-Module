# DevOps-Learning---Terraform-Module

Terraform Workflow
---
<img width="1684" height="803" alt="image" src="https://github.com/user-attachments/assets/00b6669c-4ba1-4d82-ab99-50ac91e8e4b0" />


Important things to note:
---
When deploying a resource like an EC2 instance to AWS, each region will have a different AMI so if you copy an AMI from eu-west-2 but are trying to deploy in eu-west-1. Then terraform will return an error as that image doesn't exist in that region.

