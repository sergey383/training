## Terraform how to install Windows
```
https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli
```

## Terraform Cheat Sheet

Show version:
```
terraform –version	
```
Initialize infrastructure:
```
terraform init
```
Provision infrastructure:

Creates an execution plan (dry run)
```
terraform plan
```
Executes changes to the actual environment
```
terraform apply	
```
Destroy/cleanup
```
terraform destroy
```

## Introduction:
Terraform is an open-source infrastructure provisioning tool created by HashiCorp, it allows you to define and manage your infrastructure using a declarative language which is written in GO language.

Terraform allows you to create, modify, and destroy infrastructure resources across different cloud providers, such as AWS, Azure, GCP, etc. It describes the desire state of the infrastructure on the premises environment.

Benefits of Using Terraform:

It supports multiple cloud providers such as AWS, Azure, Oracle, GCP, and more.
- Terraform configuration is written in easy-to-understand language.
- It simplifies the management and orchestration of multi-tier infrastructure.
- Terraform modules allow you to spate resources in dedicated and reusable templates.

Terraform Lab setup:
To make a terraform lab setup we have to install the following pre-requisites:

Download & install terraform.
IDE has used VS code editor can be downloaded.
Terraform Extensions for VS Code Editor:
- Terraform Autocomplete
- AWS Tool Kit
- Simple Terraform Snippets
- AWS CLI

Configure AWS Credentials:

- AWS Account
- Generate Security Credentials
- Configure in AWS CLI

Terraform Configuration
- Terraform script/code are stored in plain text file .tf extension

- The file that contains terraform code is called a configuration file or terraform manifest.

- You should know you're working directory before executing the code, whenever you execute the terraform code you should be in the working directory where the configuration file is available.

Terraform Configuration Syntax
```
<BLOCK TYPE> "<BLOCK LABEL>" "<BLOCK LABEL>"   {
  # Block body
  <IDENTIFIER> = <EXPRESSION> # Argument
}
```

Blocks and Arguments:
Blocks are like a container to define other contents of your infrastructure.
An argument assigns a value to a particular name: argument name and argument value.
Argument names, block type names, and the names of most Terraform-specific constructs like resources, input variables, etc. are all identifiers.

![image](https://github.com/nshhARCHIVE/training/blob/main/img/image.png)

Registry has all the reusable components for providers and modules. For more information check the website registry. It provides official, verified, and community tier providers for free in terraform configuration.

Providers are plugins that implement resource types. It’s a logical abstraction of an upstream API. They are responsible for understanding API interactions and exposing resources.

Modules make your work easy, as we can just plug in the common modules to our terraform code. Modules are self-contained packages of Terraform configurations that are managed as a group.

Terraform Workflow
The workflow consists of lifecycle stages which start with the init, plan, apply, and destroy. All these have been executed as a command for the written code to update or changes in the infrastructure built.


![image](https://github.com/nshhARCHIVE/training/blob/main/img/image-1.png)

Terraform Init

- Terraform initializes the working directory containing terraform configuration files.
- Terraform initializes downloads and installs the provider’s plugin so that it can later be executed.
- Terraform has created a lock file .terraform.lock.hcl to record the provider selections it made above. And also, it initializes the backend configuration.

Terraform fmt

- Terraform format command is used to rewrite Terraform configuration files to a canonical format and style.

Terraform Validate

- The terraform validate command validates the configuration files in a directory.

- Validate command to verify whether a configuration is syntactically valid and thus primarily useful for general verification of reusable modules, including the correctness of attribute names and value types.

- It can run before the terraform plan. Validation requires an initialized working directory with any referenced plugins and modules installed.

Terraform Plan

- Terraform plan command is used to create an execution plan. It will not modify things in infrastructure. It compares the desired terraform state with the current state in the cloud. It doesn’t change the deployment.

- This command is a convenient way to check whether the execution plan for a set of changes matches your expectations without making any changes to the state

Terraform Apply

- It applies the changes required to reach the desired state of the configuration. This command executes the plan that is already created. It compares the correct state with desire state.

- It will write data into terraform.tfstate file. Once the application is completed, resources are immediately available.

Terraform Destroy

- The Terraform destroy command is used to delete the created resources in the Terraform-managed infrastructure.

Terraform Block — Here is the required terraform version, list of providers required, and terraform backend.
```
terraform {
required_providers {
aws = {
source = "hashicorp/aws"
version = "5.0.1"
}
````

Provider Block — It declares providers for Terraform to install the providers which is going to be used. Terraform relies on the providers to interact with remote systems. Provider configuration belongs to the root module.
```
provider "aws" {
region = "us-east-1"
profile = "default"
}
```

Resource Block — Each resource block describes one or more infrastructure objects. Each resource block is an infrastructure object, like compute instances, virtual networks, or other components.
```
resource "aws_instance" "webserver" {
ami = "ami_id"
instance_type = "t2.micro"
}
```

Let us apply the above terraform blocks in configuration and execute commands.

Creating an EC2 Instance on AWS using Terraform:
![image](https://github.com/nshhARCHIVE/training/blob/main/img/image-2.png)

![image](https://github.com/nshhARCHIVE/training/blob/main/img/image-3.png)

![image](https://github.com/nshhARCHIVE/training/blob/main/img/image-4.png)

![image](https://github.com/nshhARCHIVE/training/blob/main/img/image-5.png)

![image](https://github.com/nshhARCHIVE/training/blob/main/img/image-6.png)

![image](https://github.com/nshhARCHIVE/training/blob/main/img/image-7.png)

![image](https://github.com/nshhARCHIVE/training/blob/main/img/image-8.png)

Now let us discuss the important files created during execution:

1. Dependency Lock File:
- The dependency lock file belongs to the configuration as a whole, rather than to each separate module in the configuration

- The lock file is named as .terraform.lock.hcl and available in the subdirectory of .terraform of your working directory.

- It automatically gets created when we execute the terraform init command to record the provider so that terraform can make the same selections by default when you run terraform init in the future.

![image](https://github.com/nshhARCHIVE/training/blob/main/img/image-9.png)

2. Terraform State File:
- Terraform state stores the infrastructure information and its configuration. The state file maintains a record of the resources created by your configuration and maps them to real-world resources.

- The state file is stored in JSON format in the name of terraform.tfstate by default in our local workspace. Also, terraform maintains a backup file for terraform state which is named terraform.tfstate.backup

- Whenever we execute terraform apply a new terraform.tfstate file will be created and its backup will be written in terraform.tfstate.backup file.

- The state file can be stored/maintained remotely in various methods like using the version control tool Git, Amazon S3 as remote store and also in terraform cloud, enterprise, and consul.

![image](https://github.com/nshhARCHIVE/training/blob/main/img/image-10.png)

Terraform State commands:
These are the list of terraform state commands used frequently.

terraform state list: It lists the resources within the state file.

terraform state mv : Move items within terraform state. This will be used for resource renaming without destruction.

terraform state pull: It manually downloads and outputs the state from the state file.

terraform state push: Manually upload a local state file to the remote state

terraform state rm: It removes items from the state & no longer managed by Terraform. The resources removed from the state are not physically destroyed.

terraform state show: Show attributes of a single resource in the state.

What Is the Current State and Desire State in Terraform
Current state
Current state is the present state of the resources deployed, stored locally as terraform.tfstate file and not in remote infrastructure.

After creating the infrastructure, any manual changes made in the configuration will not reflect in the current state file.

In the next execution of the terraform apply command, it will make all the infrastructure changes manually and keep the infrastructure the same in its current state.

Desire state
Desire state is the new state expected to apply from your terraform configuration.

It compares with the current state every time to update the infrastructure.

It will detect the changes between the desired state and the current state and try to ensure that the deployed infra is based on the desired state.

If there is a difference between the two, the terraform plan will show the necessary changes required to achieve the desired state.

Types of Terraform state store:
There are two types of stores for Terraform state: local and remote.

Local state refers to the Terraform state stored on the local filesystem, i.e. on your laptop or whatever system you are running the Terraform command from.

Remote state is Terraform state stored remotely, such as in an S3 bucket or a database like PostgreSQL.

Terraform Variables:
Terraform variables store the values which can be reusable in terraform configuration. The functions of variables are as follows:

Input Variables as function arguments, so users can customize behavior without editing the source.

Output Values are like return values for a Terraform module.

Local Values are a convenience feature for assigning a short name to an expression.

![image](https://github.com/nshhARCHIVE/training/blob/main/img/image-11.png)

Input Variables

Input variables are declared in the variable block. It has the following arguments declared in the variable block such as name, type, default, and description.

```
Variable <variable name> {
description = "updating instance type" - - - Give a meaningful description
type = string - - - - - - - - - - - - - ex: string, number, bool, list, map
default = "t2.micro" - - - - - - - - - - variable default value
```

Let’s create a few simple variable files, the type of value of the variables like string, number, or bool.

String — A sequence of Unicode characters representing some text
Number — A numeric value that includes either a whole number or a fractional value.
Bool — It's either TRUE or FALSE. A Boolean value.

![image](https://github.com/nshhARCHIVE/training/blob/main/img/image12.png)


Next check on the complex variable types of variables like Map and list are collection type.

Maps
Maps are associative arrays used for situations where you want to use one value to look up another value. We can take the example, AMI for creating EC2 instances should be taken as region-specific.

![image](https://github.com/nshhARCHIVE/training/blob/main/img/image13.png)

To change the region, we need to look up a new AMI using the input variable Map of strings.

List
Each value can be called by its corresponding index in the list. Here is an example of a list variable definition. Also in the list, we need to denote the index value that you are looking for.

![image](https://github.com/nshhARCHIVE/training/blob/main/img/image14.png)

Assigning Values to Input Variables:
There are multiple ways to assign values for input variables as listed below.

- In command-line option

- In variable definitions files

- As an environment variable

Command-line Option:

To mention the specific individual variables on the command line, we can use the –var option while running the terraform plan and terraform apply commands. Ex: We will specify a region on the command line.
```
terraform plan -var 'aws_region=us-west-1'
```

Same as can do an authentication of the access key and secret key in the command line as given below:
```
terraform apply –var="AWS_ACCESS_KEY=<Access key>"
terraform apply –var="AWS_SECRET_KEY=<Secret key>"
```

CLI Variables:
We can mention the same file in the command line option using –var-file in “terraform apply” as follows:
```
terraform apply -var="image_id=ami-xyz234"
```

Variable Definition Files Option:

Tfvars file:
To set many variables by specifying their values in a single variable file will be convenient that we can use the variable definition file ending in .tfvars. The file created is named terraform.tfvars or terraform.tfvars.json as shown below.
```
terraform apply –var-file="sample.tfvars"
```

auto tfvars files:

Another file name is given as .auto.tfvars or .auto.tfvars.json . The file .auto we add will be loaded automatically without specifying the –var-file flag.

Environment Variables:
Terraform by default searches environment variables named TF_VAR_ followed by the name of the declared variable. The environment variable names are case-sensitive, they match the variable name exactly as given in the configuration.

```
Export TF_VAR_AWS_REGION=us-east-1
```

Output Values:
Output values make information about your infrastructure available on the command line and can expose information for other Terraform configurations to use. Output values are similar to return values in a programming language.

Local Values:
Terraform local values are like temporary local variables. It’s an expression name assigned by local values so that we can use it multiple times within the same modules. It helps to avoid repeating the same values in a module configuration.

![image](https://github.com/nshhARCHIVE/training/blob/main/img/image15.png)


## Code Structure

- Our code strtucture use Directories model [Advantage Over Woskpaces](https://learn.hashicorp.com/tutorials/terraform/organize-configuration#:~:text=your%20infrastructure%20best.-,Directories,-Workspaces).
- S3 bucket configured to store states and DynamoDB table manage locks 


example: 

each S3 bucket depending on the environment region will have an S3 bucket, in each S3 bucket will have a folder with such as dev/ or stage/ or prd/ etc which will have a remote state file. 

project01-us-east-1-terraform-remote-state<br>
  dev/<br>
  stage/<br>
  prd/<br>
  ......

project02-us-west-2-terraform-remote-state<br>
  dev/<br>
  stage/<br>
  prd/<br>
  ......


