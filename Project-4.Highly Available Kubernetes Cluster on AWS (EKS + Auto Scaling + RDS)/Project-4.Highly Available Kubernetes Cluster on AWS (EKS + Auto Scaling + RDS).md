# Project 4: Highly Available Kubernetes Cluster on AWS (EKS + Auto Scaling + RDS)

### **📌 Scenario:**

You need to deploy a **highly available Kubernetes cluster** on AWS with the following setup:

✅ **Amazon EKS** as the managed Kubernetes service

✅ **Auto Scaling Worker Nodes** using AWS Auto Scaling Groups

✅ **Amazon RDS (PostgreSQL)** for database storage

✅ **Application Load Balancer (ALB)** for traffic distribution

## **📂 Procedure:**

### **Step 1: Create VPC & Networking Components**

```hcl
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "3.19.0"

  name                 = "eks-vpc"
  cidr                 = "10.0.0.0/16"
  enable_dns_hostnames = true
  enable_dns_support   = true
}
```

✅ **Creates a VPC with public/private subnets.**

### **Step 2: Deploy Amazon EKS Cluster**

```hcl
module "eks" {
  source          = "terraform-aws-modules/eks/aws"
  cluster_name    = "my-eks-cluster"
  cluster_version = "1.31"
  subnet_ids      = module.vpc.private_subnets
}
```

✅ **Sets up an EKS cluster in private subnets.**

### **Step 3: Configure Auto Scaling Worker Nodes**

```hcl
module "eks_worker_nodes" {
  source  = "terraform-aws-modules/autoscaling/aws"

  name                = "eks-workers"
  min_size            = 2
  max_size            = 5
  desired_capacity    = 3
  instance_type       = "t3.medium"
  vpc_zone_identifier = module.vpc.private_subnets
}
```

✅ **Auto-scales worker nodes from 2 to 5 instances.**

### **Step 4: Deploy Amazon RDS for PostgreSQL**

```hcl
resource "aws_db_instance" "postgres" {
  allocated_storage = 20
  engine           = "postgres"
  engine_version   = "14"
  instance_class   = "db.t3.medium"
  publicly_accessible = false
}
```

✅ **Creates an RDS database for application storage.**

### **Step 5: Deploy an Application using Kubernetes (Helm)**

```hcl
resource "helm_release" "app" {
  name       = "my-app"
  chart      = "my-app-chart"
  namespace  = "default"
}
```

✅ **Deploys an application with Helm.**

## **🧪 Verification:**

1. **Check EKS cluster:**
    
    ```bash
    $ aws eks update-kubeconfig --name my-eks-cluster
    $ kubectl get nodes
    ```
    
2. **Check Auto Scaling:**
    
    ```bash
    $ aws autoscaling describe-auto-scaling-groups
    ```
    
3. **Verify Database Connectivity:**
    
    ```bash
    $ psql -h <rds-endpoint> -U postgres -d mydb
    ```