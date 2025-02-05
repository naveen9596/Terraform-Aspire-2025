Here’s a structured guide covering **Terraform Providers, Configuration Directory, Variables, Resource Attributes, Dependencies, and Output Variables** with relevant **labs**.

---

# **1️⃣ Using Terraform Providers**
### **What are Providers?**
- Terraform **providers** are plugins that allow Terraform to interact with cloud platforms (e.g., AWS, Azure, GCP).
- Example:  
  ```hcl
  provider "aws" {
    region = "us-east-1"
  }
  ```

### **🛠 Lab: Terraform Providers**
#### **Objective:** Configure Terraform to use AWS as a provider.
#### **Steps:**
1. Create a new directory:
   ```sh
   mkdir terraform-providers && cd terraform-providers
   ```
2. Create a file **`main.tf`**:
   ```hcl
   provider "aws" {
     region = "us-east-1"
   }

   resource "aws_s3_bucket" "my_bucket" {
     bucket = "my-terraform-s3-bucket-123"
   }
   ```
3. Initialize Terraform:
   ```sh
   terraform init
   ```
4. Apply the configuration:
   ```sh
   terraform apply -auto-approve
   ```

---

# **2️⃣ Configuration Directory**
### **Terraform Project Structure**
A typical Terraform directory structure:
```
/terraform-project
  ├── main.tf
  ├── variables.tf
  ├── outputs.tf
  ├── terraform.tfvars
  ├── provider.tf
  └── modules/
```

---

# **3️⃣ Multiple Providers**
### **Why Use Multiple Providers?**
- You may need to deploy resources across **multiple cloud providers** or **multiple accounts**.

### **🛠 Lab: Multiple Providers**
#### **Objective:** Deploy resources in both AWS and Azure.
#### **Steps:**
1. Create **`main.tf`**:
   ```hcl
   provider "aws" {
     region = "us-east-1"
   }

   provider "azurerm" {
     features {}
   }

   resource "aws_s3_bucket" "aws_bucket" {
     bucket = "aws-bucket-multiple-provider"
   }

   resource "azurerm_resource_group" "azure_rg" {
     name     = "AzureResourceGroup"
     location = "East US"
   }
   ```
2. Initialize Terraform:
   ```sh
   terraform init
   ```
3. Apply:
   ```sh
   terraform apply -auto-approve
   ```

---

# **4️⃣ Using Input Variables**
### **What are Input Variables?**
- Allow **parameterization** of Terraform configurations.
- Example:
  ```hcl
  variable "instance_type" {
    type    = string
    default = "t2.micro"
  }
  ```

---

# **5️⃣ Understanding the Variable Block**
### **Variable Types**
- **String**: `"t2.micro"`
- **Number**: `2`
- **Boolean**: `true`
- **List**: `["t2.micro", "t2.medium"]`
- **Map**: `{ instance_type = "t2.micro" }`

### **🛠 Lab: Variables**
1. Create **`variables.tf`**:
   ```hcl
   variable "region" {
     description = "AWS region"
     type        = string
     default     = "us-east-1"
   }
   ```
2. Use in **`main.tf`**:
   ```hcl
   provider "aws" {
     region = var.region
   }
   ```
3. Apply:
   ```sh
   terraform apply -auto-approve
   ```

---

# **6️⃣ Using Variables in Terraform**
### **Passing Variables**
1. **Using `terraform.tfvars`**:
   ```
   region = "us-west-2"
   ```
   Apply:
   ```sh
   terraform apply -var-file="terraform.tfvars"
   ```

2. **Passing in CLI**:
   ```sh
   terraform apply -var="region=us-west-2"
   ```

### **🛠 Lab: Using Variables in Terraform**
- Modify `terraform.tfvars`, `variables.tf`, and apply.

---

# **7️⃣ Resource Attributes**
### **What are Resource Attributes?**
- Each resource has attributes that Terraform manages.
- Example:
  ```hcl
  resource "aws_instance" "web" {
    ami           = "ami-12345678"
    instance_type = "t2.micro"
  }

  output "instance_id" {
    value = aws_instance.web.id
  }
  ```

### **🛠 Lab: Resource Attributes**
1. Apply Terraform and check output:
   ```sh
   terraform apply -auto-approve
   terraform show
   ```

---

# **8️⃣ Resource Dependencies**
### **What is Resource Dependency?**
- Resources **depend** on each other.
- Terraform automatically detects dependencies, but you can **explicitly define them**.

Example:
```hcl
resource "aws_instance" "web" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"
}

resource "aws_s3_bucket" "bucket" {
  bucket = "my-bucket"
  depends_on = [aws_instance.web]  # Ensures the instance is created first
}
```

### **🛠 Lab: Resource Dependencies**
1. Modify a resource to depend on another and observe Terraform’s execution order.

---

# **9️⃣ Output Variables**
### **What are Output Variables?**
- Display key information after deployment.
- Example:
  ```hcl
  output "instance_public_ip" {
    value = aws_instance.web.public_ip
  }
  ```

### **🛠 Lab: Output Variables**
1. Add an output variable in **`outputs.tf`**.
2. Apply Terraform and check the output:
   ```sh
   terraform output
   ```

---

# **✅ Summary**
| Concept | Lab |
|---------|----|
| Terraform Providers | ✔️ |
| Multiple Providers | ✔️ |
| Using Input Variables | ✔️ |
| Variable Blocks | ✔️ |
| Using Variables | ✔️ |
| Resource Attributes | ✔️ |
| Resource Dependencies | ✔️ |
| Output Variables | ✔️ |

---

### **🚀 Next Steps**
Would you like more **hands-on exercises**, **real-world projects**, or **Terraform modules**? Let me know how I can assist further! 🚀
