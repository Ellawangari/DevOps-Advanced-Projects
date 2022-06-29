
# AUTOMATE INFRASTRUCTURE WITH IAC USING TERRAFORM PART 1

# Step 1: Creating IAM user and ensured it had programatic access and creation of an s3 bucket.

- Installed Python SDK (boto3).

 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj16/1.PNG)
-  Created an S3 bucket to store Terraform state file.

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj16/2.PNG)

-  To ensure I configured authentication and installed boto3 ran the following command to display the s3 bucket created.

 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj16/4.PNG)
 
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj16/3.PNG)
 
# Step 2:  Creating VPC Resource
- Created a folder called PBL
- Created a file in the PBL folder and named  it main.tf
- 
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj16/5.PNG)

- Added the provider AWS
- 
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj16/6.PNG)
- Ran the `terraform init` command
- Added the following resource block to create a VPC.
  ```
    provider "aws" {
    region = "eu-central-1"
    }# Create VPC
    resource "aws_vpc" "main" {
    cidr_block                     = "172.16.0.0/16"
    enable_dns_support             = "true"
    enable_dns_hostnames           = "true"
    enable_classiclink             = "false"
    enable_classiclink_dns_support = "false"}
  ```

- Added the following resource block to create 2 subnets
 ``` # Create public subnets1
    resource "aws_subnet" "public1" {
    vpc_id                     = aws_vpc.main.id
    cidr_block                 = "172.16.0.0/24"
    map_public_ip_on_launch    = true
    availability_zone          = "eu-central-1a"

}# Create public subnet2
    resource "aws_subnet" "public2" {
    vpc_id                     = aws_vpc.main.id
    cidr_block                 = "172.16.1.0/24"
    map_public_ip_on_launch    = true
    availability_zone          = "eu-central-1b"
}
```

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj16/7.PNG)

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj16/8.PNG)

- Destroyed the created subnets and vpc.

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj16/9.PNG)
- 
# Step 3:  Code Refactoring
- Introducing variables, and remove hard coded values.
- 
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj16/10.PNG)

- Ran the terrraform console to check how many cidr blocks will be created.

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj16/11.PNG)

- After code refactoring and creating new files with new resources, I ran the terraform plan, terraform fmt and terraform apply commands
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj16/12.PNG)
 
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj16/13.PNG)
 
 - Pushed my code to github reposiory and installed tree to check the structure of the directory.
 
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj16/14.PNG)
 
 - Confirmed to check for the newly created resources.
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj16/15.PNG)



