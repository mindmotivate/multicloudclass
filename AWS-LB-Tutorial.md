# An Intro introduction to AWS Load Balancing and Autoscaling in the Cloud:  

[Motivated Mindstate](https://github.com/mindmotivate)


## Greetings future AWS Cloud Engineers! <a name="example-templates-autoscaling-full-stack-template"></a>
Greetings future AWS Cloud Engineers!
Welcome to the most recent instalment of our AWS Tutorials!

The following will be a continuation of the tutorial where we created our first VPC in AWS. 
In this tutorial we will be creating a load balancer which will attach to our previous VPC.
If you like to visit the previous tutorial please use the following link:

Ok, nwo that wever gotten the formalities out of the wway.. 
Here is more infomration than you ever we wanted to know about Autoscaling, Load Balancing, NAT's, NIC's, ASG's and other Three-letter acronyms...lol 

# So what does "Autoscaling" mean?

Autoscaling is the process of automatically adjusting the number of computing resources that are allocated to an application or service based on demand. This can help to ensure that your application is always available and performant, even when traffic spikes.

Autoscaling is a critical part of cloud computing, as it allows you to scale your infrastructure up or down without having to manually provision or deprovision resources. This can save you time and money, and it can also help to improve the reliability and performance of your applications. Autoscaling is an essential part of cloud computing!

# What are Load Balancers for exactly?.....

LB's can be used with autoscaling to ensure that your applications are always available to perform at their best! For example, you can configure an ALB to distribute traffic across a set of EC2 instances that are managed by an autoscaling group. The autoscaling group will automatically add or remove EC2 instances from the group based on the traffic load. This ensures that your application always has enough resources to handle the current load, without having to manually provision or deprovision resources.

Application Load Balancers (ALBs) are a type of load balancer that is designed for HTTP and HTTPS traffic. ALBs can be used to distribute traffic across multiple EC2 instances, ECS containers, or AWS Lambda functions.

The load balancer will receive web requests and send them to the backend pool via their public IP addresses, on port 80 by default. This is because the load balancer is located in a public subnet and needs to be able to communicate with the backend servers, which are located in a private subnet.

The backend servers in a private subnet will have a public IP address assigned to them by the NAT gateway. This IP address is used for communication with the load balancer. However, the backend servers will not be directly accessible to the general web. Instead, all traffic to the backend servers must be routed through the load balancer.

Here is a diagram of the network architecture: Internet -> Load Balancer -> NAT Gateway -> Backend Servers

Internet -> Load Balancer -> NAT Gateway -> Backend Servers
The load balancer will distribute traffic to the backend servers using a load balancing algorithm. The most common algorithm is round robin, which distributes traffic evenly across all of the backend servers.

When a client sends a request to the load balancer, the load balancer will translate the client's public IP address to the public IP address of one of the backend servers. The load balancer will then forward the request to the selected backend server.

The backend server will process the request and generate a response. The backend server will then send the response back to the load balancer. The load balancer will then forward the response back to the client.

This process ensures that all traffic to the backend servers is routed through the load balancer. This helps to improve the security and reliability of the backend servers.

It is also important to note that the load balancer can be configured to use HTTPS instead of HTTP. This is a more secure way to communicate with the backend servers. However, it will require the backend servers to be configured with SSL certificates.

Now in reverse:

a server in a private subnet that wants to communicate with the web must go through the NAT gateway first, and then to the load balancer. This is because the server does not have a public IP address and cannot communicate directly with the web.

The NAT gateway will translate the server's private IP address to a public IP address. The server will then use this public IP address to communicate with the web.

The load balancer is not used for outgoing traffic from the server to the web. This is because the server can communicate directly with the web using the public IP address that is assigned to it by the NAT gateway.
The load balancer only needs to interact with the servers when they are fulfilling a request from the web. Otherwise, the servers can send information directly to the internet via the NAT gateway.

Here is a diagram of the network architecture:

Server (Private Subnet) -> NAT Gateway -> Internet
The server will initiate the communication with the web by sending a request to the NAT gateway. The NAT gateway will translate the server's private IP address to a public IP address and then forward the request to the web.

One more thing...
It is important to note that some organizations may choose to block all outbound traffic from servers in private subnets, except for traffic that is routed through the load balancer. This is done to improve security and control.


# Ok so what is this NIC i hear so much about?>
NIC is the Network Interface Card, Here's how it comes into play....
A client sends a request to the load balancer's public IP address.
The load balancer forwards the request to one of the servers in the target group/backend pool.
The server receives the request on its NIC.
The server processes the request and generates a response.
The server sends the response back to the load balancer on its NIC.
The load balancer forwards the response back to the client.
The server's NIC is essential for this process because it allows the server to communicate with the load balancer. Without the NIC, the server would not be able to send or receive traffic to or from the load balancer.


# Auto Scale Scenario:

You have a sucesful web application that is running on only a single EC2 instance.
Due to increased traffic you need a solution that will help you handle all of the incoming request
You dont want to buy any sort of permanent host solution because you may not need it all the time
Addtionaly, there is generally a hhefty upfront cost associated
You create an ALB and configure it to distribute traffic across the EC2 instance.
You create an autoscaling group and configure it to manage the EC2 instance.
You configure the autoscaling group to scale up or down the number of EC2 instances based on the traffic load.

Now, when traffic to your web application spikes, the autoscaling group will automatically add more EC2 instances to the group. The ALB will then distribute traffic across the new EC2 instances. This ensures that your web application is always available and performant, even when traffic spikes.
You save time, money (and stress) in the process!

Now that we understnad the overall concept, lets demonstrate this in AWS with our previosly created VPC
 
## Note: We are beggining this tutorial with an established VPC! ##


### Create a security group for the VPC

1. In the Amazon EC2 console, navigate to **Security Groups**.
2. Choose **Create Security Group**.
3. Enter a name and description for the security group.
4. For **VPC**, select the VPC you *previoulsy* created.<br>
> *On VPC click on the x and replace the VPC that is labeled “TouchItAndDie” replace it*<br>
> *with the VPC that you created<br>*<br>![vpcnsg](https://github.com/mindmotivate/multicloudclass/assets/130941970/da252db6-8eb0-47e0-994a-30a4af26cfdd)

6. Add inbound and outbound rules to allow the traffic that you need for your application.<br>
> *We will be creating Three: SSH, HTTP, RDP<br>*
7. Scroll down to Inbound rules and click on Add rule
8. Click on the dropdown under Type and select SSH
> *Repeat the same steps after clicking Add rule but select HTTP, then afterwards select RDP<br>
9. For each rule under the Source tabs click on the drop down and select Anywhere-IPv4
10. Give each of your rule a description under the Description tabs<br>![vpcsecurity](https://github.com/mindmotivate/multicloudclass/assets/130941970/fa7955be-d86d-4121-8efb-ad7445c4c10b)

***Next: PLEASE SKIP THE OUTBOUND RULES***<br>

11. Add descriptive Tags 
12. Choose **Create**.



### Create a security group for the Load Balancer

1. In the Amazon EC2 console, navigate to **Security Groups**.
2. Choose **Create Security Group**.
3. Enter a name and description for the security group.
5. For **VPC**, select the VPC you created.
> *On VPC click on the x and replace the VPC that is labeled “TouchItAndDie” replace it
with the VPC that you created*
6. For **Inbound Rules**, add a rule to allow traffic on port 80 from all sources.
7. Click on the dropdown under Type and select HTTP
8. For "Source", click on the drop down and select Anywhere-IPv4
9. Provide a description under the Description tab<br>![LBsecuritygroupdetails](https://github.com/mindmotivate/multicloudclass/assets/130941970/c9426542-6049-46fb-95c2-d7408cdf4954)

***Next: PLEASE SKIP THE OUTBOUND RULES***
10. Add descriptive tags as needed
11. Lastly, Choose **Create** to Create security group
> *Click on Security group on the top of the screen to verify both security groups*


### Create an EC2 instance

1. Navigate to the Amazon EC2 console,
2. Choose **Launch Template** from left side menu
3. Click on Create launch templates
- Under Launch template name type in the name of your template
- Simply copy and paste the same name into the "descrption" field
- Ensure that there is some meaningful naming convention to maintain organization
Under "Application and OS Images" choose "Quick Start"
Select the **Amazon Linux 2023 AMI** Image
 Choose "t2.micro" for your instance type
- Click on **Create new keypair** type in and label your keypair. Make sure you have RSA /
.pem format then click Create key pair
***Important Step***
  - Under **Network settings** make sure the **Subnet** drop dpown menu has the option ***Don’t include in launch
template***
- Under the **Firewall (security groups)** Select ***existing security group*** click on the drop
down and choose the VPC security group that you will attach to your server (ex: ASG01-SG01-Server)
- Click the dropdown on **Advanced network configuration**
- Here we will set up our Network Interface
Navigate to Auto-assign public IP click on the dropdown and choose ***Disable***
> You do not want your instances to be accessible from the public internet
Under the **Security Group** category - Make sure the VPC security group you attach to your server is selected
- Scroll all the way down and click on **Advanced details** then scroll all the way down to
**User data** and click on **Choose file**
  Copy and paste the following script in the **User Data" field
(insert Jay Remo Script)
- Add Tags as needed
- Click on **Create launch template**


### Create an autoscaling target group

1. In the Amazon EC2 console, navigate to the EC2 Dashboard and select **Target Groups**.
2. Choose **Create Target Group**.
4. For **Target Group Name**, enter a name for the target group.
7. For **Protocol**, select **HTTP**.
8. For **Port**, enter 80.
9. For **IP Address Type** use default IPv4
6. For **Availability Zones**, select all of the availability zones in your region.
For **VPC** click on the dropdown on the VPC you created VPC-App02
8. For **Protocal Version**, use default **HTTP1**.
9. For **Health Check Protocal**, use default **HTTP**.
10. For **Health Check Path**, use default `/`.
11. Add descriptive tags as needed
12. Choose **Create Target Group**.
12. Choose **Next**.
12. Choose **Create Target Group**.

### Create an AWS Application Load Balancer

1. In the Amazon EC2 console, navigate to **Load Balancers**.
2. Choose **Create Load Balancer**.
4. Choose **Application Load Balancer**.
***COMPLETELY IGNORE THE CLASSIC LOAD BALANCER*** (It is being decomissioned)
6. For **Scheme**, use default internet-facing
7. Scroll down to the **VPC** category and select the VPC you created earlier
**Important**
Under **Mappings** Click and checkbox each Zone and select all on public subnets
7. For **Availability Zones**, select all of the availability zones in your region.
Choose a "Public Subnet" from each AZ
Your Load balancer will be communicating with the public subnet ranges
 When you create a load balancer, you need to specify a subnet for the load balancer's front-end. This subnet is where the load balancer will receive incoming traffic. The load balancer will then distribute this traffic to the instances in its back-end pool.
The back-end pool can contain instances in any subnet in the load balancer's VPC. However, if you want to ensure that the load balancer sends incoming traffic to the public subnet ranges in the AZs, you need to select one public subnet from each AZ for the "mapping" category.
 
  Scroll down to **Security groups** and select your **Load balancer** security group
 **V J
 8. For **Listener**, choose **Create New Listener** 
9. For **Protocol**, select **HTTP**.
10. For **Port**, enter 80.
11. For **Forward Target Group**, select the target group you created in step 4.
12. Choose **Create Listener**.
13. Choose **Create Load Balancer**.
- On the Load Balancing Category click on the Load Balancer tab
- Click on Create load balancer
- Under the Load balancer types click Create under the Application Load Balancer
- Type in the name under the Load balancer name
- Scroll down to the VPC category and select the VPC you created earlier
- Under Mappings Click and checkbox each Zone and select all on public subnets
- Scroll down to Security groups and select your Load balancer security group
- Under Listeners and routing under default action select your existing Target group
- Scroll down to Load balancer tags click Add new tag and label your description
- Verify all your configuration and if everything is ok click Create load balancer
- Click on View load balancer
- Now it is time to set up your Auto Scaling group




### Create a load balancer listener

1. In the Amazon EC2 console, navigate to **Load Balancing** > **Listeners**.
2. Choose **Create Listener**.
3. For **Protocol**, select **HTTP**.
4. For **Port**, enter 80.
5. For **Default Action**, choose **Forward to Target Group**.
6. For **Target Group**, select the target group you created in the prerequisites.
7. Choose **Create Listener**.








- On the left dashboard go the the Load Balancing Category and click on Target Groups
- Click on the orange tab on the right side of the screen and click on Create target group
- Scroll down to Target group name and type in the name of your Target group
- Scroll down to your VPC and click on the dropdown on the VPC you created VPC-App02
- Scroll down to your Tags and label your tag to describe your Target Groups
- Click Next
- Scroll down and click on Create target group
- Time to create your load balancer next






4. Choose an AMI and instance type appropriate for your application.
5. For **VPC**, select the VPC you previously created.
6. For **Subnet**, select the public subnet for the availability zone in which you want to launch the instance.
7. 
8. For **Security Groups**, select the security group you created for the VPC in step 1.
9. Ensure that you do not select your Load Balancer security group!
10. Under **Advanced Details**, click **Edit** next to **Network Settings**.
11. 
12. For **Subnet**, select the public subnet for the availability zone in which you want to launch the instance.
13. For **Auto Assign Public IP Address**, select **Enable**.
    Keep in mind, the EC2 instances that you are creating will be located in a private subnet
    They will need to be assigned a Public IP to communicate with the Load Balancer and the NAT gateway
14. Choose **Launch Instances**.


- Click on the EC2 tab or type on the spacebar EC2 and click on EC2 on the dropdown
- On your left under the Instance category click on the Launch Templates tab
- Click on Create launch templates
- Under Launch template name type in the name of your template (copy + paste)
- Under Template version description type in the name of your template
- Under Quick Start make sure it is under Amazon Linux ami if not click on it
- Click on the AMI dropdown and make sure you are on t2.micro free tier eligible
- Click on Create new keypair type in and label your keypair. Make sure you have RSA /
.pem format then click Create key pair
- Under Network settings make sure Subnet drop has the option Don’t include in launch
template
- Under the Firewall (security groups) Select existing security group click on the drop
down and choose the VPC security group you will attach to your server ex:
TKY-SG-Server
- Click the dropdown on Advanced network configuration, click on Add network
interface, go to Auto-assign public IP click on the dropdown and choose Disable
- Make sure the VPC security group you attach to your server is selected
- Scroll all the way down and click on Advanced details then scroll all the way down to
User data and click on Choose file with the script you want to upload.
- Click on Create launch template
- Click on View launch templates
- Click on the AWS tab on the top left hand side
- Click on the EC2 tab
- Now it is time to set up your Target Groups








### Create a launch template

1. In the Amazon EC2 console, navigate to **Launch Templates**.
2. Choose **Create Launch Template**.
3. Enter a name and description for the launch template.
4. Under **Launch Template Options**, select the EC2 instance you created in step 2.
5. Choose **Create Launch Template**.

Note: *You may delete the orignal EC2 instance if you'd like as it is no longer needed at this point

After all of our  servers have been lainched, we need to place the in the same "basket" called a Targtet Group
(backend pool in Azure)

### Create an autoscaling target group

1. In the Amazon EC2 console, navigate to **Auto Scaling** > **Target Groups**.
2. Choose **Create Target Group**.
3. Choose **Application Load Balancer**.
4. For **Target Group Name**, enter a name for the target group.
5. For **Availability Zones**, select all of the availability zones in your region.
6. For **Protocol**, select **HTTP**.
7. For **Port**, enter 80.
8. For **Health Check**, select **HTTP**.
9. For **Health Check Path**, enter `/`.
10. Choose **Create Target Group**.



### Create an autoscaling group

1. In the Amazon EC2 console, navigate to **Auto Scaling** > **Auto Scaling Groups**.
2. Choose **Create Auto Scaling Group**.
3. For **Launch Template**, select the launch template or configuration that references the EC2 instance type you created.
4. For **Min Size**, enter the desired minimum number of instances.
5. For **Max Size**, enter the desired maximum number of instances.
6. For **Desired Capacity**, enter the desired number of instances to start with.
7. Under **Load Balancing**, choose **Attach to Existing Load Balancer**.
8. For **Load Balancer**, select the load balancer you created.
9. For **Target Group**, select the target group you created in step 1.
10. Choose **Create Auto Scaling Group**.


### Create an AWS Application Load Balancer

1. In the Amazon EC2 console, navigate to **Load Balancing** > **Load Balancers**.
2. Choose **Create Load Balancer**.
3. Choose **Application Load Balancer**.
4. For **Load Balancer Name**, enter a name for the load balancer.
5. For **Availability Zones**, select all of the availability zones in your region.
6. For **Listener**, choose **Create New Listener**.
7. For **Protocol**, select **HTTP**.
8. For **Port**, enter 80.
9. For **Forward Target Group**, select the target group you created in step 4.
10. Choose **Create Listener**.
11. Choose **Create Load Balancer**.


### Create a load balancer listener

1. In the Amazon EC2 console, navigate to **Load Balancing** > **Listeners**.
2. Choose **Create Listener**.
3. For **Protocol**, select **HTTP**.
4. For **Port**, enter 80.
5. For **Default Action**, choose **Forward to Target Group**.
6. For **Target Group**, select the target group you created in the prerequisites.
7. Choose **Create Listener**.


### Configure the load balancer listener to forward traffic to the autoscaling target group

1. In the Amazon EC2 console, navigate to **Load Balancing** > **Listeners**.
2. Select the load balancer listener you created in step 1.
3. Choose **Edit**.
4. For **Default Action**, choose **Forward to Target Group**.
5. For **Target Group**, select the target group you created in the prerequisites.
6. Choose **Save**.



### Create a load balancer rule

1. In the Amazon EC2 console, navigate to **Load Balancing** > **Listeners**.
2. Select the load balancer listener you created in step 1.
3. Choose **Rules**.
4. Choose **Create Rule**.
5. For **Priority**, enter a priority for the rule.
6. For **Conditions**, choose the criteria that you want to use to match traffic to the rule (e.g., URL path, HTTP method, etc.).
7. For **Actions**, choose **Forward to Target Group**.
8. For **Target Group**, select the target group you created in the prerequisites.
9. Choose **Create Rule**.



### Configure the load balancer rule to match traffic based on your desired criteria

1. In the Amazon EC2 console, navigate to **Load Balancing** > **Listeners**.
2. Select the load balancer listener you created in step 1.
3. Choose **Rules**.
4. Select the load balancer rule you created in step 3.
5. Choose **Edit**.
6. For **Conditions**, choose the criteria that you want to use to match traffic to the rule (e.g., URL path, HTTP method, etc.).
7. Choose **Save**.


### Forward the load balancer rule to the load balancer listener

1. In the Amazon EC2 console, navigate to **Load Balancing** > **Listeners**.
2. Select the load balancer listener you created in step 1.
3. Choose **Rules**.
4. Select the load balancer rule you created in step 3.
5. Choose **Edit**.
6. For **Actions**, choose **Forward to Target Group**.
7. For **Target Group**, select the target group you created in the prerequisites.
8. Choose **Save**.


### Test the load balancer

To test the load balancer, you can use a tool like curl or Postman to send HTTP requests to the load balancer DNS name. You should see that the requests are forwarded to the autoscaling target group and that the responses are returned successfully.
