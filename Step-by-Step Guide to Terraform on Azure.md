Here's a **step-by-step guide** to deploying infrastructure in Azure using **Terraform**. This will help you set up and manage resources directly from your **local machine** using **Terraform CLI**.

---

# **🚀 Step-by-Step Guide to Terraform on Azure**

### **🔹 Step 1: Install Prerequisites**
Before starting, make sure you have:
1. **Azure CLI** installed → [Download Azure CLI](https://aka.ms/installazurecliwindows)
2. **Terraform CLI** installed → [Download Terraform](https://developer.hashicorp.com/terraform/downloads)
3. **Azure Subscription** (Sign up at [Azure Portal](https://portal.azure.com))

---

### **🔹 Step 2: Authenticate with Azure**
Open a terminal (Command Prompt, PowerShell, or Bash) and log in to your Azure account:

```sh
az login
```

If you have multiple subscriptions, select the one you want to use:

```sh
az account set --subscription "YOUR_SUBSCRIPTION_ID"
```

---

### **🔹 Step 3: Create Terraform Configuration Files**
Create a new directory for your project:

```sh
mkdir terraform-azure-demo
cd terraform-azure-demo
```

Now, create the following Terraform files:

#### **1️⃣ provider.tf** (Configuring Azure Provider)
```hcl
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = ">=3.0"
    }
  }
}

provider "azurerm" {
  features {}
}
```

---

#### **2️⃣ variables.tf** (Defining Variables)
```hcl
variable "resource_group_name" {
  description = "Name of the Azure Resource Group"
  type        = string
  default     = "MyTerraformRG"
}

variable "location" {
  description = "Azure region for deployment"
  type        = string
  default     = "East US"
}

variable "vm_name" {
  description = "Virtual Machine Name"
  type        = string
  default     = "MyTerraformVM"
}
```

---

#### **3️⃣ main.tf** (Creating Azure Resources)
```hcl
resource "azurerm_resource_group" "rg" {
  name     = var.resource_group_name
  location = var.location
}

resource "azurerm_virtual_network" "vnet" {
  name                = "MyTerraformVNet"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
  address_space       = ["10.0.0.0/16"]
}

resource "azurerm_subnet" "subnet" {
  name                 = "MyTerraformSubnet"
  resource_group_name  = azurerm_resource_group.rg.name
  virtual_network_name = azurerm_virtual_network.vnet.name
  address_prefixes     = ["10.0.1.0/24"]
}

resource "azurerm_network_interface" "nic" {
  name                = "MyTerraformNIC"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name

  ip_configuration {
    name                          = "internal"
    subnet_id                     = azurerm_subnet.subnet.id
    private_ip_address_allocation = "Dynamic"
  }
}

resource "azurerm_virtual_machine" "vm" {
  name                  = var.vm_name
  location              = azurerm_resource_group.rg.location
  resource_group_name   = azurerm_resource_group.rg.name
  network_interface_ids = [azurerm_network_interface.nic.id]
  vm_size               = "Standard_B1s"

  storage_os_disk {
    name              = "myosdisk"
    caching           = "ReadWrite"
    create_option     = "FromImage"
    managed_disk_type = "Standard_LRS"
  }

  storage_image_reference {
    publisher = "Canonical"
    offer     = "UbuntuServer"
    sku       = "18.04-LTS"
    version   = "latest"
  }

  os_profile {
    computer_name  = var.vm_name
    admin_username = "azureuser"
    admin_password = "YourPassword123!"
  }

  os_profile_linux_config {
    disable_password_authentication = false
  }
}
```

---

#### **4️⃣ outputs.tf** (Displaying Output Information)
```hcl
output "resource_group_name" {
  value = azurerm_resource_group.rg.name
}

output "vm_public_ip" {
  value = azurerm_network_interface.nic.private_ip_address
}
```

---

### **🔹 Step 4: Initialize Terraform**
Run the following command to initialize Terraform and download the required Azure provider plugins:

```sh
terraform init
```

---

### **🔹 Step 5: Preview the Deployment**
Run a plan to preview what will be created:

```sh
terraform plan
```

---

### **🔹 Step 6: Apply the Terraform Configuration**
Run the apply command to create the resources in Azure:

```sh
terraform apply -auto-approve
```

Once the process completes, you will see the **resource group name and private IP of the VM** in the output.

---

### **🔹 Step 7: Verify in Azure Portal**
1. Go to **Azure Portal** → **Resource Groups**
2. Search for **MyTerraformRG**
3. Click on it and check if the **Virtual Machine**, **VNet**, **Subnet**, and **NIC** are created.

---

### **🔹 Step 8: Destroy the Resources (Optional)**
If you want to delete all the resources, run:

```sh
terraform destroy -auto-approve
```

This will remove everything created by Terraform.

---

## **✅ Summary**
- Installed Terraform & Azure CLI
- Authenticated with Azure
- Created Terraform configuration files (`provider.tf`, `variables.tf`, `main.tf`, `outputs.tf`)
- Deployed an **Azure Virtual Machine with Networking**
- Verified the resources in the **Azure Portal**
- Destroyed the resources (optional)

---

This step-by-step guide provides a solid **Terraform foundation** for **Azure beginners**. Let me know if you need any modifications or **advanced configurations like load balancers, storage, or managed databases**. 🚀
