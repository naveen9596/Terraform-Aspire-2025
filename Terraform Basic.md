Here’s a structured guide covering **Terraform installation**, **HashiCorp Configuration Language (HCL) basics**, and **Terraform fundamentals** to help you get started.  

---

## **1️⃣ Installing Terraform**  

### **Step 1: Download Terraform**  
Download the latest Terraform version for your OS from [HashiCorp Terraform Downloads](https://developer.hashicorp.com/terraform/downloads).  

### **Step 2: Install Terraform**  
- **Windows**:  
  1. Extract the downloaded `.zip` file.  
  2. Move `terraform.exe` to a directory in the system PATH (e.g., `C:\terraform\`).  
  3. Add the directory to the environment variables.  

- **Linux/macOS**:  
  Run the following commands:  
  ```sh
  sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
  wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
  echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
  sudo apt-get update
  sudo apt-get install terraform
  ```
  OR use Homebrew on macOS:  
  ```sh
  brew tap hashicorp/tap
  brew install hashicorp/tap/terraform
  ```

### **Step 3: Verify Installation**  
Check if Terraform is installed successfully:  
```sh
terraform version
```

---

## **2️⃣ HashiCorp Configuration Language (HCL) Basics**  
HCL is Terraform's declarative language for defining infrastructure as code (IaC).  

### **Basic HCL Syntax**  
- **Variables**  
  ```hcl
  variable "region" {
    description = "AWS Region"
    type        = string
    default     = "us-east-1"
  }
  ```
- **Resources** (e.g., AWS instance)  
  ```hcl
  resource "aws_instance" "web" {
    ami           = "ami-12345678"
    instance_type = "t2.micro"
  }
  ```
- **Output values**  
  ```hcl
  output "instance_ip" {
    value = aws_instance.web.public_ip
  }
  ```
- **Providers**  
  ```hcl
  provider "aws" {
    region = var.region
  }
  ```

---

## **3️⃣ Terraform Basics**  

### **1. Initialize Terraform**  
```sh
terraform init
```
This downloads necessary providers and initializes the workspace.

### **2. Plan Terraform Deployment**  
```sh
terraform plan
```
Shows what Terraform will create.

### **3. Apply the Configuration**  
```sh
terraform apply -auto-approve
```
Executes the changes.

### **4. Destroy the Resources**  
```sh
terraform destroy -auto-approve
```
Removes all resources.

---

### **Next Steps**
Would you like a hands-on **Terraform project** (e.g., AWS/Azure VM setup) or more advanced concepts like **Terraform modules, state management, or cloud automation**? 🚀
