# Project 6: Multi-Cloud Setup (AWS & Azure) using Terraform

### **📌 Scenario:**

You need to deploy **the same infrastructure in AWS & Azure** for redundancy.

✅ **AWS: EC2, RDS (MySQL), ALB**

✅ **Azure: Virtual Machines, PostgreSQL, Load Balancer**

✅ **Terraform Providers for AWS & Azure**

## **📂 Procedure:**

### **Step 1: Configure Multi-Cloud Terraform Providers**

```hcl
provider "aws" {
  region = "us-east-1"
}

provider "azurerm" {
  features {}
}
```

✅ **Defines AWS & Azure providers.**

### **Step 2: Deploy AWS Infrastructure**

```hcl
resource "aws_instance" "aws_vm" {
  ami           = "ami-123456"
  instance_type = "t3.medium"
}
```

✅ **Deploys EC2 instance in AWS.**

### **Step 3: Deploy Azure Infrastructure**

```hcl
resource "azurerm_virtual_machine" "azure_vm" {
  name                = "azure-vm"
  location            = "East US"
  resource_group_name = azurerm_resource_group.rg.name
  vm_size             = "Standard_DS1_v2"
}
```

✅ **Deploys VM in Azure.**

### **Step 4: Configure a Global Load Balancer**

```hcl
resource "aws_route53_record" "multi_cloud_lb" {
  zone_id = var.zone_id
  name    = "app.example.com"
  type    = "A"
  alias {
    name                   = aws_lb.app_lb.dns_name
    evaluate_target_health = true
  }
}
```

✅ **Maps global DNS to AWS & Azure.**

## **🧪 Verification:**

1. **Check AWS VM & RDS:**
    
    ```bash
    $ aws ec2 describe-instances
    ```
    
2. **Check Azure VM:**
    
    ```bash
    $ az vm list
    ```
    
3. **Test Global Load Balancer:**
    
    ```bash
    $ nslookup app.example.com
    ```