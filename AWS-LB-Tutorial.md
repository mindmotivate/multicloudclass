# An Intro introduction to AWS Load Balancing and Autoscaling in the Cloud:  

[Motivated Mindstate](https://github.com/mindmotivate)


## Greetings future AWS Cloud Engineers! <a name="example-templates-autoscaling-full-stack-template"></a>

Welcome to the most recent installment of our AWS Tutorials!

The following will be a continuation of our last AWS tutorial where we created our first VPC. 
In this tutorial we will be creating a load balancer which will attach to our previous VPC.
If you like to visit the previous tutorial please use the following link: [AWS::VPC](https://github.com/mindmotivate/multicloudclass/blob/main/aws-vpc-tutorial.md)

Ok, now that we've gotten the formalities out of the way...
Here is more information than you ever wanted to know about Autoscaling, Load Balancing, NAT's, NIC's, ASG's and other Three-letter acronyms...lol 
(Not interested in the technical aspects? Click this link to skip the "tech talk" and get right down to business:([Let's Begin!](https://github.com/mindmotivate/multicloudclass/blob/main/AWS-LB-Tutorial.md#lets-begin)) 

# So what does "Autoscaling" mean?

*Autoscaling* is the process of automatically adjusting the number of computing resources that are allocated to an application or service based on demand. This can help to ensure that your application is always available and performant, even when traffic spikes!

Autoscaling is a critical part of cloud computing, as it allows you to scale your infrastructure up or down without having to manually provision or deprovision resources. This can save you time and money, and it can also help to improve the reliability and performance of your applications. Autoscaling is an essential part of cloud computing!

# What are "Load Balancers" for exactly?.....

A Load Balancer is a tool, commonly used with autoscaling to ensure that your applications are always available to perform at their best! For example, you can configure an ALB to distribute traffic across a set of EC2 instances that are managed by an autoscaling group. The autoscaling group will automatically add or remove EC2 instances from the group based on the traffic load. This ensures that your application always has enough resources to handle the current load, without having to manually provision or deprovision resources.

*Application Load Balancers (ALBs)* are a type of load balancer that is designed for **HTTP** and **HTTPS** traffic. ALBs can be used to distribute traffic across multiple EC2 instances, ECS containers, or AWS Lambda functions.

The load balancer will receive web requests and send them to the backend pool via their public IP addresses, on port 80 by default. This is because the load balancer is located in a public subnet and needs to be able to communicate with the backend servers, which are located in a private subnet.

The backend servers in a private subnet will have a public IP address assigned to them. This IP address is used for communication with the load balancer. The backend servers will not be directly accessible from the public web. Instead, all traffic to the backend servers must be routed through the load balancer.

Here is a diagram of the network architecture: **Internet -> Load Balancer -> Backend Servers (EC2 Instances)**
<br>

![lbs](https://github.com/mindmotivate/multicloudclass/assets/130941970/2d82890c-0bec-42d8-83c2-6ba8df5b9853)


> *Note: This is an example of an "External" Load Balancer. There are also Internal Load Balancers, which we will demonstrate in a future tutorial.*

When a client sends a request to the load balancer, the load balancer will translate the client's public IP address to the public IP address of one of the backend servers. The load balancer will then forward the request to the selected backend server.

The backend server will process the request and generate a response. The backend server will then send the response back to the load balancer. The load balancer will then forward the response back to the client.

This process ensures that all traffic to the backend servers is routed through the load balancer. This helps to improve the security and reliability of the backend servers.

> Note: The load balancer can be configured to use *HTTPS* instead of *HTTP*. This is a more secure way to communicate with the backend servers. However, it will require the backend servers to be configured with SSL certificates.

# But what if one of the "Backend Servers" wants to communicate with the Public Internet?

Glad you you asked! This is where the NAT Gateway comes into play. NAT stands for Network Address Translation. It is a method of mapping an IP address space into another by modifying network address information in the IP header of packets while they are in transit across a traffic routing device.

If a server in a private subnet wants to communicate with the web, it must go through the NAT gateway first. This is because the server does not have a public IP address and cannot communicate directly with the web. The NAT gateway will translate the server's private IP address to a public IP address. The server will then use this public IP address to communicate with the web via an Internet Gateway. 

Although the internet gateway and NAT gateway are two different services, you can use them together in your VPC architecture. The internet gateway would be used to route traffic between your VPC and the internet. The NAT gateway would be used to route traffic between private subnets and the internet.


# Simple Diagram:
Here is a diagram of the network architecture:

![nat-instance_updated](https://github.com/mindmotivate/multicloudclass/assets/130941970/95f5e5ce-1c10-44ca-b594-664abb9b3301)

Path Shown: **Server (in Private Subnet) -> NAT Gateway -> Internet Gateway -> Internet**

The server will initiate the communication with the web by sending a request to the NAT gateway. The NAT gateway will translate the server's private IP address to a public IP address and then forward the request to the web.

One more thing...
It is important to note that some organizations may choose to block all outbound traffic from servers in private subnets, except for traffic that is routed through the load balancer. This is done to improve security and control.

# Ok so what is this "NIC" I hear so much about?
NIC is the Network Interface Card, Here's how it comes into play....
A client sends a request to the load balancer's public IP address.
The load balancer forwards the request to one of the servers in the target group/backend pool.
The server receives the request on its NIC.
The server processes the request and generates a response.
The server sends the response back to the load balancer on its NIC.
The load balancer forwards the response back to the client.
The server's NIC is essential for this process because it allows the server to communicate with the load balancer. Without the NIC, the server would not be able to send or receive traffic to or from the load balancer. NICs are assigned to the instance when it is launched, and can be modified or removed at any time.

# Important Terminology:

> **Load balancer:** A load balancer is a device that distributes traffic across multiple servers. This can improve performance and reliability by preventing any one server from being overloaded.<br>
> **Backend servers:** The backend servers are the web servers that host your website or application. They are located in a private subnet, which means that they are not directly accessible from the internet.<br>
> **NAT gateway:** A NAT gateway is a device that translates private IP addresses to a public IP address. This allows private subnet instances to communicate with the internet and other public resources.<br>
> **NIC** A NIC is a virtual network interface that allows an EC2 instance to communicate with other instances on the same network, as well as with the internet and other public resources. Each EC2 instance has at least one NIC<br>
> **Internet Gateway:** The internet gateway is the main entry point for traffic into your VPC from the internet. It is a highly available device that routes traffic between your VPC and the internet.<br>


# Auto Scale Scenario:

Let's say you have a successful web application running on a single EC2 instance. As traffic increases, you need a solution that can handle the load without breaking the bank. You discover that AWS offers an auto scaling Solution with its ALB (Application Load Balancer). The ALB will distribute traffic across multiple EC2 instances. Auto Scaling automatically scales the number of EC2 instances in a group up or down based on traffic load.

So you you proceed to create an ALB and configure it to distribute traffic to your EC2 instance. You then create an Auto Scaling group and configure it to manage your EC2 instance. You configure the Auto Scaling group to scale up or down the number of EC2 instances based on traffic load.
Now all of the sudden, your web application is always available, even when traffic spikes.Your web application can automatically scale up or down to meet demand.
The best part is... you only pay for the EC2 instances that you need!

You now have a web application running on a single EC2 instance in a VPC. You created an ALB and configure it to distribute traffic to your EC2 instance. You then create an Auto Scaling group and configure it to manage your EC2 instance. You configure the Auto Scaling group to scale up to two EC2 instances when traffic load exceeds a certain threshold.  When traffic to your web application spikes, the Auto Scaling group will automatically add another EC2 instance to the group. The ALB will then distribute traffic across the two EC2 instances. This ensures that your web application is always available and performant, even when traffic spikes...Hooray!

Now that we understand the overall concepts, let's demonstrate this in AWS with our previosly created VPC

## Let's Begin! ##
> ### Note: We are beggining this tutorial with an established VPC! ###


# 1. Create a security group for the VPC

1. In the Amazon EC2 console, navigate to **Security Groups**.
2. Choose **Create Security Group**.
3. Enter a name and description for the security group.
4. For **VPC**, select the VPC you *previoulsy* created.<br>
> *On VPC click on the x and replace the VPC that is labeled “TouchItAndDie” replace it with the VPC that you created<*<br>![clickx](https://github.com/mindmotivate/multicloudclass/assets/130941970/3d5478e0-74c4-490c-9ac1-f3f49b067399)

> ![vpcnsg](https://github.com/mindmotivate/multicloudclass/assets/130941970/da252db6-8eb0-47e0-994a-30a4af26cfdd)

5. Add inbound and outbound rules to allow the traffic that you need for your application.<br>
> *We will be creating Three: SSH, HTTP, RDP<br>*
6. Scroll down to Inbound rules and click on Add rule
7. Click on the dropdown under Type and select SSH
> *Repeat the same steps after clicking Add rule but select HTTP, then afterwards select RDP<br>
8. For each rule under the Source tabs click on the drop down and select Anywhere-IPv4
9. Give each of your rule a description under the Description tabs<br>![vpcsecurity](https://github.com/mindmotivate/multicloudclass/assets/130941970/fa7955be-d86d-4121-8efb-ad7445c4c10b)

***Next: PLEASE SKIP THE OUTBOUND RULES***<br>

10. Add descriptive Tags <br>
11. Choose **Create**.<br>

# 2. Create a security group for the Load Balancer

1. In the Amazon EC2 console, navigate to **Security Groups**.
2. Choose **Create Security Group**.
3. Enter a name and description for the security group.
4. For **VPC**, select the VPC you created.
> *On VPC click on the x and replace the VPC that is labeled “TouchItAndDie” replace it
with the VPC that you created*
5. For **Inbound Rules**, add a rule to allow traffic on port 80 from all sources.
6. Click on the dropdown under Type and select HTTP
7. For "Source", click on the drop down and select Anywhere-IPv4
8. Provide a description under the Description tab<br>![LBsecuritygroupdetails](https://github.com/mindmotivate/multicloudclass/assets/130941970/c9426542-6049-46fb-95c2-d7408cdf4954)

***Next: PLEASE SKIP THE OUTBOUND RULES***<br>
9. Add descriptive tags as needed<br>
10. Lastly, Choose **Create** to Create security group<br>
> *Click on Security group on the top of the screen to verify both security groups* <br>![confirmsgs](https://github.com/mindmotivate/multicloudclass/assets/130941970/80ca0b5b-5a4b-4c5c-9ae7-1d5d43515c50)

![confirmsg3](https://github.com/mindmotivate/multicloudclass/assets/130941970/57ecf7da-4639-4e42-9845-16b9e5c799c4)

# 3. Create a Launch Template

1. Navigate to the Amazon EC2 console,
2. Choose **Launch Template** from left side menu
3. Click on Create launch templates


> *Under Launch template name type in the name of your template<br>Simply copy and paste the same name into the "description" field<br>Ensure that there is some meaningful naming convention to maintain organization* <br>


  
![Temp DescriptionJPG](https://github.com/mindmotivate/multicloudclass/assets/130941970/2671bb14-82f7-43f9-8427-82e5c8cfa3d5)


4. Under "Application and OS Images" choose "Quick Start"
Select the **Amazon Linux 2023 AMI** Image

![tempmahcine](https://github.com/mindmotivate/multicloudclass/assets/130941970/887ab259-aba7-4e60-a85c-92006176add2)

5. Choose "t2.micro" for your instance type
 
![tempimage](https://github.com/mindmotivate/multicloudclass/assets/130941970/b08fb4ac-d8a3-46c9-a7f4-0f9a632a9630)


6. Click on **Create new keypair** type in and label your keypair. Make sure you have RSA /
.pem format then click Create key pair<br>

![launchtemplatekeypair](https://github.com/mindmotivate/multicloudclass/assets/130941970/0dea68bc-9ebf-4174-88d3-f17948e978ea)


***Important Step!***
  - Under **Network settings** make sure the **Subnet** drop dpown menu has the option ***Don’t include in launch
template***![donotincude](https://github.com/mindmotivate/multicloudclass/assets/130941970/86a4284c-4011-4164-868c-077daaaa4e45)

7. Under the **Firewall (security groups)** Select ***existing security group*** click on the drop
down and choose the VPC security group that you will attach to your server (ex: ASG01-SG01-Server)
 Click the dropdown on **Advanced network configuration**
> *Here we will set up our Network Interface*
> Navigate to Auto-assign public IP click on the dropdown and choose ***Disable***

9. You do not want your instances to be accessible from the public internet

![advancednetworkconfig](https://github.com/mindmotivate/multicloudclass/assets/130941970/9611492d-c714-4ec2-b9a7-54ec2df98637)

10. Under the **Security Group** category - Make sure the VPC security group you attach to your server is selected
11. Scroll all the way down and click on **Advanced details** then scroll all the way down to
**User data** and click on **Choose file**
  Copy and paste the following script in the **User Data" field<br>

> Please copy and paste the following script in the box:<br>
> **Copy Launch Script Below:**\.

```
#!/bin/bash
# Use this for your user data (script from top to bottom)
# install httpd (Linux 2 version)
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd

# Get the IMDSv2 token
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")

# Background the curl requests
curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/local-ipv4 &> /tmp/local_ipv4 &
curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/placement/availability-zone &> /tmp/az &
curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/network/interfaces/macs/ &> /tmp/macid &
wait

macid=$(cat /tmp/macid)
local_ipv4=$(cat /tmp/local_ipv4)
az=$(cat /tmp/az)
vpc=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/network/interfaces/macs/${macid}/vpc-id)

echo "
<!doctype html>
<html lang=\"en\" class=\"h-100\">
<head>
<title>Details for EC2 instance</title>
</head>
<body>
<div>
<h1>AWS Instance Details</h1>
<h1>Samurai Katana</h1>

<br>
# insert an image or GIF
<img src="https://www.w3schools.com/images/w3schools_green.jpg" alt="W3Schools.com">
<br>

<p><b>Instance Name:</b> $(hostname -f) </p>
<p><b>Instance Private Ip Address: </b> ${local_ipv4}</p>
<p><b>Availability Zone: </b> ${az}</p>
<p><b>Virtual Private Cloud (VPC):</b> ${vpc}</p>
</div>
</body>
</html>
" > /var/www/html/index.html

# Clean up the temp files
rm -f /tmp/local_ipv4 /tmp/az /tmp/macid


                                                               
```
12. Add Tags as needed
13. Click on **Create launch template**


# 4. Create a Target Group

1. In the Amazon EC2 console, navigate to the EC2 Dashboard and select **Target Groups**.
2. Choose **Create Target Group**.
3. Choose **Instances** under Basic Configuration.
 
![Tgroupbasic](https://github.com/mindmotivate/multicloudclass/assets/130941970/c5fa0e93-83f2-4ecc-8559-458968d9c3f1)
 
4. For **Target Group Name**, enter a name for the target group.
5. For **Protocol**, select **HTTP**.
6. For **Port**, enter 80.
7. For **IP Address Type** use default IPv4
8. For **Availability Zones**, select all of the availability zones in your region.
9. For **VPC** click on the dropdown on the VPC you created VPC-App02
10. For **Protocal Version**, use default **HTTP1**.

![tgname](https://github.com/mindmotivate/multicloudclass/assets/130941970/13b66183-022f-415b-acca-ec4962df23a0)

12. For **Health Check Protocal**, use default **HTTP**.
13. For **Health Check Path**, use default `/`.
14. Add descriptive tags as needed
15. Click "Next" (Leave all default options on this page)
16. Choose **Create Target Group**.


# 5. Create an AWS Application Load Balancer

1. In the Amazon EC2 console, navigate to **Load Balancers**.
2. Choose **Create Load Balancer**.
3. Choose **Application Load Balancer**.![Applb](https://github.com/mindmotivate/multicloudclass/assets/130941970/8a2144f3-ef42-45fd-9726-88f12b01dd9a)

***COMPLETELY IGNORE THE CLASSIC LOAD BALANCER*** (It is being decomissioned)<br>
4. For **Scheme**, use default internet-facing<br>
5. For **IP address type**, use default IPv4![Basiclbinfo](https://github.com/mindmotivate/multicloudclass/assets/130941970/fec0f286-e8e2-4894-a90e-1de134221d65)

6. Scroll down to the **VPC** category and select the VPC you created earlier
**Important**
7. Under **Mappings** Click and checkbox each Zone and select all on public subnets

![ntwkmap](https://github.com/mindmotivate/multicloudclass/assets/130941970/1ae66356-a621-4321-b918-6824c039b8ee)


8. For **Availability Zones**, select all of the availability zones in your region.
Choose a "Public Subnet" from each AZ

![networkmapper](https://github.com/mindmotivate/multicloudclass/assets/130941970/632f53d1-f3f3-4d51-85a7-8cb5f65a1e64)


> *Your Load balancer will be communicating with the public subnet ranges. When you create a load balancer, you need to specify a subnet for the load balancer's front-end. This subnet is where the load balancer will receive incoming traffic. The load balancer will then distribute this traffic to the instances in its back-end pool. The back-end pool can contain instances in any subnet in the load balancer's VPC. However, if you want to ensure that the load balancer sends incoming traffic to the public subnet ranges in the AZs, you need to select one public subnet from each AZ for the "mapping" category.*

 
9. Scroll down to **Security groups** and select your **Load balancer** security group
Be sure to delete the default security group!

  ![REMOVEDEFAULTSECGR](https://github.com/mindmotivate/multicloudclass/assets/130941970/c40511fc-b1c9-4f5b-ad33-1349ef6a2a00)
10. Under **Listeners & Routing** Select the Target Group you created in the previous section

![Listner](https://github.com/mindmotivate/multicloudclass/assets/130941970/8e95815c-d329-4775-8135-76fde9376f34)

**Important**
> *Your listeners determine which application your using
> As of right now we have only one Target Group to select. However, in the future we will use several.
> At which time we would use the "Add Listener" feature.
> We will leave the remaining defaults for now. (just keep this in mind for the future!*

11. For **Add on services** use default
12. Add descriptive tags as needed

***Review you Summary carefully!***

![LBsummary](https://github.com/mindmotivate/multicloudclass/assets/130941970/8f250e78-61dd-4087-8c22-e945b3dcdbb6)


12. Choose **Create Load Balancer**.
Patiently wait for your load balancer to deploy
![LBcreated](https://github.com/mindmotivate/multicloudclass/assets/130941970/cd422e55-799a-4135-93bb-9bda2260f0d0)

# 6. Create an autoscaling group

1. In the Amazon EC2 console, navigate to **Auto Scaling** > **Auto Scaling Groups**.
2. Choose **Create Auto Scaling Group**.
3. For **Launch Template**, select the launch template or configuration that references the EC2 instance type you created previously.
   
![ASGsetup](https://github.com/mindmotivate/multicloudclass/assets/130941970/fd9f829c-a4e2-4f5b-b5a9-69b55b6b134d)

4. Click Next
5. Under the "Choose instnace launch options dashboard
6. Scroll down to the **VPC** category and select the VPC you created earlier
7. For **Network** under "Availabuility Zones and subnets" select your Private subnets only (one from each region)

![asggrouoplaunchJPG](https://github.com/mindmotivate/multicloudclass/assets/130941970/109e486d-051e-47e8-9c86-b99487c55e8a)

> We want our instances launched in a private subnet
> Select the private subnets from each AZ
8. Select Next
9. **Attach to existing Load Balancer**
10. **Select existing target group**
11. Turn on **Elastic Load Balancing Health checks** 
12. **Health Check Garce Period** ahould be 300 seconds
![healthcheck](https://github.com/mindmotivate/multicloudclass/assets/130941970/5a425885-0334-4424-86d5-044297f63d61)

13. For **Additional Settings**
14. Enable group metrics collection within Cloudwatch
15. **Default Instance Warmup:** 120 seconds
16. Select Next

![scalingpolicy2](https://github.com/mindmotivate/multicloudclass/assets/130941970/3f8bf24f-b014-4280-ae57-a0f8c1ac918b)

17. For **Desired Capacity**, enter the desired number of instances to start with.
18. For **Min Size**, enter the desired minimum number of instances.
19. For **Max Size**, enter the desired maximum number of instances.

![grouppolcy](https://github.com/mindmotivate/multicloudclass/assets/130941970/fa6059ad-f206-4f6f-a509-28fa03c331f4)


20. For **Scaling Policies** Select "Target tracking scaling policy"
21. **Scaling Policies Name** Create a policy name
22. **Metric type** use default
23. Target Value:  75
24. Instance Warmup: 120 seconds
25. **Instance scale-in protection** Enable metric scale in protection
26. Click "Next" on the "Add notifictions" screen
27. Add descriptive tags as needed
28. Carefully look over review page

![review](https://github.com/mindmotivate/multicloudclass/assets/130941970/c9ea6718-d963-4dfd-9b5d-949e7eda12ec)

29. Choose **Create Auto Scaling Group**.

> *Take a moment to navigate to your "instances" page and see if all of your instances have been created!*


### Test the load balancer

>To test the load balancer Navigate to the navigation plane and select "Load Balancer"
Navigate to the Load Balancing details and copy the the DNS name and paste it in your Browser search bar

![lbdns](https://github.com/mindmotivate/multicloudclass/assets/130941970/38ebf568-5f20-4822-b67c-4a70e71187e1)

(Ensure that you have "http:" in front of the URL)

![httpdns](https://github.com/mindmotivate/multicloudclass/assets/130941970/db5c0a8e-61cb-41b2-bcb1-592e7bb81694)

>The Instance Description should appear on the screen
Click he refresh button on your browser and observe the changing instance details
Note:The Availability Zone number should match Third Octect of the Ip address

![instancedetails1](https://github.com/mindmotivate/multicloudclass/assets/130941970/c1f6b6df-63f4-4ae8-b9c3-3029554f1e5a)

![instancedetails2](https://github.com/mindmotivate/multicloudclass/assets/130941970/5f78e4cb-ab54-4ef5-8b9b-6ecf38651d64)

![instancedetails3](https://github.com/mindmotivate/multicloudclass/assets/130941970/7d094bb6-05bf-4088-86d2-51ca007a6465)


# Congratulations! You have created a functioning Load Balancer! Now here comes the fun part....the tear down!

#7. Tear Down Process

**Delete the Auto Scaling group. This will terminate all of the instances in the group.**
Open the Amazon EC2 console.
In the navigation pane, choose Auto Scaling groups.
Select the Auto Scaling group that you want to delete and choose Delete.
In the Confirm deletion dialog box, choose Delete.

**Delete the load balancer.**
Open the Amazon EC2 console.
In the navigation pane, choose Load Balacers.
Select the Load Balancer that you want to delete and choose Delete.
In the Confirm deletion dialog box, choose Delete.

> *Before deleting the Auto Scaling group, you should make sure that all of the instances in the group are in a terminated state.
Before deleting the target group, you should make sure that there are no load balancers using the target group.
Before deleting the load balancer, you should make sure that there are no target groups associated with the load balancer.
Before deleting the security group, you should make sure that there are no instances or network interfaces associated with the security group.
Before deleting the VPC, you should make sure that there are no resources associated with the VPC, such as subnets, internet gateways, and route tables.
Additional steps*

**Delete the Auto Scaling group**
Open the Amazon EC2 console.
In the navigation pane, choose Auto Scaling groups.
Select the Auto Scaling group that you want to delete and choose Delete.
In the Confirm deletion dialog box, choose Delete.
Delete the launch template


**Delete the launch template.**
Open the Amazon EC2 console.
In the navigation pane, choose Launch Templates.
Select the launch template that you want to delete and choose Delete.
In the Confirm deletion dialog box, choose Delete.
Delete the target group


**Delete the target group.**
In the navigation pane, choose Target groups.
Select the target group that you want to delete and choose Delete.
In the Confirm deletion dialog box, choose Delete.
Delete the load balancer


In addition to the above steps, you may also want to consider the following:
Delete any NIC's that were created from the instances in the Auto Scaling group.
Delete any Elastic Ip addresses that may exist








