# DevOps-Learning---Terraform-Module

Terraform Workflow
---
<img width="1684" height="803" alt="image" src="https://github.com/user-attachments/assets/00b6669c-4ba1-4d82-ab99-50ac91e8e4b0" />


Terraform interview questions
---
**What is terraform and how does it work?**

Terraform is an open source infrastructure as code tool developed by HashiCorp. Also mention the difference between infra orchestration and config management and throw in terms such as cloud agnostic.

Mentioning how it allows you to define, provision and manage infra across various cloud providers shows your understand of the tool and futhermore mentioning how terraform is idempotent and the benefits around that.

**What are benefits of using IAC**
IAC providers automation consistency and repeatability in infrastructure management. It enables version control, reduces manual intervention and allows for faster and safer deployments.

**What is the difference between plan and apply**
Terraform plan outputs your desired state and the actions needed in order to achieve that desired state. And Terraform Apply simply applies that.

Extra points if you mention how plan makes that comparison between desired state and actual state and sees what changes need to be implemented and almost predcits the future.

Using analogies can help by saying something like plan is like a dry run and apply is where it actually applies changes to the infra and it's what executes the plan.

**What is a terraform provider**
A provider is a plugin in terraform that interacts with APIs of cloud platforms, SAAS providers and other services to manage resources for example AWS. Can talk about how that provider allows that connection and communication between Terraform configuration and to that hosted cloud platform.

**Explain the role of state in Terraform**
Terraform keeps track of your infra state via a state file. This file records all the resources that terraform manages allow it to know what changes need to be applied when you run commands like apply or destroy. Essentially the state file is like the blueprint of your infra taking notes of everything and keeping track of your infra. Without the state file, you wouldn't be able to do actions you need to do.

**What is the purpose of a backends in Terraform**
So a backend defines where and how the Terraform state is is stored. Backends can be stored in state files locally or remotely and they enable collaboration and versioning of the state. 

Good to mention the use cases of remote and local state files. 

Remote state files would be beneficial when collaborating with others, where you'd be using a remote backend like s3 and that allows for that terraform colaboration. Extra brownie points if you mention **Terraform Locking**

**What is the difference between Terraform import and Terraform init**
They're actually both very different commands. So don't be thrown off by interview questions where you think, you know, something might closely relate to each other. But that's far from the truth. Some interview questions can be something that throws you off completely. I've been there in the past. 

Terraform import brings the existing infrastructure that was not created by Terraform under Terraform's management. So you can simply answer that as that is. But to take it the extra mile, you want to talk about potentially working in a production environment or when a use case for Terraform import will be important. For example, you join a company that might be an early-stage startup where they've deployed things manually to the cloud and they haven't followed best practices. Instead of ripping everything down and deploying everything again, which can cause a lot of downtime, cost a lot of money, you could import existing resources into your Terraform configuration so you can now manage that within Terraform. So you could mention those benefits there.

Terraform init, that initializes the working directory containing your Terraform configuration files, downloads the required plugins, and also initializes any modules that you've configured.

**How do you manage sensitive data in terraform**
A good way in order to manage sensitive data would be storing sensitive data in env variables. Sensitive data like API keys, passwords and credentials should always be handled very carefully and you don't want to be causing any risks.

Utilising a remote backend helps with this as if you're using AWS S3 to store your state file, you could then use AWS secret management to store your secrets and sensitive data.

**What is the purpose of Terraform refresh**
Terraform refresh updates the state file to match the current real-world infra. It queries the actual infra to ensure terraform state is in sync with the latest reality. This is important to keep your terraform state file in line with your actual infra. This will ensure that when you run your terraform plan that it will actually be going according to plan and not deploying anything it shouldn't or deleting anything it shouldn't 

**Can you describe a challenging problem you encountered while using Terraform and how you solved it**
Interviewers can ask these sorts of questions to gain your problem-solving skills. You want to showcase this opportunity to talk about projects you worked on. An additional tip would be sharing your screen. Actually going through code that you've deployed, saying what problems and blockers you experienced and how you went about resolving it is super impressive to a lot of technical interviewers. It's an opportunity to showcase your skills and separate you from others and also explain code. Being able to explain code and digest it simply is again a massive advantage.

**how do you ensure your terraform code is maintainable and scalable**
This is an opportunity to talk about things like modules to break down your infrastructure. We spoke about modularity, how you want to make sure that modules are very specific, leveraging variable files for flexibility, maintaining documentation, using version control. This is all opportunities for you to talk about how you maintain and scale your code.

**Have you worked with remote state management**
This is important. These questions will help you prepare for Terraform-related interviews and reinforce your knowledge. Remember, in addition to understanding the technical details, interviewers often focus on problem-solving, best practices, and your ability to think critically about how to use Terraform effectively in real-world scenarios. So in real-world scenarios, things like security should come to mind, sensitive data, remote backend for collaboration. These are all things that will separate you from other candidates and mention what Terraform looks like in a production environment.

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

<img width="563" height="313" alt="image" src="https://github.com/user-attachments/assets/4b20ee9f-3a3d-4126-b473-bc634070e328" />


**Variable types**

<img width="1863" height="917" alt="image" src="https://github.com/user-attachments/assets/86486633-24fa-44ff-b408-17d1970aa8a8" />

