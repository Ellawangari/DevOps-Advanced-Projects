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
 
 
# Step 6: Running The Terraform Cloud To Provision Resources
- Inputing the AMI ID in my terraform.tfvars file for the servers built with packer which terraform will use to provision Bastion, Nginx, Tooling and Wordpress server.
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/9.PNG)
 
 - Pushing the code to my github repo will cause a trigger plan on my terraform cloud workspace, so i accepted the plan and automatically did a terraform apply of my infrastructure.
  ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/10.PNG)
  
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/11.PNG)
 
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/12.PNG)
 
  # Step 7 : Configuring The Infrastructure With Ansible
  - Connected to the bastion  server on my vs code to run ansible scripts for the infrastructure.
  
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/14.PNG)
 
 - Updating the nginx.conf.j2 file to input the internal load balancer dns name generated via terraform:


 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/15.PNG)

  
- Updated the RDS endpoints, Database name, password and username in the setup-db.yml file for both the tooling and wordpress role

  ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/16.PNG)
  
   ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/17.PNG)
  
- Updated the EFS Access point ID for both the wordpress and tooling role in the main.yml
- For tooling.
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/18.PNG)
- For Wordpress 

 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/19.PNG)
 
 
 - Verified the inventory using dynamic inventory.
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/20.PNG)

- Exported the environment variable ANSIBLE_CONFIG to point to the ansible.cfg from the repo and running the ansible-playbook command: $ ansible-playbook -i inventory/aws_ec2.yml playbook/site.yml

 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/21.PNG)
  ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/22.PNG)
  
# Step 8:  Setting Slack Notification For trigger Events
- On the setting tab selected notification, selecting slack option.
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/slack1.PNG)
 
  ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/slack2.PNG)
  
# Step 9:  Setting The Terraform Cloud To Execute Only From 'Dev' Branch
- Created 3 branches dev, prod and test.
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/branches.PNG)

- On the version control page in the settings tab, setting the VCS branch to dev.
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/dev.PNG)

- Pushed the code and the terraform plan was executed on the dev branch thus triggering a notification on my slack channel.
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/dev2.PNG)

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/dev%20slack.PNG)

# Step 9:  Confirmed my infrastructure and performed a terraform destroy on it.

- Confirmed the tooling and wordpress sites created by the infrastructure.
- For tooling 
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/tooling1.PNG)
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/tooling2.PNG)

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/25.PNG)
- For wordpress
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/wordpress1.PNG)
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/wordpress2.PNG)

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/26.PNG)

- Destroyed the infrastructure by clicking on Destruction and Delete option in the settings tab.

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/destroy1.PNG)

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj19/destroy2.PNG)







 
This [link](https://github.com/Ellawangari/Terraform-Cloud) contains the repo of the codes used in this project 18
