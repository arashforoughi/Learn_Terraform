# Project 1: Deploy an AWS VPC with Private & Public Subnets using Terraform Modules

## **🔹 Overview**

We will create a **VPC** with:

✅ **Public and Private Subnets**

✅ **Internet Gateway & NAT Gateway**

✅ **Routing Tables**

## **Step 1: Create a Terraform Module for VPC**

### **📂 Folder Structure**

```css
terraform-vpc/
│── main.tf
│── modules/
│   ├── vpc/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
```

## **Step 2: Define the VPC Module (modules/vpc/main.tf)**

```hcl
resource "aws_vpc" "main" {
  cidr_block = var.vpc_cidr
  tags = {
    Name = var.vpc_name
  }
}

resource "aws_subnet" "public_subnet" {
  vpc_id            = aws_vpc.main.id
  cidr_block        = var.public_subnet_cidr
  map_public_ip_on_launch = true
}

resource "aws_subnet" "private_subnet" {
  vpc_id            = aws_vpc.main.id
  cidr_block        = var.private_subnet_cidr
}

resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.main.id
}

resource "aws_route_table" "public_rt" {
  vpc_id = aws_vpc.main.id
}

resource "aws_route" "public_route" {
  route_table_id         = aws_route_table.public_rt.id
  destination_cidr_block = "0.0.0.0/0"
  gateway_id             = aws_internet_gateway.igw.id
}

resource "aws_route_table_association" "public_assoc" {
  subnet_id      = aws_subnet.public_subnet.id
  route_table_id = aws_route_table.public_rt.id
}
```

## **Step 3: Define Module Variables (modules/vpc/variables.tf)**

```hcl
variable "vpc_cidr" {}
variable "vpc_name" {}
variable "public_subnet_cidr" {}
variable "private_subnet_cidr" {}
```

## **Step 4: Use the Module in `main.tf`**

```hcl
module "vpc" {
  source              = "./modules/vpc"
  vpc_cidr           = "10.0.0.0/16"
  vpc_name           = "MyVPC"
  public_subnet_cidr = "10.0.1.0/24"
  private_subnet_cidr = "10.0.2.0/24"
}
```

✅ **Now run Terraform commands to create the VPC!**

```bash
$ terraform init
$ terraform apply
```

Let's test the **VPC with Public & Private Subnets** setup.

## **✅ Steps to Test the VPC Setup**

### **1️⃣ Initialize Terraform**

Run the following command in your project folder:

```bash
$ terraform init
```

This downloads required providers and initializes the working directory.

### **2️⃣ Validate the Configuration**

Check for any syntax errors before applying:

```bash
$ terraform validate
```

If everything is correct, it will return:

```bash
Success! The configuration is valid.
```

### **3️⃣ Preview the Changes**

Run the following command to see what resources will be created:

```bash
$ terraform plan
```

You'll see an output listing all the **VPC, Subnets, Internet Gateway, and Route Table** resources that will be created.

### **4️⃣ Apply the Terraform Configuration**

If everything looks good, apply the changes:

```bash
$ terraform apply -auto-approve
```

🔹 **Terraform will create:**

✅ **VPC** with CIDR block `10.0.0.0/16`

✅ **Public subnet** `10.0.1.0/24`

✅ **Private subnet** `10.0.2.0/24`

✅ **Internet Gateway for Public Subnet**

✅ **Route Tables & Associations**

✅ **Once completed, you’ll see an output like:**

```bash
Apply complete! Resources: 5 added, 0 changed, 0 destroyed.
```

### **5️⃣ Verify in AWS Console**

Go to **AWS Console → VPC Dashboard** and check:

- **VPCs:** Verify that "MyVPC" exists.
- **Subnets:** Check the public and private subnets.
- **Route Tables:** Ensure the public subnet has an Internet Gateway.

### **6️⃣ Clean Up (Optional)**

If you want to destroy everything:

```bash
$ terraform destroy -auto-approve
```

This removes all resources created by Terraform.