# 🎥[Watch Me Deploy Infrastructure with Terraform](https://www.loom.com/share/66b4aeae387a4234b4f8d1d087d7f0dd)
# Infrastructure as Code with Terraform (Azure)

**Author:** Glen Page  
**Estimated Time:** 45 Minutes  
**Difficulty:** Intermediate  


## Overview

This lab demonstrates how to deploy cloud infrastructure using **Infrastructure as Code (IaC)** with Terraform in Microsoft Azure. Instead of manually creating resources through the Azure Portal, Terraform scripts are used to automate the deployment of infrastructure components.

The lab walks through the Terraform workflow: **Initialize → Plan → Apply → Destroy**, and deploys a Resource Group, Virtual Network, Subnet and Network Security Group using Azure Cloud Shell. 🚀

## Architecture Diagram

![alt image](https://github.com/glenpagesr-dev/Infrastructure-as-code-with-Terraform-Azure-/blob/main/IAC%20Terraform.png)

---

## Environment Setup

- **Cloud Provider:** Microsoft Azure  
- **Tool:** Terraform  
- **Deployment Method:** Azure Cloud Shell (Bash)  
- **Region:** East US  

---

## Resources Created

- **Resource Group:** rg-lab04-tf-[yourname]  
- **Virtual Network:** vnet-terraform  
- **Subnet:** snet-backend  

---

## Steps Performed

### 1. Open Azure Cloud Shell

![alt image](https://github.com/glenpagesr-dev/Infrastructure-as-code-with-Terraform-Azure-/blob/main/open%20azure%20Cloud%20Shell.png)

- Logged into the **Azure Portal**.
- Clicked the **Cloud Shell icon (>_)** in the top-right menu.
- Selected **Bash** environment.
- Created a storage account when prompted.

---

### 2. Create a Working Directory

![alt image](https://github.com/glenpagesr-dev/Infrastructure-as-code-with-Terraform-Azure-/blob/main/create%20working%20directory.png)

Created a folder for the Terraform project and navigated into it.


mkdir terraform-lab
cd terraform-lab


---

### 3. Create Terraform Configuration File

![alt image](https://github.com/glenpagesr-dev/Infrastructure-as-code-with-Terraform-Azure-/blob/main/Terraform%20config.png)

Opened the built-in editor to create the Terraform configuration file.


code main.tf


---

### 4. Configure the Terraform Script

![alt image](https://github.com/glenpagesr-dev/Infrastructure-as-code-with-Terraform-Azure-/blob/main/Terraform%20Script.png)

Added the Terraform configuration to define Azure resources.


terraform {
required_providers {
azurerm = {
source = "hashicorp/azurerm"
version = "~> 3.0"
}
}
}

provider "azurerm" {
features {}
}

resource "azurerm_resource_group" "rg" {
name = "rg-lab04-tf-glenpage"
location = "East US"
}

resource "azurerm_virtual_network" "vnet" {
name = "vnet-terraform"
location = azurerm_resource_group.rg.location
resource_group_name = azurerm_resource_group.rg.name
address_space = ["10.0.0.0/16"]
}

resource "azurerm_subnet" "subnet" {
name = "snet-backend"
resource_group_name = azurerm_resource_group.rg.name
virtual_network_name = azurerm_virtual_network.vnet.name
address_prefixes = ["10.0.1.0/24"]
}


Saved the file using **Ctrl + S** and closed the editor with **Ctrl + Q**.

---

### 5. Initialize Terraform

![alt image](https://github.com/glenpagesr-dev/Infrastructure-as-code-with-Terraform-Azure-/blob/main/initialize%20terraform.png)

Initialized the Terraform working directory and downloaded the Azure provider.


terraform init


Expected output:


Terraform has been successfully initialized


---

### 6. Review the Terraform Plan

Terraform compared the code configuration with the current Azure environment.


terraform plan


Expected result:


Plan: 3 to add, 0 to change, 0 to destroy


Resources planned for creation:

- Resource Group  
- Virtual Network  
- Subnet  

---

### 7. Deploy Infrastructure

![alt image](https://github.com/glenpagesr-dev/Infrastructure-as-code-with-Terraform-Azure-/blob/main/Deploy%20infrastructure.png)

Executed the Terraform deployment.


terraform apply


Confirmed the deployment by typing:


yes


Terraform created all resources automatically in Azure.

---

### 8. Verify Deployment

![alt image](https://github.com/glenpagesr-dev/Infrastructure-as-code-with-Terraform-Azure-/blob/main/Verify%20deployment.png)

Verified resources in the Azure Portal.

Steps:

- Navigate to **Resource Groups**
- Locate **rg-lab04-tf-[yourname]**
- Open the resource group

Confirmed the following resources were created:

- Virtual Network  
- Subnet (snet-backend)

---

### 9. Modify Infrastructure (Add Network Security Group)

Reopened the Terraform configuration file.


code main.tf


Added the following resource block:


resource "azurerm_network_security_group" "nsg" {
name = "nsg-web"
location = azurerm_resource_group.rg.location
resource_group_name = azurerm_resource_group.rg.name
}


Saved the file and ran Terraform again.


terraform plan
terraform apply


Terraform detected the change and added the new resource.

---

### 10. Destroy Infrastructure

Removed all resources defined in the Terraform configuration.


terraform destroy


Confirmed by typing:


yes


Terraform deleted all deployed resources from Azure.

---

## Troubleshooting

**Issue:** Resource Group already exists  
**Fix:** Delete the existing resource group in Azure or rename it in `main.tf`.

**Issue:** Terraform syntax error  
**Fix:** Check for missing brackets `{}`, missing quotes `" "`, or incorrect formatting.

---

## Lab Outcome

This lab demonstrated how to automate Azure infrastructure deployment using Terraform and apply Infrastructure as Code principles for consistent and repeatable cloud deployments.
