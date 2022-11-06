# Azure Infrastructure Operations Project: Deploying a scalable IaaS web server in Azure

### Introduction
For this project, you will write a Packer template and a Terraform template to deploy a customizable, scalable web server in Azure.

### Getting Started
1. Clone this repository

2. Create your infrastructure as code

3. Update this README to reflect how someone would use your code.

### Dependencies
1. Create an [Azure Account](https://portal.azure.com) 
2. Install the [Azure command line interface](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
3. Install [Packer](https://www.packer.io/downloads)
4. Install [Terraform](https://www.terraform.io/downloads.html)

### Instructions
1. Create and apply a tagging policy
  - Run 'az login' to login to azure account from azure CLI
  - az policy definition create --name tagging-policy --rules az-rules.json --params az-param.json
  - az policy assignment create --policy tagging-policy -p '{\"author\": {\"value\": \"hiepdang\"}}' --name tagging-policy --display-name tagging-policy -e Default
  - az policy assignment list --> verify 
2. Create Packer image
  - Run 'az login' to login to azure account from azure CLI
  - Run 'az group create -l eastus -n hiepdang' to create a resource group on East US region
  - Run 'az account show --query "{ subscription_id: id }"' to get Azure subscription ID
  - Run ' az ad sp create-for-rbac --role Contributor --scopes /subscriptions/<subscription_id> --query "{ client_id: appId, client_secret: password, tenant_id: tenant }"' to create azure credentials including client_id, client_secret and tenant_id
  - Run 'export ARM_CLIENT_ID=<client_id>'
  - Run 'export ARM_CLIENT_SECRET=<client_secret>'
  - Run 'export ARM_SUBSCRIPTION_ID=<subscription_id>'
  - Run 'export ARM_TENANT_ID=<tenant_id>'
  - Run 'packer build server.json' to build Packer image
3.  Deploy Terraform template
  - Modify vars.tf to customize default value
  - Run 'terraform plan -out solution.plan' to deploy VM, then save plan file as 'solution.plan'
  - Run 'terraform apply "solution.plan"' to deploy changes

### Output
1. Tagging policy
  - az policy assignment list
![alt text](https://github.com/hiepdv4/nd082-Azure-Cloud-DevOps-Starter-Code/blob/f7ad6da54154f98735816800a0c30e75e0b7fbda/C1%20-%20Azure%20Infrastructure%20Operations/project/starter_files/assignment.jpg)
  - Test without policy
![alt text](https://github.com/hiepdv4/nd082-Azure-Cloud-DevOps-Starter-Code/blob/b8ef58e9cbe69d8a7fd1376b1132e1bf7e195ae3/C1%20-%20Azure%20Infrastructure%20Operations/project/starter_files/testimagewithouttag.jpg)
2. Create Packer image
  - Build packer image 1
![alt text](https://github.com/hiepdv4/nd082-Azure-Cloud-DevOps-Starter-Code/blob/a001858519d4e0e9f4f7e2723917e9ca8e01430e/C1%20-%20Azure%20Infrastructure%20Operations/project/starter_files/buildimage1.jpg)
  - Build packer image 2
![alt text](https://github.com/hiepdv4/nd082-Azure-Cloud-DevOps-Starter-Code/blob/a001858519d4e0e9f4f7e2723917e9ca8e01430e/C1%20-%20Azure%20Infrastructure%20Operations/project/starter_files/buildimage2.jpg)
  - Verify image
![alt text](https://github.com/hiepdv4/nd082-Azure-Cloud-DevOps-Starter-Code/blob/a204180bc236379ae36cceeaa751a00bfc8e27f1/C1%20-%20Azure%20Infrastructure%20Operations/project/starter_files/verifyimage.jpg)
3. Deploy Terraform template
  - Terraform plan
![alt text](https://github.com/hiepdv4/nd082-Azure-Cloud-DevOps-Starter-Code/blob/a204180bc236379ae36cceeaa751a00bfc8e27f1/C1%20-%20Azure%20Infrastructure%20Operations/project/starter_files/terraform-plan.jpg)
  - Terraform apply solution-plan
![alt text](https://github.com/hiepdv4/nd082-Azure-Cloud-DevOps-Starter-Code/blob/d84b2811ae450f8188ac55c17d6a1d3520034dac/C1%20-%20Azure%20Infrastructure%20Operations/project/starter_files/teraform-apply.png)
  - Verify resource build from terraform
![alt text](https://github.com/hiepdv4/nd082-Azure-Cloud-DevOps-Starter-Code/blob/a204180bc236379ae36cceeaa751a00bfc8e27f1/C1%20-%20Azure%20Infrastructure%20Operations/project/starter_files/terraformbuild.jpg)
