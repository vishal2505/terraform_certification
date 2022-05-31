# Terraform Provisioner

Provisioners allow additional steps to be taken when provisioning resources in Terraform. They can provide a great solution for additional automation steps,  however, it is important to understand that provisioners introduce more complexity into the Terraform code and should be used as a last resort.

Instead of pushing the docker image through a Terraform provisioner, it would be best to use a CI/CD tool like Azure DevOps or Jenkins to perform that task and let the Terraform code handle the infrastructure. 


