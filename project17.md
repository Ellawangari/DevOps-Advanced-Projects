# AUTOMATE INFRASTRUCTURE WITH IAC USING TERRAFORM. PART 2

# Step 1: Networking
- Created 4 private subnets keeping in mind following principles:
```
Make sure you use variables or length() function to determine the number of AZs
Use variables and cidrsubnet() function to allocate vpc_cidr for subnets
Keep variables and resources in separate files for better code structure and readability
Tags all the resources you have created so far. Explore how to use format() and count functions to automatically tag subnets with its respective number.
```
![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj17/1.PNG)

- Created an Internet Gateway in a separate Terraform file internet_gateway.tf

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj17/2.PNG)
- Created 1 NAT Gateways and 1 Elastic IP (EIP) addresses using the following command
```
resource "aws_eip" "nat_eip" {
  vpc        = true
  depends_on = [aws_internet_gateway.ig]

  tags = merge(
    var.tags,
    {
      Name = format("%s-EIP", var.name)
    },
  )
}

resource "aws_nat_gateway" "nat" {
  allocation_id = aws_eip.nat_eip.id
  subnet_id     = element(aws_subnet.public.*.id, 0)
  depends_on    = [aws_internet_gateway.ig]

  tags = merge(
    var.tags,
    {
      Name = format("%s-Nat", var.name)
    },
  )
}
```

# Step 2: AWS ROUTES
- Created a file called route_tables.tf and use it to create routes for both public and private subnets.
```
# create private route table
resource "aws_route_table" "private-rtb" {
  vpc_id = aws_vpc.main.id

  tags = merge(
    var.tags,
    {
      Name = format("%s-Private-Route-Table", var.name)
    },
  )
}

# associate all private subnets to the private route table
resource "aws_route_table_association" "private-subnets-assoc" {
  count          = length(aws_subnet.private[*].id)
  subnet_id      = element(aws_subnet.private[*].id, count.index)
  route_table_id = aws_route_table.private-rtb.id
}

# create route table for the public subnets
resource "aws_route_table" "public-rtb" {
  vpc_id = aws_vpc.main.id

  tags = merge(
    var.tags,
    {
      Name = format("%s-Public-Route-Table", var.name)
    },
  )
}

# create route for the public route table and attach the internet gateway
resource "aws_route" "public-rtb-route" {
  route_table_id         = aws_route_table.public-rtb.id
  destination_cidr_block = "0.0.0.0/0"
  gateway_id             = aws_internet_gateway.ig.id
}

# associate all public subnets to the public route table
resource "aws_route_table_association" "public-subnets-assoc" {
  count          = length(aws_subnet.public[*].id)
  subnet_id      = element(aws_subnet.public[*].id, count.index)
  route_table_id = aws_route_table.public-rtb.id
}
```

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj17/4.PNG)

- Installed graph using the following command  `sudo apt install graphviz` and ran the command to view the view the relation of the resources created
`terraform graph -type=plan | dot -Tpng > graph.png`

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj17/3.PNG)

![alt text](https://github.com/Ellawangari/DevOps-Advanced-Projects/blob/main/Imgs/prj17/5.PNG)

This [link](https://github.com/Ellawangari/Terraform-IAC-Part2) contains the repo of the codes used in this project 17
