# Terraform Infrastructure Automation

## Overview
This repository contains Terraform scripts for automating the setup of cloud infrastructure required to host a server instance. The scripts provision an AWS EC2 instance and configure necessary resources such as security groups and networking components.

## Prerequisites
- [Terraform](https://www.terraform.io/downloads.html) installed on your local machine.
- An AWS account with appropriate permissions to create resources.
- AWS CLI configured with your AWS credentials.

## Directory Structure
```
.
├── main.tf            # Main Terraform configuration file
├── variables.tf       # Variables used in Terraform configurations
├── terraform.tfvars    # Values for the defined variables
```

## Getting Started

### 1. Clone the Repository
Clone this repository to your local machine:

```bash
git clone https://github.com/yourusername/your-repo.git
cd your-repo
```

### 2. Configure AWS Credentials
Make sure your AWS credentials are set up. You can configure your AWS CLI with:

```bash
aws configure
```

### 3. Review Variables
Open the `variables.tf` file and set any required variables such as instance type, region, etc. You can also specify these in `terraform.tfvars`.

### 4. Initialize Terraform
Run the following command to initialize the Terraform configuration. This downloads the necessary provider plugins:

```bash
terraform init
```

### 5. Plan the Deployment
Run the following command to create an execution plan. This shows what resources will be created, updated, or destroyed:

```bash
terraform plan
```

### 6. Apply the Configuration
To create the resources defined in the Terraform configuration, run:

```bash
terraform apply
```

Type `yes` when prompted to confirm the action.

### 7. Outputs
After applying the configuration, Terraform will output important information, such as the public IP of the created EC2 instance. You can also check the `outputs.tf` file for any specific outputs defined.

### 8. Clean Up
To destroy the resources created by Terraform, run:

```bash
terraform destroy
```

Type `yes` when prompted to confirm the action.

## Notes
- Ensure you have the necessary IAM permissions in your AWS account to create and manage the resources defined in the Terraform scripts.
- Modify security group settings in the `main.tf` file to allow inbound traffic as required for your application.

