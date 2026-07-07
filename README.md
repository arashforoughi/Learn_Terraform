# Terraform Learning Journey

This repository is a structured learning workspace for mastering Terraform and Infrastructure as Code (IaC). It contains step-by-step notes, hands-on lab concepts, and real-world project ideas covering the fundamentals of Terraform as well as advanced deployment patterns.

Whether you are a beginner learning Terraform for the first time or an intermediate engineer looking to strengthen your cloud automation skills, this repository provides a practical path from basic concepts to production-style infrastructure design.

## What This Repository Covers

This course includes:

- Terraform fundamentals and architecture
- Providers, resources, variables, outputs, and state management
- Modules, workspaces, backends, loops, and conditionals
- Provisioners, debugging, security best practices, and CI/CD integration
- Hands-on cloud projects for AWS and multi-cloud scenarios

## Repository Structure

The content is organized into lessons and projects:

- [00.List of Contents](00.List%20of%20Contents/) – Course overview and study roadmap
- [01.Introduction to Terraform](01.Introduction%20to%20Terraform/) – What Terraform is and why it matters
- [02.Setting Up Terraform](02.Setting%20Up%20Terraform/) – Installation and setup
- [03.Terraform Configuration Files & Basics](03.Terraform%20Configuration%20Files%20%26%20Basics/) – Core syntax and configuration structure
- [04.Terraform State Management](04.Terraform%20State%20Management/) – State files, backends, and locking
- [05.Terraform Variables & Outputs](05.Terraform%20Variables%20%26%20Outputs/) – Reusable and parameterized infrastructure
- [06.Terraform Modules](06.Terraform%20Modules/) – Reusable components and best practices
- [07.Terraform Provisioners & Null Resources](07.Terraform%20Provisioners%20%26%20Null%20Resources/) – Execution and post-creation tasks
- [08.Terraform Workspaces & Environments](08.Terraform%20Workspaces%20%26%20Environments/) – Multi-environment workflows
- [09.Managing Terraform Backends](09.Managing%20Terraform%20Backends/) – Remote state and collaboration
- [10.Terraform Loops & Conditionals](10.Terraform%20Loops%20%26%20Conditionals/) – Dynamic and conditional resource creation
- [11.Handling Terraform Errors & Debugging](11.Handling%20Terraform%20Errors%20%26%20Debugging/) – Troubleshooting and validation
- [12.Terraform Security Best Practices](12.Terraform%20Security%20Best%20Practices/) – Secrets, access control, and security hygiene
- [13.Integrating Terraform with CI CD Pipelines](13.Integrating%20Terraform%20with%20CI%20CD%20Pipelines/) – Automation and DevOps workflows
- [14.Terraform Cloud & Enterprise](14.Terraform%20Cloud%20%26%20Enterprise/) – Remote execution and governance
- [15.Advanced Terraform Topics](15.Advanced%20Terraform%20Topics/) – Advanced patterns and scaling
- [16.Real-World Projects & Hands-On Labs](16.Real-World%20Projects%20%26%20Hands-On%20Labs/) – Practice projects
- [17.Deploying a Bastion Host in AWS using Terraform](17.Deploying%20a%20Bastion%20Host%20in%20AWS%20using%20Terraform/) – A practical AWS example
- Project folders for larger architecture scenarios such as VPCs, ALB, EKS, serverless apps, and multi-cloud setups

## Recommended Learning Path

1. Start with the introduction and setup sections.
2. Learn the core Terraform syntax and configuration structure.
3. Understand state management and variables before building larger infrastructure.
4. Practice with modules and workspaces.
5. Move on to advanced topics like backends, security, CI/CD, and cloud projects.

## Prerequisites

To follow along effectively, you should have:

- Terraform installed locally
- A cloud account such as AWS, Azure, or GCP (depending on the lesson)
- Basic familiarity with command-line tools
- Optional: Git and a code editor such as VS Code

## How to Use This Repository

- Read the lesson files in order.
- Follow the examples and try them locally in a sandbox or test environment.
- Apply the concepts to the hands-on projects.
- Experiment with your own infrastructure ideas after completing each section.

## Example Workflow

A typical Terraform workflow covered in this repository looks like this:

```bash
terraform init
terraform plan
terraform apply
terraform destroy
```

These commands are used throughout the learning materials to provision, review, and remove infrastructure safely.

## Notes

This repository is primarily educational and documentation-oriented. The focus is on understanding Terraform concepts, architecture, and best practices rather than delivering a single production application.

## Goals

By the end of this learning journey, you should be able to:

- Write Terraform configurations confidently
- Manage infrastructure as code in a repeatable way
- Use modules and backends effectively
- Deploy and troubleshoot cloud infrastructure with Terraform
- Apply Terraform in real-world DevOps and cloud automation scenarios

## License

This project is intended for educational purposes.
