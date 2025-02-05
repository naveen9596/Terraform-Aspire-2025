# **Terraform Modules Guide**  
Terraform **modules** allow you to **reuse** and **organize** infrastructure code efficiently. They help break down configurations into **smaller, reusable components**.  

---

## **1️⃣ What is a Terraform Module?**
- A **module** is a collection of Terraform files (`.tf` files) grouped together.
- A **module** consists of:
  - **`main.tf`** → Defines resources.
  - **`variables.tf`** → Defines input variables.
  - **`outputs.tf`** → Defines output variables.
- The **root module** is the main Terraform configuration, while **child modules** are reusable.

---

## **2️⃣ Why Use Terraform Modules?**
✅ **Reusability** – Write once, use multiple times.  
✅ **Scalability** – Manage large infrastructures efficiently.  
✅ **Maintainability** – Organize code into logical components.  
✅ **Standardization** – Enforce best practices across teams.  

---

## **3️⃣ Creating a Terraform Module (Lab)**
### **🛠 Lab: Creating a Simple AWS EC2 Module**
### **Step 1: Create a Module Directory**
```sh
mkdir terraform-modules && cd terraform-modules
mkdir ec2-module
```
Your directory structure should look like this:
```
/terraform-modules
  ├── main.tf  (calls the module)
  ├── ec2-module/  (module directory)
      ├── main.tf
      ├── variables.tf
      ├── outputs.tf
```

---

### **Step 2: Define the EC2 Module (`ec2-module/main.tf`)**
```hcl
resource "aws_instance" "web" {
  ami           = var.ami_id
  instance_type = var.instance_type

  tags = {
    Name = var.instance_name
  }
}
```

---

### **Step 3: Define Variables (`ec2-module/variables.tf`)**
```hcl
variable "ami_id" {
  description = "The AMI ID for the EC2 instance"
  type        = string
}

variable "instance_type" {
  description = "The instance type"
  type        = string
  default     = "t2.micro"
}

variable "instance_name" {
  description = "The name of the EC2 instance"
  type        = string
}
```

---

### **Step 4: Define Outputs (`ec2-module/outputs.tf`)**
```hcl
output "instance_id" {
  value = aws_instance.web.id
}

output "public_ip" {
  value = aws_instance.web.public_ip
}
```

---

### **Step 5: Use the Module in the Root (`main.tf`)**
```hcl
provider "aws" {
  region = "us-east-1"
}

module "ec2" {
  source         = "./ec2-module"
  ami_id         = "ami-0c55b159cbfafe1f0" # Update with a valid AMI
  instance_type  = "t2.micro"
  instance_name  = "MyTerraformInstance"
}

output "ec2_public_ip" {
  value = module.ec2.public_ip
}
```

---

### **Step 6: Initialize and Apply Terraform**
Run the following commands:
```sh
terraform init
terraform apply -auto-approve
```
This will:
✅ **Use the module** to create an **EC2 instance**  
✅ **Output** the instance ID and public IP  

---

## **4️⃣ Using Terraform Registry Modules**
You can also use **pre-built modules** from the **Terraform Registry**:  
🔗 [Terraform Registry](https://registry.terraform.io/)  

Example:  
```hcl
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "3.19.0"

  name = "my-vpc"
  cidr = "10.0.0.0/16"
}
```

---

## **5️⃣ Best Practices for Terraform Modules**
✅ **Keep modules small** (e.g., separate networking, compute, storage).  
✅ **Use versioning** (`source = "github.com/org/repo//module?ref=v1.0"`).  
✅ **Follow standard naming conventions**.  
✅ **Use input variables for flexibility**.  
✅ **Store modules in private Git repositories for team collaboration**.  

---

## **✅ Next Steps**
Would you like:  
1️⃣ A **multi-module Terraform project** (e.g., EC2 + VPC + S3)?  
2️⃣ **Module versioning and remote module storage (GitHub, S3)?**  
3️⃣ More **hands-on Terraform projects**?  

Let me know! 🚀
