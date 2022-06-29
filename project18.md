# AUTOMATE INFRASTRUCTURE WITH IAC USING TERRAFORM. PART 3 – REFACTORING

# Step 1: Introducing Backend on S3
- Each Terraform configuration can specify a backend, which defines where and how operations are performed, where state snapshots are stored, etc.
- Created a file and name it backend.tf and added the following code snippet.
```
# Note: The bucket name may not work for you since buckets are unique globally in AWS, so you must give it a unique name.
resource "aws_s3_bucket" "terraform_state" {
  bucket = "dev-terraform-bucket"
  # Enable versioning so we can see the full revision history of our state files
  versioning {
    enabled = true
  }
  # Enable server-side encryption by default
  server_side_encryption_configuration {
    rule {
      apply_server_side_encryption_by_default {
        sse_algorithm = "AES256"
      }
    }
  }
}
```
- Created a DynamoDB table to handle locks and perform consistency checks.Performed the terraform apply then.
```
resource "aws_dynamodb_table" "terraform_locks" {
  name         = "terraform-locks"
  billing_mode = "PAY_PER_REQUEST"
  hash_key     = "LockID"
  attribute {
    name = "LockID"
    type = "S"
  }
}
```
- Configured S3 Backend and perform a `terraform init` command.
```
terraform {
  backend "s3" {
    bucket         = "dev-terraform-bucket"
    key            = "global/s3/terraform.tfstate"
    region         = "eu-central-1"
    dynamodb_table = "terraform-locks"
    encrypt        = true
  }
}
```
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj18/6.PNG)

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj18/4.PNG)

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj18/5.PNG)

# Step 2: Terraform Workspaces and Terraform Modules
- Workspaces are used for environments that do not greatly deviate from each other, to avoid duplication of your configurations.
- Modules serve as containers that allow to logically group Terraform codes for similar resources in the same domain (e.g., Compute, Networking, AMI, etc.). One root module can call other child modules and insert their configurations when applying Terraform config. This concept makes the code structure neater, and it allows different team members to work on different parts of configuration at the same time.

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj18/1.PNG)

# Step 3: Refactored my code using Modules.

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj18/2.PNG)

- Did a  terraform fmt then a  `terraform plan ` to check the infrastructure that was to be created  and `terraform validate ` commands.

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj18/3.PNG)

- Terraform apply command could not working for this specific project since launch template does not contain the latest endpoints for EFS, ALB and RDS and also my AMI were not properly configured.

This [link](https://github.com/Ellawangari/Terraform-IAC-Part3) contains the repo of the codes used in this project 18
