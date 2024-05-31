# Hosting a Simple Webpage with Amazon EC2

In this project, we are going to create a simple web page using .html and publish it on an AWS EC2 instance using the open-source Apache web server. 

Our task order will be as follows:

* Login to AWS Console
* Configure and launch an EC2 instance
* ssh into the EC2 instance
* Install the Apache web server on the EC2 instance
* Create a simple .html webpage and publish it to the instance

### Network Architecture Diagram

<img width="800" alt="image" src="https://github.com/blacksmith121/AWS_Projects/blob/main/Website%20Hosting%20with%20EC2/9.jpg?raw=true)">

## Step 1 - Login to AWS Console

Create an AWS account and log in using the credentials you created. Follow the link below to set up an AWS account if you have not already.

https://portal.aws.amazon.com/billing/signup?type=enterprise#/start/email 

## Step 2 - Configure and launch an EC2 instance

First, we need to provision a VPC. VPCs are important because they offer an isolated section of the AWS cloud where you can launch AWS resources that you define. This ensures that the resources are separated and secure from other AWS customers. A VPC also allows you to provision security groups and network access control lists (NACLS) to further enhance security with a layered approach. 

Navigate to VPC by typing "VPC" in the search bar and clicking VPC. Under the "Actions" button click "Create default VPC". Your default VPC will be created with the necessary subnets and should look similar to what I have below. 

<img width="800" alt="image" src="https://github.com/blacksmith121/AWS_Projects/blob/main/Website%20Hosting%20with%20EC2/9.jpg?raw=true)">
