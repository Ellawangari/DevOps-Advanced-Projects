# ORCHESTRATING CONTAINERS ACROSS MULTIPLE VIRTUAL SERVERS WITH KUBERNETES. PART 1

# Kubernetes From-Ground-Up
- The project was all about implementing a K8s from scratch from configuring the master and worker nodes to security and network configuration.The sole purpose for this was to get a better understanding of installing each and every component manually from scratch and learn how to make them work together.

# Step 1: Install Client Tools
- Tools to be installed were:
```
awscli – is a unified tool to manage your AWS services
kubectl – this command line utility will be your main control tool to manage your K8s cluster. You will use this tool so many times, so you will be able to type ‘kubetcl’ on your keyboard with a speed of light. You can always make a shortcut (alias) to just one character ‘k’. Also, add this extremely useful official kubectl Cheat Sheet to your bookmarks, it has examples of the most used ‘kubectl’ commands.
cfssl – an open source toolkit for everything TLS/SSL from Cloudflare
cfssljson – a program, which takes the JSON output from the cfssl and writes certificates, keys, CSRs, and bundles to disk.
```

- Installing kubectl command
```
Download the binary

wget https://storage.googleapis.com/kubernetes-release/release/v1.21.0/bin/linux/amd64/kubectl
Make it executable

chmod +x kubectl
Move to the Bin directory

sudo mv kubectl /usr/local/bin/
Verify that kubectl version 1.21.0 or higher is installed:

kubectl version --client
```

- Installing CFSSL and CFSSLJSON
```
wget -q --show-progress --https-only --timestamping \
  https://storage.googleapis.com/kubernetes-the-hard-way/cfssl/1.4.1/linux/cfssl \
  https://storage.googleapis.com/kubernetes-the-hard-way/cfssl/1.4.1/linux/cfssljson
chmod +x cfssl cfssljson
sudo mv cfssl cfssljson /usr/local/bin/
```

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj21/1.PNG)

# Step 2: Configure Network Infrastructure

- Created a directory named k8s-cluster-from-ground-up.
- Creating a VPC and storing its ID as a variable:
```$ VPC_ID=$(aws ec2 create-vpc \
--cidr-block 172.31.0.0/16 \
--output text --query 'Vpc.VpcId'
)
```

- Tagging the VPC so that it is named:
```
$ NAME=k8s-cluster-from-ground-up

$ aws ec2 create-tags \
  --resources ${VPC_ID} \
  --tags Key=Name,Value=${NAME}
  ```
- Enabling DNS support for the VPC:
```
$ aws ec2 modify-vpc-attribute \
--vpc-id ${VPC_ID} \
--enable-dns-support '{"Value": true}'
```
- Enabling DNS support for hostnames:
```
$ aws ec2 modify-vpc-attribute \
--vpc-id ${VPC_ID} \
--enable-dns-hostnames '{"Value": true}'
Setting the required region:$ AWS_REGION=us-east-1
```
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj21/2.PNG)

- Configuring DHCP Options Set:
```
$ DHCP_OPTION_SET_ID=$(aws ec2 create-dhcp-options \
  --dhcp-configuration \
    "Key=domain-name,Values=$AWS_REGION.compute.internal" \
    "Key=domain-name-servers,Values=AmazonProvidedDNS" \
  --output text --query 'DhcpOptions.DhcpOptionsId')
  ```
  
- Tagging the DHCP Option set:
```
$ aws ec2 create-tags \
  --resources ${DHCP_OPTION_SET_ID} \
  --tags Key=Name,Value=${NAME}
Associating the DHCP Option set with the VPC:
$ aws ec2 associate-dhcp-options \
  --dhcp-options-id ${DHCP_OPTION_SET_ID} \
  --vpc-id ${VPC_ID}
```

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj21/3.PNG)

- Confirmed the DHCP option set was included in the VPC

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj21/4.PNG)

- Created a Subnet:
```
SUBNET_ID=$(aws ec2 create-subnet \
  --vpc-id ${VPC_ID} \
  --cidr-block 172.31.0.0/24 \
  --output text --query 'Subnet.SubnetId')
  
aws ec2 create-tags \
  --resources ${SUBNET_ID} \
  --tags Key=Name,Value=${NAME
  
  ```
  
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj21/5.PNG)

  - Created the Internet Gateway and attached it to the VPC:
  ```
  INTERNET_GATEWAY_ID=$(aws ec2 create-internet-gateway \
  --output text --query 'InternetGateway.InternetGatewayId')
aws ec2 create-tags \
  --resources ${INTERNET_GATEWAY_ID} \
  --tags Key=Name,Value=${NAME}
aws ec2 attach-internet-gateway \
  --internet-gateway-id ${INTERNET_GATEWAY_ID} \
  --vpc-id ${VPC_ID}
  ```
  
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj21/6.PNG)

  - Created route tables, associated the route table to subnet, and created a route to allow external traffic to the Internet through the Internet Gateway:
  ```
  ROUTE_TABLE_ID=$(aws ec2 create-route-table \
  --vpc-id ${VPC_ID} \
  --output text --query 'RouteTable.RouteTableId')
aws ec2 create-tags \
  --resources ${ROUTE_TABLE_ID} \
  --tags Key=Name,Value=${NAME}
aws ec2 associate-route-table \
  --route-table-id ${ROUTE_TABLE_ID} \
  --subnet-id ${SUBNET_ID}
aws ec2 create-route \
  --route-table-id ${ROUTE_TABLE_ID} \
  --destination-cidr-block 0.0.0.0/0 \
  --gateway-id ${INTERNET_GATEWAY_ID}
  ```
  
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj21/7.PNG)



