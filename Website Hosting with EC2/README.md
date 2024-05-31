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

Navigate to VPC by typing "VPC" in the search bar and clicking VPC. Under the "Actions" button click "Create default VPC". Your default VPC will be created with the necessary subnets and should look similar to what I have below. (State should say "Available")

<img width="30000" alt="image" src="https://github.com/blacksmith121/AWS_Projects/blob/main/Website%20Hosting%20with%20EC2/1.png?raw=true)">

Now let's launch our EC2 instance! 

In the "Services" menu navigate to EC2 or type "EC2" in the search bar to get to the EC2 menu. Click on the "Instances" option on the left panel and click the "Launch Instances" button. 
Name your instance whatever you like and stick to the default AMIs and Instance types. For this demo, we are using the Amazon Linux 2 AMI and the t2.micro Instance type. Both are AWS free tier eligible. 

See Example below:

<img width="30000" alt="image" src="https://github.com/blacksmith121/AWS_Projects/assets/83726405/4af85008-7642-40ae-97c5-96a57899c717)">

Don't leave this menu yet! Time to configure the Security groups. Scroll down to "Network Settings", ensure your default VPC is selected then configure the following settings:

* Auto-assign public IP: Enable
* Select Create Security group
* Security group name: "Whatever name you decide"
* Description: Brief description of what this security group is used for

 Now time to add security group rules. A security group is a virtual firewall in AWS that controls outbound and inbound traffic in a cloud computing environment. 

 * For HTTP, Select Add security group rule Button
 * Choose Type: Select HTTP
 * Source: Select Anywhere
 * Do the same for the ssh protocol.

See the below screenshot for a reference of Network and security group configuration:

<img width="30000" alt="image" src="https://github.com/blacksmith121/AWS_Projects/blob/main/Website%20Hosting%20with%20EC2/3.png?raw=true)">

Click the "Launch Instance" button and proceed to the next step!

## Step 3 - SSH into the EC2 instance

Navigate to the EC2 instance page and ensure your EC2 instance's state is running with 2/2 status checks passed. 

<img width="30000" alt="image" src="https://github.com/blacksmith121/AWS_Projects/blob/main/Website%20Hosting%20with%20EC2/4.png?raw=true)">

Select the EC2 Instance Connect option and click on the "Connect: button. (Keep everything else as default). A new tab will open in your browser where you can execute Linux commands. Congrats you have successfully connected to your EC2 instance via SSH! 

## Step 4 - Install the Apache web server on the EC2 instance

The commands below will configure your EC2 instance to run an Apache web server and verify that it's up and running:

* Switches to root user VERY IMPORTANT:

```
sudo su 
```

* Runs updates:

```
yum -y update
```

* Installs the Apache web server:

```
yum install httpd -y
```

* Starts the web server:

```
systemctl start httpd
```

* Enables the Apache web server

```
systemctl enable httpd
```

* Checks the status of the Apache web server (Should say "active" in the command line

```
systemctl status httpd
```

 Now paste the public IPv4 address of your EC2 instance in a web browser and you should see the Apache Test page. If you can see the page below in your browser, that means the Apache server is installed and working properly.

<img width="30000" alt="image" src="https://github.com/blacksmith121/AWS_Projects/blob/main/Website%20Hosting%20with%20EC2/5.png?raw=true)">
 
## Step 5 - Create a Simple .html Webpage and Publish it to the Instance

We made it to the last step! Now we will add content to the index.html file in our EC2 instance using the command below:

```
echo "<html>This is a test webpage hosted on Apache webserver</html>" > /var/www/html/index.html
```
Use this command to restart the webserver:

```
systemctl restart httpd
```

Finally, enter the public IPv4 address of your EC2 instance followed by /index.html. Make sure the URL protocol is http and not https. 

Example URL: HTTP://54.173.203.101/index.html 

The result in your browser should look something like the screenshot below:

<img width="30000" alt="image" src="https://github.com/blacksmith121/AWS_Projects/blob/main/Website%20Hosting%20with%20EC2/7.png?raw=true)">

That is how you host a very simple webpage using AWS. Thank you for reading! 
