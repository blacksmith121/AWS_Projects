# Use Amazon Inspector to Run a Vulnerability Assessment on an EC2 Instance

In this project, I will guide you through the process of using Cloudwatch to monitor resources, create alarms and dashboards. 
CloudWatch enables you to monitor your complete stack (applications, infrastructure, network, and services) and use alarms, logs, and events data to take automated actions and reduce mean time to resolution (MTTR). This frees up important resources and allows you to focus on building applications and business value.

Our task order will be as follows:

* Login to AWS Console
* Configure and Launch an EC2 instance
* ssh into the EC2 instance
* Create SNS Topic
* Subscribe to the SNS Topic
* Use Cloudwatch to Check Ec2 CPU Utilization Metrics
* Create Cloudwatch Alarm
* Test Cloudwatch Alarm
* Check email from SNS Topic
* Check Cloudwatch Alarm Graph
* Create Cloudwatch Dashboard

## Step 1 - Login to AWS Console

Create an AWS account and log in using the credentials you created. Follow the link below to set up an AWS account if you have not already.

https://portal.aws.amazon.com/billing/signup?type=enterprise#/start/email 

## Step 2 - Configure and launch an EC2 Instance

In the "Services" menu navigate to EC2 or type "EC2" in the search bar to get to the EC2 menu. Click on the "Instances" option on the left panel and click the "Launch Instances" button. 
Name your instance whatever you like and stick to the default AMIs and Instance types. For this demo, we are using the Amazon Linux 2 AMI and the t2.micro Instance type. Both are AWS free tier eligible. 

See Example below:

<img width="30000" alt="image" src="https://github.com/blacksmith121/AWS_Projects/assets/83726405/4af85008-7642-40ae-97c5-96a57899c717)">

Scroll down to "Network Settings" then configure the following settings:

* Auto-assign public IP: Enable
* Select Create Security group
* Security group name: "Whatever name you decide"
* Description: Brief description of what this security group is used for
* Choose Type: select SSH
* Source: Select Anywhere

Keep everything else default and click Launch Instance. 

## Step 3 - SSH into the EC2 instance

The commands below will install the Stress tool, a command line tool available for Linux-based operating systems to evaluate the performance and reliability of a system under a variety of workloads by introducing stress (heavy load) to the system. Once the CloudWatch alarm is created, we will ssh back into our EC2 instance and simulate a heavy load on the system to trigger an alarm. 

* Switches to root user VERY IMPORTANT:

```
sudo su 
```

* Runs updates:

```
yum -y update
```

* Installs Stress tool:

```
sudo amazon-linux-extras install epel -y

or

yum install stress -y
```

## Step 4 - Create SNS Topic

