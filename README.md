# DevOps-Learning---Terraform-Module

Terraform Workflow
---
<img width="1684" height="803" alt="image" src="https://github.com/user-attachments/assets/00b6669c-4ba1-4d82-ab99-50ac91e8e4b0" />


Important things to note:
---
When deploying a resource like an EC2 instance to AWS, each region will have a different AMI so if you copy an AMI from eu-west-2 but are trying to deploy in eu-west-1. Then terraform will return an error as that image doesn't exist in that region.

**tfvar and Var.tf files**

A variables.tf file is where you declare your variables and can have them answered such as setting a default in the file or other values like desctiption etc. 

The ,tfvar file is where you can assign values to those variables instead of setting defaults within the variables.tf file as that can be very long or setting it in the command line if no default is set. Using this makes things much more organized and repeatable and safe. 

So here we can see I have set a value in the tfvar file for instance_type variable

<img width="587" height="136" alt="image" src="https://github.com/user-attachments/assets/495ad386-7084-4617-87f6-02b8760bb93b" />

And here where can see that there is no default set in the variables.tf file however even if there was it would still take what's in the tfvar file.

<img width="598" height="138" alt="image" src="https://github.com/user-attachments/assets/1c4841f4-97cc-4ab0-be2f-6deffbe204d1" />

**The flow:**

If you set a default in variables.tf, that’s only used when nothing else provides a value.

If you also have a value in a *.tfvars file, the tfvars value will override the default.

If you pass something with -var on the CLI, that will override both.
A variables.tf file is used to declare variables and the tfvar file can be used to assign values to those declared variables instead of having to 

