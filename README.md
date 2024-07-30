AWS Infrastructure Setup Documentation

Overview
This Terraform configuration defines an AWS infrastructure setup that includes a Virtual Private Cloud (VPC), subnets, security groups, an ECS (Elastic Container Service) cluster, two Fargate task definitions, two ECS services and an application load balancer for high availability. This setup enables hosting HTML web pages on AWS with the help of an application load balancer.

Prerequisites
•	Terraform: Installed on your local machine.
•	AWS Account: An AWS account with appropriate permissions to create the specified resources.
•	Variables Configuration: A configured variables.tf file containing values for the following variables:
o	vpc_cidr: The CIDR block for the VPC.
o	public_subnet_cidr_1: The CIDR block for the first public subnet.
o	public_subnet_cidr_2: The CIDR block for the second public subnet.
o	private_subnet_cidr: The CIDR block for the private subnet.
o	aws_region: The AWS region where the resources will be created
o	cluster_name: The name of the ECS cluster.
o	task_definition_family: The family name for the ECS task definitions.
o	service_name: The name of the ECS service.
o	alb_name: The name of the Application Load Balancer.
o	target_group_name: The name of the target group.
 
Resources Created
1.	VPC
o	Resource Type: aws_vpc
o	Description: Creates a VPC to host the resources, providing a secure and isolated network.
3.	Internet Gateway
o	Resource Type: aws_internet_gateway
o	Description: Creates an internet gateway to provide internet access to the VPC, allowing communication with external networks.
4.	Route Table
o	Resource Type: aws_route_table
o	Description: Creates a route table to direct traffic from the VPC to the internet via the internet gateway.
o	Route: Allows outbound traffic to all IP addresses (0.0.0.0/0).
5.	Route Table Association
o	Resource Type: aws_route_table_association
o	Description: Associates the public route table with the public subnet, enabling instances in the public subnet to access the internet.
6.	Public Subnet 1
o	Resource Type: aws_subnet
o	Description: Creates the first public subnet within the VPC.
o	Properties:
	Automatically assigns public IPs to instances launched in this subnet, facilitating internet access for those instances.
7.	Public Subnet 2
o	Resource Type: aws_subnet
o	Description: Creates the second public subnet within the VPC.
8.	Private Subnet
o	Resource Type: aws_subnet
o	Description: Creates a private subnet within the VPC
9.	Security Group
o	Resource Type: aws_security_group
o	Description: Creates a security group that allows inbound HTTP traffic (port 80) and all outbound traffic, ensuring that web traffic can reach the ECS services.
10.	ECS Cluster
o	Resource Type: aws_ecs_cluster
o	Description: Creates an ECS cluster for managing containerized applications, providing a managed environment for running containers.
11.	ECS Task Definition
o	Resource Type: aws_ecs_task_definition
o	Description: Defines a Fargate task with specific container configurations.
o	Properties:
	Family: Defined by the variable task_definition_family.
	Network Mode: awsvpc, enabling network isolation and control.
	CPU: 256 units, specifying the CPU resources for the task.
	Memory: 512 MiB, defining the memory allocation for the task.
	Container Definition: Defines a single container running an HTTP server with a sample HTML page.
12.	ECS Service
o	Resource Type: aws_ecs_service
o	Description: Creates a service for the Fargate task definition, ensuring that the specified number of tasks are running.
o	Properties:
	Desired Count: 1 (indicates one task should be running).
	Launch Type: FARGATE, allowing serverless container management.
	Network Configuration: Uses the public subnets and the defined security group, assigning public IPs to the tasks.
13.	Application Load Balancer (ALB)
o	Resource Type: aws_lb
o	Description: Creates an Application Load Balancer for routing HTTP traffic to the ECS service.
14.	Target Group
o	Resource Type: aws_lb_target_group
o	Description: Defines a target group for the ALB to direct traffic to the ECS service.
Usage
1.	Clone the Repository: Clone the repository containing the Terraform configuration.
2.	Navigate to the Directory: Open your terminal and navigate to the directory containing the Terraform files.
3.	Initialize Terraform: Run the command:
terraform init
4.	Plan the Deployment: Check what resources will be created by running:
terraform plan
5.	Apply the Configuration: Deploy the infrastructure by running:
terraform apply
6.	Verify Resources: Once applied, verify the created resources in the AWS Management Console.
Notes
•	Ensure your AWS credentials are configured properly for Terraform to authenticate and create resources in your account.
•	Review and update the security group rules as needed based on your application requirements.
Cleanup
To remove all the created resources, run:
terraform destroy


