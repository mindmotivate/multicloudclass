# An Intro introduction to AWS Load Balancing and Autoscaling in the Cloud:  

[Motivated Mindstate](https://github.com/mindmotivate)


## Greetings future AWS Cloud Engineers! <a name="example-templates-autoscaling-full-stack-template"></a>
Greetings future AWS Cloud Engineers!
Welcome to the most recent instalment of our AWS Tutorials!

The following will be a continuation of the tutorial where we created our first VPC in AWS. 
In this tutorial we will be creating a load balancer which will attach to our previous VPC.
If you like to visit the previous tutorial please use the following link:

# So what does "Autoscaling" mean?

Autoscaling is the process of automatically adjusting the number of computing resources that are allocated to an application or service based on demand. This can help to ensure that your application is always available and performant, even when traffic spikes.

Autoscaling is a critical part of cloud computing, as it allows you to scale your infrastructure up or down without having to manually provision or deprovision resources. This can save you time and money, and it can also help to improve the reliability and performance of your applications.

Autoscaling is an essential part of cloud computing, as it allows you to scale your infrastructure up or down based on demand without having to manually provision or deprovision resources. This can have a number of benefits, including: Cost Reduction , Improved Reliability and Increased Performance

# What are Load Balancers for exactly?.....

LBs can be used with autoscaling to ensure that your applications are always available to perofrm at their best! For example, you can configure an ALB to distribute traffic across a set of EC2 instances that are managed by an autoscaling group. The autoscaling group will automatically add or remove EC2 instances from the group based on the traffic load. This ensures that your application always has enough resources to handle the current load, without having to manually provision or deprovision resources.

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
4. For **VPC**, select the VPC you created.
5. Add inbound and outbound rules to allow the traffic that you need for your application.
6. Choose **Create**.

### Create a security group for the LB

1. In the Amazon EC2 console, navigate to **Security Groups**.
2. Choose **Create Security Group**.
3. Enter a name and description for the security group.
4. For **VPC**, select the VPC you created.
5. For **Inbound Rules**, add a rule to allow traffic on port 80 from all sources.
6. For **Outbound Rules**, add a rule to allow all traffic to all destinations.
7. Choose **Create**.

### Create an EC2 instance

1. In the Amazon EC2 console, navigate to **Instances**.
2. Choose **Launch Instance**.
3. Choose an AMI and instance type appropriate for your application.
4. For **VPC**, select the VPC you created.
5. For **Subnet**, select the public subnet for the availability zone in which you want to launch the instance.
6. For **Security Groups**, select the security group you created for the VPC in step 1.
7. Under **Advanced Details**, click **Edit** next to **Network Settings**.
8. For **Subnet**, select the public subnet for the availability zone in which you want to launch the instance.
9. For **Auto Assign Public IP Address**, select **Enable**.
10. Choose **Launch Instances**.

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
