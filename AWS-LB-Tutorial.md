# An Intro introduction to AWS Load Balancers:  

[Motivated Mindstate](https://github.com/mindmotivate)



## Greetings future AWS Cloud Engineers! <a name="example-templates-autoscaling-full-stack-template"></a>
Greetings future AWS Cloud Engineers!
Welcome to the second instalment of our AWS Tutorials!
The following will be a continuation of the tutorial where we created our first VPC in AWS. 
In this tutorial we will be creating a load balancer which will attach to our previous VPC.
If you like to visit the previous tutorial please use the following link:





## Step 9: Create VPC ##
*Wait several minutes for process to complete*<br>
*As a side note, your NAT will be created during this phase!*<br>


## Step 11:Create an EC2 instance ## 
*For instructions on how to create an EC2 instances refer to the following documentation:<br>*
[EC2 Tutorial](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)

In our previous tutorial we created a  VPC and  our security group. Now we will create a secuity groups specificually for our LB and we will also crate "Mulitple" EC2 instances.
(We previously just added on)
We will need at minimum one instance in each availaility zone in our network.
## Create an LB Securitu Group: ##

## Create a set of EC2 instances: ##


1. From Amazon EC2 console, navigate to Instances and choose Launch Instance and assign the appropriate naming convention for your first AZ<br>

Example: if you are launching your instance in Availailty Zone A, You should select a image name that bears A in the tile
such as: EC2A



2. Select an Amazon Machine Image (default) and choose Next.<br>
3. Choose an instance type and configure any additional settings as needed.<br>
4. Select the VPC we previously created with the instance and choose a subnet within that VPC.<br>
5. Configure"Network Settings" next
6. Click "edit" button
7. Select the previously created VPC name

8. In the "Subnet" category we will chose the name of one of our "Private" Ip addresses

IMPORTANT!: choose the correct private subnet for your AZ
check your planning sheet if necessary
Example: if you are launching your instance in Availailty Zone A, You should select subnet 10.18.11.0/24
Be consistent across all you subnets!

For "Auto Assign Public IP Address" we will select enable
1.  Select existing security group next and choose the name of your previosuly existing security group
2.  Be careful , do not choose the SG for the Load Balancer
3.  
4.  Next, we will add our user data script (shout out ot Jay Remo!)
5.  
6.  Place the script in the user data field box,
7.  (enter script).<br>
8.  
9.  Adjust the numberof intances you would like to launch in your desired AZ
10. Go to "Summary" and asjust the number of instances you would like to launch

11. Review your settings and launch your instance.<br>

We will repeat the previous process for each of our remaining Availability Zones B and C

After all of our  servers have been lainched, we need to place the in the same "basket" called a Targtet Group
(backend pool in Azure)

Target Group Settings
Tags

Choose a Load Balancer
Application Load Balancer
Basic Configuration
Public Mapping:
we will enter our PUBLIC subnet ranges so that the pub;ic traffic an reach our LB

Choose the LB secutity grou
Listeners - a lb needs to have a listener to ?
Tags:

Create LB
