# Terraform


## Infrastructure as code
Benefits
1. Version Controlled and can be stored in a repository alongside application code
2. Programatically deploy infrastructure
3. The reduction of misconfigurations that could lead to security vulnerabilities asnd unplanned downtime
4. Standardize your deployment workflow

Reducing vulnerabilities in your publicly-facing applications is **NOT** a benefit of using IaC since IaC is geared towards deploying infrastructure and applications, but not determining whether your application is secure.


### Terraform is infrasturcutre automation.

### Terraform is available for macOS, FreeBSD, OpenBSD, Linux, Solaris, and Windows. There is no Terraform binary for AIX.

## Terraform Object Types

1. Providers
2. Resources
3. Data Sources   (associated witht the providers)


## Format of Resource Block Configuration

The format of resource block configurations is as follows:

```<block type> <resource type> <local name/label>```

   
## Terraform Object Rerference

<resource_type>.<object_name>.<attribute_name>


## Terraform Newer Version Update

From 0.13 onwards, Terraform requires explicit source information for any providers that are not HashiCorp maintained, using a new syntax in the **required_providers** nested block inside the terraform configuration block.

### HasiCorp Maintained
```
provider "aws" {
  region     = "us-west-2"
  access_key = "PUT-YOUR-ACCESS-KEY-HERE"
  secret_key = "PUT-YOUR-SECRET-KEY-HERE"
}
```

### Non-HasiCorp Maintained
```
terraform {
  required_providers {
    digitalocean = {
      source = "digitalocean/digitalocean"
      version = "2.5.0"
    }
  }
}

provider "digitalocean" {
  token = "PUT-YOUR-TOKEN-HERE"
}
```

## Variable types in Terraform
   
1. string
2. number
3. bool
4. list (or tuple)
5. map (or object). 
   
## Functions in Terraform

## String Functions

1. replace
2. join
3. format

## Type conversion Functions

**tostring** is not a string function, it is a type conversion function. tostring converts its argument to a string value.
   
   
## Workflow

terraform init  --> terraform plan  --> terraform apply


## Terrfaomr CLI Commands


1. **terraform init** - providers/plugins are downloaded to .terraform/providers and modules are downloaded to the .terraform/modules directory.
2. terrafrom plan
3. terraform apply
    **replace -** If you untent to force replacement of a particular object evern though there are no configuraiton changes use -replaxe option with terraform apply.
    for Example: terraform apply -replace="aws_instance.example[0]"
4. **terraform destroy** - To destroy a specific resource use -target flag.
5. **terraform providers** - Used to find what provider name you are dpeloying
6. **terraform providers Schema**  - print detialed schema for the providers used in the current configuration.
7. **terraform taint** - Deprecated now in terrafornm version 0.15.2. and higher. The terraform taint command informs Terraform that a particular object has become degraded or damaged. Terraform represents this by marking the object as "tainted" in the Terraform state, and Terraform will propose to replace it in the next plan you create.
8. **terraform login [hostname]** - login into terraform enterprise. Saves the api token
9. **terraform logout** - logout from terraform cloud. Used to remove credentials stored by terraform login.
10. The **terraform refresh** command updates state data to match the real-world condition of the managed resources. This is done automatically during plans and applies, but not when interacting with state directly.
11. **terraform apply -refresh-only** To instruct Terraform to refresh the state file based on the current configuration of managed resources, you can use the terraform apply -refresh-only command. If Terraform discovers drift, it will update the state file with the changes.

Note that terraform refresh used to be the correct command here, but that command is deprecated. It might show up on the exam though.

## Terrafrom state commands 

1. The **terraform state list**  command shows the resource addresses for every resource Terraform knows about in a configuration, optionally filtered by partial resource address.

2. The **terraform state show** command displays detailed state data about one resource.


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

A local value assigns a name to an expression, so you can use the name multiple times within a module instead of repeating the expression.

```
locals {
  # Ids for multiple sets of EC2 instances, merged together
  instance_ids = concat(aws_instance.blue.*.id, aws_instance.green.*.id)
}

locals {
  # Common tags to be assigned to all resources
  common_tags = {
    Service = local.service_name
    Owner   = local.owner
  }
}
```
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
   
1. Modules repositories must use this thre-part name format - ```terraform-<provider>-<name>```
2. Registeries uses tag to identify module versions. Release tag names must be in the format x.y.z and can optionally be prefixed with a **v**. 
3. The module must be on GitHub and must be a public repo. This is only a requirement for the public registry. If you're using a private registry, you may ignore this requirement.
4. Both the **terraform get** and **terraform init** commands will install and update modules. The terraform init command will also initialize backends and install plugins.


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



## Seninel - Policy as Code

### Benefits
1. Sandboxing
2. Codification
3. Version Control
4. Testing
5. Automation

Sentinel Policy is applied after Terraform Plan and Before Terraform Apply.


## Terraform Backends

Backends define where Terraform's state snapshots are stored.

### Enhanced
    Enhacned backends can both store state and perform operations. There are only 2 enhanced backends: local and remote.

### Standard
    Standard backends only store state and rely on local backend for performaing operations


## Terraform Provisioners

### local-exec provisioner

The local-exec provisioner invokes a local executable after a resource is created. This invokes a process on the machine running Terraform, not on the resource

```
resource "null_resource" "example1" {
  provisioner "local-exec" {
    command = "open WFH, '>completed.txt' and print WFH scalar localtime"
    interpreter = ["perl", "-e"]
  }
}
```

### remote-exec provisioner
The remote-exec provisioner invokes a script on a remote resource after it is created. This can be used to run a configuration management tool, bootstrap into a cluster, etc.
The remote-exec provisioner requires a connection and supports both ssh and winrm.
```
resource "aws_instance" "web" {
  # ...

  # Establishes connection to be used by all
  # generic remote provisioners (i.e. file/remote-exec)
  connection {
    type     = "ssh"
    user     = "root"
    password = var.root_password
    host     = self.public_ip
  }

  provisioner "remote-exec" {
    inline = [
      "puppet apply",
      "consul join ${aws_instance.web.private_ip}",
    ]
  }
}
```

## Retrieving data from data sources
   
It is important to consider that Terraform reads from data sources during the **plan** phase and writes the result into the plan. For something like a Vault token which has an explicit TTL, the apply must be run before the data, or token, in this case, expires, otherwise, Terraform will fail during the apply phase.
   
Another example of this is AWS credentials:

The token is generated from the moment the configuration retrieves the temporary AWS credentials (on terraform plan or terraform apply). If the apply run is confirmed after the 120 seconds, the run will fail because the credentials used to initialize the Terraform AWS provider has expired. For these instances or large multi-resource configurations, you may need to adjust the default_lease_ttl_seconds.

https://learn.hashicorp.com/tutorials/terraform/secrets-vault#provision-compute-instance
   
   
## Terraform Cloud supports the following VCS providers as of January 2022:

  - GitHub

  - GitHub.com (OAuth)

  - GitHub Enterprise

  - GitLab.com

  - GitLab EE and CE

  - Bitbucket Cloud

  - Bitbucket Server

  - Azure DevOps Server

  - Azure DevOps Services


## Terraform Enterprise
   
1. A Terraform Enterprise install that is provisioned on a network that does not have Internet access is generally known as an **air-gapped install**. These types of installs require you to pull updates, providers, etc. from external sources vs. being able to download them directly.
2. Single Sign-On is a feature of Terraform Enterprise and Terraform Cloud for Business. It is NOT available in Terraform Cloud (free tier).


