# Project 2: AWS Auto Scaling with ALB using Terraform

## **🔹 Overview**

We will create:

✅ **VPC with Public & Private Subnets** (Using our previous VPC)

✅ **Application Load Balancer (ALB)**

✅ **Auto Scaling Group (ASG) with EC2 Instances**

✅ **Target Group & Health Checks**

## **📂 Folder Structure**

```bash
terraform-autoscaling/
│── main.tf
│── variables.tf
│── outputs.tf
│── modules/
│   ├── alb/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   ├── asg/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
```

## **Step 1: Define the ALB Module (modules/alb/main.tf)**

```hcl
resource "aws_lb" "my_alb" {
  name               = "my-load-balancer"
  internal           = false
  load_balancer_type = "application"
  security_groups    = [var.alb_sg_id]
  subnets           = var.public_subnets

  tags = {
    Name = "MyALB"
  }
}

resource "aws_lb_target_group" "my_target_group" {
  name     = "my-target-group"
  port     = 80
  protocol = "HTTP"
  vpc_id   = var.vpc_id
}

resource "aws_lb_listener" "http" {
  load_balancer_arn = aws_lb.my_alb.arn
  port              = "80"
  protocol          = "HTTP"

  default_action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.my_target_group.arn
  }
}
```

## **Step 2: Define the Auto Scaling Group (modules/asg/main.tf)**

```hcl
resource "aws_launch_template" "my_template" {
  name_prefix   = "asg-template"
  image_id      = "ami-0c55b159cbfafe1f0"  # Use latest AMI
  instance_type = "t2.micro"

  tag_specifications {
    resource_type = "instance"
    tags = {
      Name = "AutoScalingInstance"
    }
  }
}

resource "aws_autoscaling_group" "my_asg" {
  vpc_zone_identifier = var.private_subnets
  desired_capacity    = 2
  max_size           = 3
  min_size           = 1

  launch_template {
    id      = aws_launch_template.my_template.id
    version = "$Latest"
  }

  target_group_arns = [var.target_group_arn]
}
```

## **Step 3: Use the Modules in `main.tf`**

```hcl
module "alb" {
  source        = "./modules/alb"
  vpc_id        = module.vpc.vpc_id
  alb_sg_id     = module.security_group.alb_sg_id
  public_subnets = module.vpc.public_subnets
}

module "asg" {
  source        = "./modules/asg"
  private_subnets = module.vpc.private_subnets
  target_group_arn = module.alb.target_group_arn
}
```

## **Step 4: Deploy the Auto Scaling Setup**

```bash
$ terraform init
$ terraform apply -auto-approve
```

## **✅ What We Have Built**

✔ **ALB with a target group for Auto Scaling**

✔ **Auto Scaling Group that launches EC2 instances**

✔ **Instances automatically register with ALB**