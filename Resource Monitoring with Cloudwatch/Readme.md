# Resource Monitoring with Cloudwatch

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

Navigate to Simple Notification Service by typing "SNS" in the search bar. 

Click topics in the left panel then the "Create Topic" button. 

<img width="30000" alt="image" src="https://github.com/blacksmith121/AWS_Projects/blob/main/Resource%20Monitoring%20with%20Cloudwatch/3.png?raw=true)">

Under "Details" :

Type: Standard

Name: Whatever you like

Display Name: Whatever you like

<img width="30000" alt="image" src="https://github.com/blacksmith121/AWS_Projects/blob/main/Resource%20Monitoring%20with%20Cloudwatch/4.png?raw=true)">

Leave everything else default and click the "Create Topic" button. Your SNS topic will be created! 

<img width="30000" alt="image" src="https://github.com/blacksmith121/AWS_Projects/blob/main/Resource%20Monitoring%20with%20Cloudwatch/5.png?raw=true)">

## Step 5 - Subscribe to the SNS Topic

Once your SNS topic is created, Click on the SNS topic you just created. Then click on the "Create subscription" button.

Under Details populate the following information:

Protocol: Select "Email"
Endpoint: Enter your email address (This is where your notification will be delivered.)

You will receive a confirmation page on the AWS console. 

<img width="30000" alt="image" src="https://github.com/blacksmith121/AWS_Projects/blob/main/Resource%20Monitoring%20with%20Cloudwatch/6.png?raw=true)">

Check the email address you registered, and you should see a subscription confirmation email. Click "Confirm Subscription"

<img width="30000" alt="image" src="https://github.com/blacksmith121/AWS_Projects/blob/main/Resource%20Monitoring%20with%20Cloudwatch/7.png?raw=true)">

If you receive the message below, then your email has successfully subscribed to the SNS topic you created!

<img width="30000" alt="image" src="https://github.com/blacksmith121/AWS_Projects/blob/main/Resource%20Monitoring%20with%20Cloudwatch/8.png?raw=true)">

## Step 6 - Use Cloudwatch to Check EC2 CPU Utilization Metrics

Navigate to Cloudwatch in the services menu by using the search bar. Click all metrics in the left panel. 

<img width="30000" alt="image" src="https://github.com/blacksmith121/AWS_Projects/blob/main/Resource%20Monitoring%20with%20Cloudwatch/9.png?raw=true)">

Here, you should see all available metrics that can be tracked via CLoudwatch. The metric we will track in this tutorial will be EC2. 

<img width="30000" alt="image" src="https://github.com/blacksmith121/AWS_Projects/blob/main/Resource%20Monitoring%20with%20Cloudwatch/10.png?raw=true)">

Click "EC2" ---> Per-instance Metrics.

In this menu, you can see a list of different metrics for your EC2 instance. Select the "CPUUtilization" metric as shown below. 

<img width="30000" alt="image" src="https://github.com/blacksmith121/AWS_Projects/blob/main/Resource%20Monitoring%20with%20Cloudwatch/11.png?raw=true)">

Once selected, you will see a graph of CPU utilization, in this case, there won't be much activity shown on the graph.

## Step 7 - Create Cloudwatch Alarm

Now we will use a Cloudwatch alarm to watch the CPU Utilization metric.

Click "Alarms" in the left-hand panel of the Cloudwatch dashboard then "Create Alarm"

The Specify Metric and Conditions page will open:

* Select Metrics: Select EC2
* Select per-instance metrics
* Select your EC2 instance (You can search for it by using your EC2 Instance- ID)
* Select CPUUtilization metric
* Click Select Metric

See example below:

<img width="30000" alt="image" src="https://github.com/blacksmith121/AWS_Projects/blob/main/Resource%20Monitoring%20with%20Cloudwatch/12.png?raw=true)">

You will be directed to the "Specify Metric and Conditions" page. 

* Metric name: Whatever you like
* Statistic: Average
* Period: 1 -5 minutes
* Threshold type: Static
* Whenever CPUUtilization is: Choose Greater
* than...: Enter 30

Leave everything else as default and click "Next"

<img width="30000" alt="image" src="https://github.com/blacksmith121/AWS_Projects/blob/main/Resource%20Monitoring%20with%20Cloudwatch/13.png?raw=true)">

Configure Actions:

* Alarm state trigger: In alarm
* Send a notification to the following SNS topic: Select an existing SNS topic
* Send a notification to...: Choose the SNS topic you created 

<img width="30000" alt="image" src="https://github.com/blacksmith121/AWS_Projects/blob/main/Resource%20Monitoring%20with%20Cloudwatch/14.png?raw=true)">


