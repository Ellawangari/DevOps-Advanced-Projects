# Automate Infrastructure With IaC using Terraform. Part 4 – Terraform Cloud

- Terraform Cloud is a managed service that provides you with Terraform CLI to provision infrastructure, either on demand or in response to various events.
- Terraform Cloud executes Terraform commands on disposable virtual machines, this remote execution is also called remote operations.

# Step 1: Created a Terraform Cloud account and an organization

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/3.PNG)

# Step 2: Configure a version control workflow workspace 


![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/4.PNG)

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/5.PNG)

# Step 3: Configured variables
- Terraform Cloud supports two types of variables: environment variables and Terraform variables. Either type can be marked as sensitive, which prevents them from being displayed in the Terraform Cloud web UI and makes them write-only.

- Set two environment variables: AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY.

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/6.PNG)

# Step 4: Installed Packer and Ansible

- AMI: for building packer images
- Ansible: for Ansible scripts to configure the 

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/1.PNG)

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/2.PNG)

# Step 5 : Builing Packer Images

- Running the packer commands to build AMI for Bastion server, Nginx server and webserver

 ### For Bastion Server
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/bastionpkr1.PNG)
 
  ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/bastionpkr2.PNG)
  
 ### For Nginx Server
 
  ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/nginxami.PNG)
  
 ### For Tooling and Wordpress Server
 
   ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/webami.PNG)
   
  ### For Jenkins, Artifactory and sonarqube Server
   ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/jenkinsonarami.PNG)

 - Final output of the created AMIs
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/8.PNG)
 
 
# Step 5 : Running The Terraform Cloud To Provision Resources
- Inputing the AMI ID in my terraform.tfvars file for the servers built with packer which terraform will use to provision Bastion, Nginx, Tooling and Wordpress server.
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/8.PNG)
 
 
 
 
This [link](https://github.com/Ellawangari/Terraform-Cloud) contains the repo of the codes used in this project 18
