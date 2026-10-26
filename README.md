# AWS EC2 Auto Scaling with Application Load Balancer

## Project Overview

This project demonstrates how to host a static website on an Amazon EC2 instance using Nginx and implement automatic scaling using an Auto Scaling Group (ASG) and an Application Load Balancer (ALB).

An Amazon Machine Image (AMI) is created from the configured EC2 instance and used in a Launch Template. The Auto Scaling Group launches additional instances based on CPU utilization, while the Application Load Balancer distributes incoming HTTP traffic across healthy instances.

## AWS Services and Tools Used

- **Amazon EC2** – Virtual servers for hosting the website.
- **Nginx** – Web server used to serve the static website.
- **Amazon Machine Image (AMI)** – Image of the configured EC2 instance.
- **Launch Template** – Defines the configuration for launching instances.
- **Auto Scaling Group (ASG)** – Automatically adjusts the number of EC2 instances.
- **Application Load Balancer (ALB)** – Distributes incoming requests.
- **Target Group** – Registers instances and monitors their health.
- **Stress Tool** – Generates CPU load to test scaling behavior.

## Architecture
                Users
                  |
                  v
        Application Load Balancer
                  |
                  v
             Target Group
                  |
                  v
          Auto Scaling Group
             /     |     \
            v      v      v
          EC2-1  EC2-2  EC2-N
            |
            v
      Nginx Web Server
            |
            v
       Static Website

## Project Implementation

### Step 1: Launch an EC2 Instance

- Launch an Ubuntu EC2 instance.
- Select an instance type, key pair, storage, and security group.
- Connect to the instance using EC2 Instance Connect.

### Step 2: Install Nginx and Deploy the Website

Install the required packages and Nginx:


sudo apt update
sudo apt install nginx unzip wget -y


Download and extract a static website template:

wget -O website.zip "<WEBSITE_TEMPLATE_DOWNLOAD_URL>"
unzip website.zip

Copy the extracted website files into the Nginx web directory:

sudo cp -r <EXTRACTED_FOLDER>/* /var/www/html/
sudo systemctl enable --now nginx

Allow inbound HTTP traffic on port 80 and verify the website using the EC2 public IP address.

### Step 3: Create an AMI

- Select the configured EC2 instance.
- Choose **Actions → Image and templates → Create image**.
- Create the AMI and wait until its status becomes **Available**.

### Step 4: Create a Launch Template

- Open EC2 Launch Templates.
- Select the created AMI.
- Configure the instance type, key pair, security group, and storage.
- Create the Launch Template.

### Step 5: Configure the Application Load Balancer

- Create a target group using HTTP on port 80.
- Configure the health check.
- Create an internet-facing Application Load Balancer.
- Configure its HTTP listener to forward traffic to the target group.

### Step 6: Create an Auto Scaling Group

- Create an Auto Scaling Group using the Launch Template.
- Select the required VPC and subnets across Availability Zones.
- Attach the target group to the Auto Scaling Group.
- Configure minimum, desired, and maximum instance capacity.
- Configure a target-tracking scaling policy based on average CPU utilization.

### Step 7: Test Auto Scaling

Install the Stress tool on an EC2 instance managed by the Auto Scaling Group:

sudo apt update
sudo apt install stress -y

Generate CPU load:

stress --cpu 2 --timeout 300

Monitor CPU utilization, Auto Scaling activity, and the number of running instances in the AWS Console.

### Step 8: Verify the Deployment

- Access the website through the ALB DNS name.
- Verify the target group health status.
- Monitor Auto Scaling activity during the CPU load test.
- Confirm that additional instances launch if the scaling policy conditions are met.

## Screenshots

The `screenshots/` directory can contain evidence of the following stages:

- EC2 instance setup
- Website hosted using Nginx
- AMI creation
- Launch Template configuration
- Auto Scaling Group configuration
- Load Balancer and target group
- CPU stress test and scaling activity

Add screenshots using the actual filenames in your repository.

## Expected Outcome

- A static website hosted on an Ubuntu EC2 instance using Nginx.
- An AMI and Launch Template for launching consistent instances.
- An Auto Scaling Group configured to adjust EC2 capacity.
- An Application Load Balancer configured to distribute HTTP traffic.
- CPU load testing to observe Auto Scaling behavior.

*Document actual test results separately; successful scale-out should only be claimed after verifying that additional instances launched.*

## Cleanup

To avoid unnecessary AWS charges after completing the lab:

- Delete the Auto Scaling Group and its managed instances.
- Delete the Application Load Balancer.
- Delete the target group when no longer in use.
- Delete the Launch Template if no longer required.
- Deregister unused AMIs and delete their associated snapshots if no longer needed.
- Terminate any remaining EC2 instances.

## Lab Report

The detailed lab report is available in `AWS LAB REPORT.pdf`.
