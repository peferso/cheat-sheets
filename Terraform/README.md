# Terraform 

## Basic introduction

[Terraform](https://www.terraform.io/) is a software that allows to configure, create and destroy cloud ressources using code (Infrastructure as Code: IaC).

The code should be written in `*.tf` files using [Hashicorp syntax](https://www.terraform.io/docs/language/syntax/configuration.html) 
which is based on [HLC](https://github.com/hashicorp/hcl/blob/main/hclsyntax/spec.md). 
Those files should be all inside the project folder and subfolders.
What Terraform does is to aggregate all the code and discover the ressources dependencies.

Terraform is executed from the command line. Let us consider the following project structure assuming some extended ["best practices"](https://www.terraform-best-practices.com/code-structure)
```sh
___ project-folder
    |___ main.tf
    |___ variables.tf
    |___ outputs.tf
```
We distinguish three files:
* `main.tf`: this file would contain the code necessary to identify the cloud ressources. This is achieved performing calls to modules: code blocks that are like black boxes with several **input** and **output** parameters.
* `variables.tf`: this file would contain the declaration of variables and their default values. Variables are used as inputs in the modules declared in `main.tf`.
* `outputs.tf`: this file would contain the declaration of variables that store outputs from modules included in `main.tf`. The outputs declared in `output.tf` will be printed after running Terraform. It is not necessary to declare all the outputs, only those that need their values to be returned.

To transform this code into infrastructure, the process is the following. With a terminal opened in `project-folder`,
1. Initiate Terraform: `terraform init`. This step will scan all the `.tf` files and download all the necessary modules and provider plugins (AWS, Azure...). It is necessary to run this command everytime a module is modified or added to the configuration.
2. Check out what the configuration will do: `terraform plan`. This step prints out the changes to the existing configuration in the cloud.
3. Implement the changes: `terraform apply`. This step prints again the changes and asks for confirmation to create the ressources. You can omit the confirmation step using `terraform apply -auto-approve`.

The first time that a `terraform apply` is performed, Terraform generates several files on the project folder. 
One of these files is `terraform.tfstate`, which contains the information of all the ressources created, i.e., the actual picture of the cloud world.
Now, one could modify the code of the `.tf` files.
The next time that an `apply` or `plan` command is used, 
Terraform will use the `terraform.tfstate` information to print out the changes to the cloud ressources with respect to the configuration contained in the updated local `.tf` files.

* To destroy all the ressources: `terraform destroy`.
* To retrieve the values of the output variables declared in `outputs.tf`: `terraform refresh`

## Terraform syntax

As already mentioned, Terraform code consists mainly in calls to modules and variable declarations. Inputs and outputs of modules can be parametrized and used throughout the configuration. Let us consider the following basic code of a `main.tf` file:
```hlc
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 2.70"
    }
  }
}

provider "aws" {
  region  = "eu-west-3"
  access_key = "AKFAQNOK7RH4RAFKTWWJ"
  secret_key = "GGlxnpdnnQzGAlKm4LThSjQohZ8kP1IRimUdGzfd"
}
```
It consists of two blocks, which are native Terraform modules necessary to specify the cloud provider, [AWS](https://registry.terraform.io/providers/hashicorp/aws/latest/docs) in this case. We can also appreciate how input parameters are hardcoded. Note that an AWS region, public and private access keys are needed (see the [CLI section in AWS](../AWS/README.md) for information regarding these credentials). 

### Variables

We can use variables to feed some of these values to the _provider_ module:
```hlc
provider "aws" {
  region  = var.my_region
  access_key = var.my_public_key
  secret_key = var.my_secret_key
}
```
given that, in the `variables.tf` file, the variables are declared as follows:
```hcl
variable "my_region" {
  type        = string
  description = "The AWS region where resources will be created"
  default     = "eu-west-3"
}

variable "my_public_key" {
  type        = string
  default     = "AKFAQNOK7RH4RAFKTWWJ"
}

variable "my_secret_key" {
  type        = string
  default     = "GGlxnpdnnQzGAlKm4LThSjQohZ8kP1IRimUdGzfd"
}
```
At this step, the variables take their values from the _default_ entry in each _variable_ block.
It is not recommended to hard code credentials in the configuration. 
We can take advantage of this issue to introduce now **environment variables**. 
Terraform scans the existing system environment variables and takes those ones whose names satisfy the pattern `TF_VARS_*`. 
Thus, we could create in our system two variables: `TF_VARS_my_public_key` and `TF_VARS_my_secret_key` with the necessary values (see [Environment variables in the bash cheatsheet](../bash/README.md)).
In this way, we could have the following `variables.tf` file:
```hcl
variable "my_region" {
  type        = string
  description = "The AWS region where resources will be created"
  default     = "eu-west-3"
}

variable "my_public_key" {
  type        = string
}

variable "my_secret_key" {
  type        = string
}
```
Let us introduce the `terraform.tfvars` file. Until now, we have two ways to feed variable values: _via_ their default value in the declaration `variables.tf` file or _via_ environment variables for those critical or secret ones. A variable without _default_ value is interpreted as mandatory, and must be fed somehow or there will be an error. Adding a `terraform.tfvars` file to the configuration, we can centralise values declarations in one file:
```hcl
my_region = "eu-west-3"
my_subnet_id = "subnet-0e58bf84280768750"
my_security_group_SSH_laptop = "sg-0a115751499efa539"
```
Finnally, we can feed variable values in the command line:
```sh
terraform apply -var="image_id=ami-abc123"
```
The values in the `terraform.tfvars` file, or given in any other way, will override the default ones. In the case of giving values to the same variable using simultaneously some of these methods, Terraform loads the values in the following order overriding the old ones:
1. Environment variables
2. `terraform.tfvars` file if present
3. `terraform.tfvars.json` file if present
4. Any `*.auto.tfvars` or `*.auto.tfvars.json` files, processed in lexical order of their filenames.
5. Any `-var` and `-var-file` options on the command line, in the order they are provided.

For more info on this topic, check out the [official documentation](https://www.terraform.io/docs/language/values/variables.html). 

### Outputs

Outputs are similar to variables, they must be declared in the configuration in order to be printed after running `terraform apply` or `terraform refresh`. 
Let us continue with our project structure, and lets place them in a file called `outputs.tf`. Suppose that in the `main.tf` file we have included this piece of code:
```hlc
...

resource "aws_instance" "database_ec2" {
  ami                         =  var.amiType
  instance_type               =  var.insType
  subnet_id                   =  var.ec2SubNt
  vpc_security_group_ids      = [var.secGrpID]
  associate_public_ip_address =  var.publicIP
  user_data                   =  file(var.pathToUD)
  key_name                    =  var.keyName
}

```
With this piece of code we are incorporating a module: the native ressource [`aws_instance`](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance#argument-reference), which could be thought as a primary or elementary module. 
>In general, a module can be described as a structure that itself one or more ressources and/or modules. 
This code creates an EC2 instance, the ressource is called "aws_instance" and we give it the name "database_ec2". 
We could invoke this module more times but writing it with a different name. 

In this example we can see that we are providing 7 input values using variables. 
But in Terraform, modules usually have both input and output variables.
In the documentation of the native [`aws_instance`](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance#argument-reference) module we can see the possible [outputs](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance#attributes-reference) (in the case of ressources they are called "attributes", but lets call them outputs). 
Lets think that we are interested in printing out the value of the private IP of the instance once it is created, well, there is an output value for this: `private_ip`. 
In order to visualize its value on the terminal after running the `init` or `refresh` commands, we could add the output `private_ip` to the `outputs.tf` file:
```hlc
output "database_ec2_private_ip" {
  description = "Private IP of the database EC2 instance"
  value       = aws_instance.database_ec2.private_ip
}
```
The value would be printed together with the rest of the outputs declared as follows:
```sh

Apply complete! Resources: 13 added, 0 changed, 0 destroyed.

Outputs:

database_ec2_id = "i-00df1d82b4c10345a"
database_ec2_private_ip = "10.10.1.202"
database_ec2_state = "running"
```

Besides printing out information, outputs can be used in other modules/ressources. A case of use may be providing the `id` of the instance as input to another module in `main.tf`:
```hlc
module "another_module" "name" {
  ...
  input_par_1 = aws_instance.database_ec2.private_ip
  ...
}
```

***

Return to **[main page](../README.md)** 
