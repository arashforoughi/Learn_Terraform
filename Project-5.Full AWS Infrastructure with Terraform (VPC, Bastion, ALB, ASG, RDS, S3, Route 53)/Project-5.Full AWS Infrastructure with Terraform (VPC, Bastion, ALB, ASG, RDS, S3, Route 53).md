# Project 5: Full AWS Infrastructure with Terraform (VPC, Bastion, ALB, ASG, RDS, S3, Route 53)

### **📌 Scenario:**

A company needs a **secure, scalable AWS infrastructure** with the following:

✅ **VPC with Public & Private Subnets**

✅ **Bastion Host for SSH Access**

✅ **Auto Scaling for Application Servers**

✅ **Application Load Balancer for High Availability**

✅ **Amazon RDS (MySQL) for Database Storage**

✅ **Route 53 for Custom Domain Name Resolution**

## **📂 Procedure:**

### **Step 1: Create VPC with Public & Private Subnets**

```hcl
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  cidr    = "10.0.0.0/16"
  enable_dns_hostnames = true
  enable_dns_support   = true
}
```

✅ **Creates a VPC with networking.**

### **Step 2: Deploy a Bastion Host for Secure SSH Access**

```hcl
resource "aws_instance" "bastion" {
  ami           = "ami-123456"
  instance_type = "t2.micro"
  subnet_id     = module.vpc.public_subnets[0]
  key_name      = var.ssh_key_name
}
```

✅ **Provides a secure entry point into the private network.**

### **Step 3: Create an Auto Scaling Group (ASG)**

```hcl
resource "aws_autoscaling_group" "web_asg" {
  desired_capacity     = 3
  max_size            = 5
  min_size            = 2
  vpc_zone_identifier = module.vpc.private_subnets
}
```

✅ **Auto-scales the application servers.**

### **Step 4: Deploy an Application Load Balancer (ALB)**

```hcl
resource "aws_lb" "app_lb" {
  name               = "app-lb"
  internal           = false
  load_balancer_type = "application"
}
```

✅ **Distributes traffic to the ASG.**

### **Step 5: Configure Route 53 for a Custom Domain**

```hcl
resource "aws_route53_record" "app_dns" {
  zone_id = var.zone_id
  name    = "app.example.com"
  type    = "A"
  alias {
    name                   = aws_lb.app_lb.dns_name
    evaluate_target_health = true
  }
}
```

✅ **Maps a custom domain to the ALB.**

## **🧪 Verification:**

1. **Check Load Balancer:**
    
    ```bash
    $ aws elbv2 describe-load-balancers
    ```
    
2. **Verify Auto Scaling:**
    
    ```bash
    $ aws autoscaling describe-auto-scaling-groups
    ```
    
3. **Test Bastion Host SSH Access:**
    
    ```bash
    $ ssh -i my-key.pem ec2-user@<bastion-host-ip>
    ```