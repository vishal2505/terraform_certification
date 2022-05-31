# Terraform

## Terraform is infrasturcutre automation.

## Terraform Object Types

1. Providers
2. Resources
3. Data Sources   (associated witht the providers)

## Terraform Object Rerference

<resource_type>.<object_name>.<attribute_name>

## Workflow

terraform init  --> terraform plan  --> terraform apply

## Variables and Outputs

variable "name_label" {}

variable "name_label" {
    type = value
    descritpion = "value"
    default = "value"
    sensitive = true | false
}

Example

variable "aws_region" {
    type = string"
    default - "us-wast-1"
}

### Value Reference

var.<name_label>

var.aws_region

### Terraform Datatypes

<img src="./images/data_types.png">

#### List Example

<img src="./images/list.png">

#### Map Example

<img src="./images/map.png">

#### Local Variables Example

<img src="./images/locals_var.png">

### Supply Variable Values

1. Default Value
2. -var flag
3. -var-file flag
4. terraform.tfvars or terraform.tfvars.json
5. .auto.tfvars or .auto.tfvars.json
6. Environment variable TF_VAR_         

<img src="./images/supply_var.png">

<img src="./images/eval.png">


## Terraform Modules

Modules are a critical component of production-grade Terraform configurations. It gives infrastructure developers the ability to split infrastructure services up into separate components. For example, you can have a module for deploying EC2 Instances and a module for deploying VPCs. You can then use each module as a building block for creating entire environments in AWS.

Terraform modules are just snippets of code that can be called from within other Terraform configurations. Think of it as how functions are used from a software development perspective. Functions are used to isolate actions or processes in code, which allows them to be tested, decoupled, and reused throughout the application. In Terraform, the concept is still the same, but they are called modules. 


<img src="./images/modules.png">


## Terraform Remote state

1. Terraform state is stored locally by default in a terraform.tfstate file. With remote state, Terraform writes to a state file hosted in a remote data store. 
2. This provides a few advantages over a local state file like security, version control, and centralized storage. It also provides state locking, which is where only one person can modify the state file at a time, which prevents teammates from writing over each other. 
3. In AWS you can use an S3 bucket for storing the state file and a DynamoDB table for state locking.
4. The backend block refers to how Terraform operates with the state file. By default, the local backend is used, but creating a backend block within the Terraform configuration block tells Terraform to use an S3 bucket to store the state file.

<img src="./images/tf_remote.png">


## Importing AWS Resources into Terraform

Importing existing infrastructure into Terraform is a slow process that must be done with caution. One of the key takeaways is to understand that importing infrastructure does not create a Terraform configuration automatically. The configuration must be created manually. The typical workflow for importing existing infrastructure into Terraform is as follows:

<img src="./images/tf_wf.png">

Existing infrastructure is imported using the terraform import command, then the Terraform configuration file is modified to sync with the existing infrastructure settings. Afterward, terraform plan and terraform apply are used to verify both the Terraform configuration, state, and existing resources are in sync. 

Terraform requires the ID of the resource to be imported. In this case, the VPC ID is required since a VPC is being imported. If a Subnet were to be imported, Terraform would require a Subnet ID.


```
**main.tf**
resource "aws_vpc" "dev" {}
```

```
VpcID=$(aws ec2 describe-vpcs --region us-west-2 --filters Name=tag:Name,Values='Web VPC' --output text --query "Vpcs[].VpcId") && echo $VpcID
//vpc-078251144ebb7a53b
terraform import aws_vpc.dev $VpcID
```

<img src="./images/tf_import.png">


## Conditional Logic in Terraform Configurations

<img src="./images/tf_cond.png">

<img src="./images/tf_cond_var.png"> <img src="./images/tf_cond_main.png">


## Creating Loops in the Terraform Configuration and Scaling Resources

Dynamic blocks can be used for resources that contain repeatable configuration blocks. Instead of repeating several ebs_block_device blocks, a dynamic block is used to simplify the code. This is done by combining the dynamic block with a for_each loop inside. 

<img src="./images/tf_loop_dynamic.png">

Count allows for creating multiple instances of a resource block. Almost all resource blocks can use the count attribute. It is simply the number of times to create the resource block. It can also be used as conditional logic. In this case, the value of count is a conditional expression. If var.associate_public_ip_address is true set the count value to 1, if false set it to 0. This allows resource blocks to be created conditionally. In this example, a public IP address is not created if var.associate_public_ip_address is set to false.

<img src="./images/tf_loop_count.png">










