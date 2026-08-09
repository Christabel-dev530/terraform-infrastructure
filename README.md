# terraform-infrastructure
Terraform is an open-source infrastructure as code (IaC) software tool created by HashiCorp. It lets you build, change, and manage safe and predictable cloud and on-premises infrastructure using simple, human-readable configuration files
# AWS VPC Web Server Deployment with Terraform

##  Project Overview

This project demonstrates how to provision a complete AWS networking environment and deploy an Nginx web server using **Terraform**.

The infrastructure was created using **Infrastructure as Code (IaC)** instead of manually creating resources through the AWS Management Console.

The project provisions:

* AWS VPC
* Public Subnet
* Internet Gateway
* Public Route Table
* Security Group
* EC2 Instance
* Nginx Web Server

Terraform variables were also used to make the configuration **reusable, flexible, and easier to maintain**.

#  Project Architecture

The infrastructure follows this flow:

```text
                         Internet
                            │
                            ▼
                  ┌──────────────────┐
                  │ Internet Gateway │
                  └────────┬─────────┘
                           │
                           ▼
                  ┌──────────────────┐
                  │       VPC        │
                  │   10.0.0.0/16    │
                  └────────┬─────────┘
                           │
                           ▼
                  ┌──────────────────┐
                  │  Public Subnet   │
                  │   10.0.1.0/24    │
                  └────────┬─────────┘
                           │
                           ▼
                  ┌──────────────────┐
                  │  Public Route    │
                  │      Table       │
                  └────────┬─────────┘
                           │
                           ▼
                  ┌──────────────────┐
                  │  Security Group  │
                  └────────┬─────────┘
                           │
                           ▼
                  ┌──────────────────┐
                  │   EC2 Instance   │
                  │     Linux        │
                  └────────┬─────────┘
                           │
                           ▼
                  ┌──────────────────┐
                  │  Nginx Web Server│
                  └──────────────────┘
```

#  Project Objectives

The main objectives of this project were to:

1. Create an AWS VPC using Terraform.
2. Create a public subnet inside the VPC.
3. Create an Internet Gateway.
4. Create a public route table.
5. Associate the route table with the public subnet.
6. Create a Security Group to control traffic.
7. Provision an EC2 instance inside the public subnet.
8. Install Nginx on the EC2 instance.
9. Use Terraform variables instead of hardcoding configuration values.
10. Understand how AWS networking components work together.
11. Practice Infrastructure as Code using Terraform.

# Technologies and Tools

| Technology       | Purpose                                   |
| ---------------- | ----------------------------------------- |
| AWS              | Cloud infrastructure                      |
| Terraform        | Infrastructure as Code                    |
| Amazon VPC       | Isolated network environment              |
| Public Subnet    | Network for publicly accessible resources |
| Internet Gateway | Internet connectivity                     |
| Route Table      | Controls network traffic routing          |
| Security Group   | Controls inbound/outbound traffic         |
| Amazon EC2       | Virtual server                            |
| Linux            | Operating system                          |
| Nginx            | Web server                                |
| Git/GitHub       | Version control and documentation         |

# Step-by-Step Implementation

## Step 1: Configure the Terraform Provider

The first step was to configure Terraform to work with AWS.

The AWS provider tells Terraform that AWS is the cloud platform where the infrastructure should be created. Terraform itself does not know how to communicate with AWS.

The AWS provider allows Terraform to interact with AWS APIs and create resources such as:

* VPCs
* Subnets
* EC2 instances
* Security Groups
* Route Tables
* Internet Gateways

# Step 2: Create Terraform Variables

Instead of hardcoding configuration values directly inside the resources, I created variables. Variables make Terraform configurations:

* Reusable
* Flexible
* Easier to maintain
* Easier to modify
* Suitable for different environments

# Step 3: Define Variable Values

The actual values for the variables were stored in `terraform.tfvar`.** AMI IDs are region-specific. An AMI available in one AWS region may not exist in another region.**

# Step 4: Create the VPC

The first AWS networking resource created was the VPC.

### What is a VPC?

A **Virtual Private Cloud (VPC)** is an isolated virtual network within AWS.

It provides the foundation for networking resources such as:

* Subnets
* Route Tables
* Internet Gateways
* Security Groups
* EC2 instances

### CIDR Block

The VPC uses:

```text
10.0.0.0/16
```

This provides a large private IP address range that can be divided into smaller subnets.

# Step 5: Create the Public Subnet

The next step was to create a subnet inside the VPC.

The subnet uses:

10.0.1.0/24


### What is a subnet?

A subnet is a smaller network segment inside a VPC. The subnet was created as a **public subnet** because it will have a route to the Internet Gateway. The following configuration also allows instances launched into the subnet to receive public IPv4 addresses

# Step 6: Create the Internet Gateway

The Internet Gateway provides a path between the VPC and the internet.

### What does the Internet Gateway do?

The Internet Gateway allows resources in the VPC to communicate with the internet when the appropriate routing and public addressing are configured. A route to the Internet Gateway must also be configured.

# Step 7: Create the Public Route Table

The next step was to create a route table.

The important route is:

0.0.0.0/0 → Internet Gateway


### What does `0.0.0.0/0` mean?

It represents traffic destined for any IPv4 address that is not covered by a more specific route. Therefore, traffic from the subnet destined for the internet is sent through the Internet Gateway.

# Step 8: Associate the Route Table with the Public Subnet

Creating a route table is not enough. The route table must be associated with the subnet. This association means that the public subnet 

# Step 9: Create the Security Group

The Security Group controls network traffic to the EC2 instance.

SSH access should ideally be restricted to your own public IP rather than allowing

0.0.0.0/0


HTTP port 80 can be publicly accessible because the purpose of the server is to serve a web page.

# Step 10: Create the EC2 Instance

After creating the networking infrastructure, the EC2 instance was provisioned inside the public subnet. The EC2 instance receives:

* An AMI
* An instance type
* A subnet
* A Security Group

The EC2 instance is therefore connected to the network created earlier.

# Step 11: Install Nginx

Nginx was installed automatically using EC2 `user_data`.

### What is Nginx?

Nginx is a web server that can serve websites and web applications.

After installation, Nginx listens for HTTP traffic on port 80.

#  Step 12: Initialize Terraform

Before creating the infrastructure, Terraform needs to download and initialize the required provider.

Run:

terraform init


Terraform initializes the working directory and downloads the required provider plugins.

Expected result:

Terraform has been successfully initialized!

# Step 13: Validate the Terraform Configuration

Before applying the configuration, validate the Terraform files.

Run:

terraform validate


This checks whether the Terraform configuration is syntactically valid and internally consistent.

Expected result:

Success! The configuration is valid.


# Step 14: Format the Terraform Code

Terraform provides a formatting command:


terraform fmt


This automatically formats Terraform files according to Terraform's standard formatting conventions.

This helps keep the code clean and consistent.

# Step 15: Review the Terraform Plan

Before creating anything in AWS, I reviewed the execution plan.

Run:

terraform plan

Terraform shows the resources that it intends to create.

For this project, Terraform should identify resources such as:

VPC
Subnet
Internet Gateway
Route Table
Route Table Association
Security Group
EC2 Instance
```

This step is important because it allows you to review the proposed infrastructure before Terraform makes changes.

# Step 16: Deploy the Infrastructure

After reviewing the plan, the infrastructure can be created with:

terraform apply

Terraform will ask for confirmation.

Enter:

yes

Terraform then communicates with AWS and creates the infrastructure defined in the configuration.

# Step 17: Verify the Infrastructure in AWS

After Terraform completes successfully, I verified the resources in the AWS Management Console.

The following resources were checked:

### VPC

Confirmed that the VPC was created with:

10.0.0.0/16
### Public Subnet

Confirmed that the subnet was created with:


10.0.1.0/24

and located in the configured Availability Zone.

### Internet Gateway

Confirmed that the Internet Gateway was attached to the VPC.

### Route Table

Confirmed that the route table contained:

0.0.0.0/0 → Internet Gateway

### Security Group

Confirmed that the required inbound and outbound rules were configured.

### EC2

Confirmed that the EC2 instance was running inside the public subnet.

# Step 18: Test the Nginx Web Server

After the EC2 instance was running, I obtained its public IP address.I then opened the public IP address in a web browser:

http://EC2-PUBLIC-IP


If everything was configured correctly, the Nginx default page was displayed. This confirmed that the complete infrastructure was working.

#
