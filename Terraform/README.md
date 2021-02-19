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

The first time that a `terraform apply` is performed, Terraform generates on the project folder several files. 
One of these files is `terraform.tfstate`, which contains the information of all the ressources created, i.e., the actual picture of the cloud world.
Now, one could modify the code of the `.tf` files.
The next time that an `apply` or `plan` command is used, 
Terraform will use the `terraform.tfstate` information to print out the changes to the cloud ressources with respect to the configuration contained in the updated local `.tf` files.

4. To destroy all the ressources: `terraform destroy`.
5. To retrieve the values of the output variables declared in `outputs.tf`: `terraform refresh`

## Terraform syntax



***

Return to **[main page](../README.md)** 
