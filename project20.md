# MIGRATION TO THE СLOUD WITH CONTAINERIZATION. PART 1 – DOCKER &AMP; DOCKER COMPOSE



# Step 1: Install Docker and prepare for migration to the Cloud
-Install Docker Engine, which is a client-server application that contains:
```
A server with a long-running daemon process dockerd.
APIs that specify interfaces that programs can use to talk to and instruct the Docker daemon.
A command-line interface (CLI) client docker.
```
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/1.PNG)

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/2.PNG)

# Step 2: Deploy the MySQL Container to your Docker Engine
- Created a custom network with a subnet dedicated for both MySQL and the Tooling application so that they connect: `$ docker network create --subnet=172.18.0.0/16 tooling_app_network`
- Pulling the MySQL image and running the container: `$ docker run --network tooling_app_network -h mysqlserverhost --name=mysql-server -e MYSQL_ROOT_PASSWORD=$MYSQL_PW -d mysql/mysql-server:latest`
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/3.PNG)

- Verified the docker container by running `docker ps -a`
- 
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/4.PNG)
 
 
- Because it's not a good practice to connect to MySQL server remotely using the root user. Creating a file create_user.sql and adding the following code in order to create a user: `CREATE USER 'admin'@'%' IDENTIFIED BY 'password123'; GRANT ALL PRIVILEGES ON * . * TO 'admin'@'%';`
- Running the script to create the new user: `$ docker exec -i mysql-server mysql -uroot -p$MYSQL_PW < create_user.sql`
- Connecting to the MySQL server from a second container running the MySQL client utility:` $ docker run --network tooling_app_network --name mysql-client -it --rm mysql mysql -h mysqlserverhost -u admin -p`


 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/5.PNG)
 
 
# Step 3: Preparing The Database Schema
- Cloned the Tooling-app repository: `$ git clone https://github.com/darey-devops/tooling.git`
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/6.PNG)
 
- Exported the location of the SQL file that contains data for setting up the MySQL database:`$ export tooling_db_schema=/tooling_db_schema.sql`

 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/7.PNG)
 
 - Ran `export -p ` to confirm the exported location.
 
  ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/8.PNG)
 
 
- Using the SQL script to create the database and prepare the schema:` $ docker exec -i mysql-server mysql -uroot -p$MYSQL_PW < $tooling_db_schema`

 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/9.PNG)
 
# Step 4: Running The Tooling App
- From the tooling app directory where the Dockerfile is, running the following command to build the docker image:` $ docker build -t tooling:0.0.1 .`

 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/10.PNG)

- Running the container: `$ docker run --network tooling_app_network -p 8085:80 -it tooling:0.0.1`

  ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/11.PNG)

- Tested the tooling app in the browser:http://localhost:8085

   ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/12.PNG)

# Step 5 : Migrating PHP-Todo App Into A Containerized Application
- Cloned the php-todo app repository https://github.com/Ellawangari/php-todo and wrote a Dockerfile for the application

- Created a MySQL container for the php-todo frontend
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/13.PNG)
 
- Executed the build command to create the Docker image.

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/13p1.PNG)

- Ran` docker container ls` to check for the container
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/13p2.PNG)


- Running the artisan migrate command inside the php-todo container.

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/13p3.PNG)


- Tested the php-todo app on the browser.
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/13p4.PNG)


# Step 5: Pushing The Docker Image To Docker Registry
 - Created a new repository in the Docker registry.
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/docker1.PNG)


- Logged in from the commandline and changing the name of the php-todo image and giving it a tag.

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/docker2.PNG)

 - Pushed the php-todo app image to my Docker repository
 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/docker3.PNG)
 
 
# Step 6: Running Docker Build And Docker Push on Jenkins
- Created a repository in AWS Elastic Container Registry

 ![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/14.PNG)
 

- Setting up the Jenkins server by installing Docker plugin in Jenkins to run Docker job.

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/15.PNG)

- Giving Jenkins permission on .aws configuration file: Moving the .aws folder to /var/lib/jenkins directory and running the following command:$ sudo chown jenkins:jenkins .aws and also running the command: `$ sudo chmod 666 /var/run/docker.run`

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/16.PNG)

- Logged in to my commandline.

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/17.PNG)


- Pushed the changes to my github repo and created a multibranch pipeline job and linking it to the php-todo repository.

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/18.PNG)

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/19.PNG)

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj20/20.PNG)






 

