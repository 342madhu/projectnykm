Step 2: Define Variables in variables.tf
variable "aws_region" {
  description = "AWS region where resources will be deployed"
}

variable "vpc_cidr" {
  description = "CIDR block for the VPC"
}

variable "public_subnet_cidr" {
  description = "CIDR block for the public subnet"
}

variable "private_subnet_cidr" {
  description = "CIDR block for the private subnet"
}

variable "ami_id" {
  description = "Latest AWS AMI ID for the web server"
}

# Add more variables as needed

Step 3: VPC Configuration in vpc.tf
resource "aws_vpc" "main" {
  cidr_block = var.vpc_cidr
  enable_dns_support = true
  enable_dns_hostnames = true
}

resource "aws_subnet" "public" {
  vpc_id                  = aws_vpc.main.id
  cidr_block              = var.public_subnet_cidr
  availability_zone       = "${var.aws_region}a"
  map_public_ip_on_launch = true
}

resource "aws_subnet" "private" {
  vpc_id                  = aws_vpc.main.id
  cidr_block              = var.private_subnet_cidr
  availability_zone       = "${var.aws_region}b"
}Step 4: Security Group Configuration in security_group.tf
resource "aws_security_group" "web_sg" {
  vpc_id = aws_vpc.main.id

  # Define security group rules as needed for web application
}
Step 5: Load Balancer Configuration in load_balancer.tf
resource "aws_lb" "web_lb" {
  name               = "web-lb"
  internal           = false
  load_balancer_type = "application"
  security_groups    = [aws_security_group.web_sg.id]
  subnets            = [aws_subnet.public.id]
}
# Add listener, target group, and other configurations as needed
Step 6: Autoscaling Group Configuration in autoscaling.tf
resource "aws_launch_configuration" "web_lc" {
  image_id = var.ami_id

  # Define instance type, key name, user data script, and other configurations
}

resource "aws_autoscaling_group" "web_asg" {
  desired_capacity     = 2
  max_size             = 5
  min_size             = 1
  vpc_zone_identifier = [aws_subnet.private.id]
  launch_configuration = aws_launch_configuration.web_lc.id
  health_check_type          = "ELB"
  health_check_grace_period  = 300
  force_delete               = true
  wait_for_capacity_timeout = "0"
} } 
Step 7: User Data Script and Ansible Playbook
Create user_data.sh to install and configure the web server using Ansible.
#!/bin/bash
# Add commands to install and configure the web server using Ansible
Create ansible_playbook.yml with your Ansible playbook.
Step 8: Route 53 Private Hosted Zone and Certificate Configuration
resource "aws_route53_zone" "private_zone" {
  name            = "example.com"
  vpc {
    vpc_id = aws_vpc.main.id
  }
}

resource "aws_acm_certificate" "web_certificate" {
  domain_name       = "test.example.com"
  validation_method = "DNS"
  tags = {
    Name = "web_certificate"
  }
}
