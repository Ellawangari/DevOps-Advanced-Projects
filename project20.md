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
 
 

