# An Intro introduction to AWS VPC Creation:  

Motivated Mindstate



## Greeting future AWS Cloud Engineers! <a name="example-templates-autoscaling-full-stack-template"></a>
Greeting future AWS Cloud Engineers!
Before we get into all of the fun and confusing technical jargon surrounding Virtual Private Clouds, allow me provide a few analogies to help set the stage for what we will be learning today!

Think of a Amazon **Virtual Private Cloud** (VPC)  like a virtual data center in the cloud. It’s a private space that you can create in the cloud to store your data and run your applications. It’s actually a bit like having your own private room in a shared house.

A **VPC** is made up of smaller networks called **subnets**. Think of subnets as different rooms within your private space. Each subnet can have its own rules and settings, like who can enter and what they can do. 

A **route table** is like a map that tells your VPC how to send data between different subnets. It’s like having a map of your house that tells you how to get from one room to another.

An **internet gateway** is like a door that connects your VPC to the internet. It allows you to access the internet from within your private space.

**Network Address Translation (NAT)** is like a translator that helps your VPC communicate with the internet. It translates private IP addresses into public IP addresses so that data can be sent and received.

**Security groups** are like bouncers that control who can enter and exit your private space. They make sure that only authorized people can access your data and applications.

**NACL** (Network Access Control List): Think of NACL as a security gatekeeper at the entrance of a neighborhood. It operates at the subnet level and controls traffic by allowing or denying access based on predefined rules. Just like a gatekeeper checks who can enter the neighborhood, NACL checks the source and destination IP addresses to determine if traffic is allowed or denied. It also specifies the return traffic that must be allowed1.

**OK… So everything is crystal clear right?
If not, don’t worry. Follow along as  I explain with more “technical” detail!**  

•	**VPC (Virtual Private Cloud)**: A VPC is a virtual data center in the cloud that allows you to have complete control over your virtual networking environment. It includes features such as selecting your own private IP address range, creating subnets, configuring route tables, and network gateways. VPCs provide benefits like privacy, security, and preventing loss of proprietary data .

•	**Subnets**: Subnets are smaller networks created by dividing a large network. They help with network maintenance and provide security by isolating different parts of the network from each other .

•	**Route Tables**: A route table contains a set of rules called routes that determine where network traffic should be directed. In a VPC, you can have multiple route tables to control traffic flow .

•	**Internet Gateways (IGW)**: An internet gateway is a combination of hardware and software that provides your private networks within a VPC with a route to the outside world. It allows communication between instances within the VPC and the internet. Each VPC can have only one internet gateway attached to it .

•	**Network Address Translation (NAT)**: NAT is used when instances within a subnet have private IP addresses that cannot be used in public. NAT maps the private IP addresses to public addresses on the way out and vice versa on the way in. Elastic IP addresses are static, public IPv4 addresses designed for dynamic cloud computing. They can be associated with any instance or network interface in your VPC, allowing rapid remapping of addresses in case of instance failure .

•	**Security Groups**: Security groups are sets of firewall rules that control traffic for your instances. In Amazon Firewall, the only action that can be carried out is “allow.” You cannot create rules to deny traffic. The destination for traffic is always the instance on which the security group is applied .

•	**Customer Gateway**: A customer gateway is the anchor on your side of the Amazon VPC VPN connection that links your data center or network to your Amazon VPC. It can be a physical or software appliance.

•	**Virtual Private Gateway**: A virtual private gateway is the VPN concentrator on the Amazon side of the VPN connection. You create a virtual private gateway and attach it to the VPC from which you want to create the VPN connection.

•	**VPN**: VPN stands for “virtual private networking,” which is a popular internet security method originally designed for large organizations where employees needed to connect to a certain computer from different locations.

•	**VPC Peering**: A VPC peering connection allows you to route traffic between two VPCs using IPv4 or IPv6 private addresses. Instances in either VPC can communicate with each other as if they are within the same network. You can create a VPC peering connection between your own VPCs or with a VPC in another AWS account. A VPC peering connection helps you facilitate the transfer of data.

•	**Network Access Control Lists (NACL)**: NACL is an optional layer of security for your VPC that acts as a firewall for controlling traffic in and out of one or more subnets. You might set up network ACLs with rules similar to your security groups to add an additional layer of security to your VPC. The default network ACL is configured to allow all traffic to flow in and out of the subnets to which it is associated.

*Let’s create a Virtual Private Cloud!
For additional documentation on VPC’s please refer here:[AWS::VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)

## Step 1: Navigate to your top search bar and type: "VPC"
You should see the following VPC dashboard:
![image](https://github.com/mindmotivate/multicloudclass/assets/130941970/eba6d2f5-3aba-480f-804a-f477cd29804d)


## Step 2: Select your desired region. 
(As stated in the lecture, be sure to do a bit of due diligence here regarding region selection.) We will use N. Virginia in this example.<br>

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-vpc/chooseregion.png" width="25%" height="25%">

## Step 3: We will rename our “Default VPC” as “Do Not Touch” 
Navigate to “Your VPC” on the left side of the page<br>
![image](https://github.com/mindmotivate/multicloudclass/assets/130941970/437939a9-48ba-44a6-b46d-32db0fea753a)

Please note that every region should have a “Default VPC” already established<br>
Do not make any changes to the settings of your "default VPC"<br>

It is good practice to do this for any region that you are working in!<br>
This will serve as a reminder to help prevent any future issues or accidental deletions<br> 

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-vpc/vpcintances.png" width="105%" height="105%">


## Step 4: Click “Create VPC” 
![image](https://github.com/mindmotivate/multicloudclass/assets/130941970/ef1145f2-6a60-4b92-8206-4f1aa86e3577)<br>
Once you arrive at the "Create VPC" screen, go ahead and click: “VPC and more”<br>
This gives you a visual representation while you build your VPC<br>

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-vpc/fullcreatevpcscreen.png" width="100%" height="100%">
Name your VPC (with one word only )
Do not include “white spaces” in your naming conventions
In this example we will name it: “firstvpc”
As a general rule: Don’t use default VPC’s, it’s lazy and unprofessional<br>
![image](https://github.com/mindmotivate/multicloudclass/assets/130941970/8f141d7b-8b8a-4dba-aa53-6ab7ccdacbc1)


## Intro to OCTECT’s ##
A few basic concepts related to IP address format:<br> 

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-vpc/octect_table.JPG" width="75%" height="75%">

## Step 5: Change the “IPv4 CIDR block” ## 
Change the default from 10.0.0.0/16 to another starting IP address using the following conditions…<br>
•	Your region needs an identifying number<br> 
•	Choose a number that is easy to remember<br>
•	This can be your own original number (preferably two digits)<br> 

## This number will replace the second “Octet” in the address ONLY ##<br>
Example: North Virginia 10.*36*.0.0/16<br>
Do not change the Third and Fourth OCTET in the address<br>
Example: North Virginia 10.36.0.0/16<br>
Do not change the LAST number in the address (Host)
Example: North Virginia 10.36.0.0/16


Sample Region Identifiers:<br>
Ireland:32<br>
Tokyo:97<br>
Virginia:76<br>
Paris:94<br>

For the purposes of this tutorial, we will define a specific range for each cloud provider. These are the ranges with which you can obtain a value for your second octet (for this tutorial)<br>

AWS: 10.1-99.0.0/16<br>
Azure: 10.200-255.0.0/16<br>
Google Cloud/Oracle: 10.10-199.0.0/16<br>

*Note: You can have the same network in two different areas however if they are joined, they will NOT WORK. This is what’s referred to as overlap.* 

## Step 6: Choose the Availability Zones ## 
Firstly, Check to see how many AV’s you have in your region!
Do your due diligence here!  The zone selection if of the upmpost importance. Please consider the following:
•	N. Virgina has 3 regions<br>
•	California does not have an “a” ZONE<br>
•	In Sao Paulo AV “b” is not available (even though it is displayed on the site)<br>
•	Some regulations require 4 AZ’s you must clarify with your company before proceeding<br>
•	If you have a domain registration or get a security cert. for your domain, you need to be in N. Virginia region (regardless if your global)<br>
*You need to understand which regions have which services available<br>*

In our tutorial we will choose the following:<br>
•	We will choose the following number of Availability Zones: 3<br>
•	We will choose the following number of Pubic Subnets: 3<br>
•	We will choose the following number of Private Subnets: 3<br>

Your screen should look similar to this:<br>
![image](https://github.com/mindmotivate/multicloudclass/assets/130941970/a03e5d06-503e-46f1-b799-40ca4578576d)

## Step 7: Plan Subnet Routing ## 

Create an entirely separate document in order to plan out our routing
*You must do this to remove confusion!*

Take quick look at the following subnet diagram:
It represents or previously specified requirements
There are 3 availability zones shown
There is a public and private subnet in each zone:<br>
![image](https://github.com/mindmotivate/multicloudclass/assets/130941970/4cdb98a5-716b-4d4c-aa1c-758994ee3153)

## Intial Layout: ##
We will create a list of 6 items based off our AZ / Sub net requirements <br>
Let’s layout our plan first….<br>
We have specified that each zone has a public and private subnet<br>
1.public / zone A<br>
2.public / zone B<br>
3.public / zone C<br>
4.private / zone A<br>
5.private/ zone B<br>
6.private / zone C<br>

## Subnet Plan ##
We will now add the subnet values based of our previously determined
CDIR of: 10.36.0.0/16

## We will list out 6 subnets:<br> ##
*Notice how the first two octets are the same as our CDIR*
Also note how the last number in now 24 (this is due to binary which will be explained in another tutorial)
1. 10.36.0.0/24
2. 10.36.0.0/24
3. 10.36.0.0/24
4. 10.36.0.0/24
5. 10.36.0.0/24
6. 10.36.0.0/24
<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-vpc/first2octects.JPG" width="60%" height="60%">
<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-vpc/hostnumber.JPG" width="65%" height="65%">

## Subnet Rules: ##
You cannot have the name subnet in multiple regions<br>
You can only have one public per av zone<br>
You can however, have multiple privates per av<br>
1 digit in the second octet represents a public subnet<br>
2 digits in the second octet represents a private subnet<br>

*per the previous rules..*
1. 10.36.1.0/24  (ensure that the 2nd octet is a single digit) public / zone A<br>
2. 10.36.2.0/24 (ensure that the 2nd octet is a single digit) public / zone B<br>
3. 10.36.3.0/24 (ensure that the 2nd octet is a single digit) public / zone C<br>
4. 10.36.11.0/24 (ensure that the 2nd octet is two digits) private / zone A<br>
5. 10.36.12.0/24 (ensure that the 2nd octet is two digits) private / zone B<br>
6. 10.36.13.0/24 (ensure that the 2nd octet is two digits) private / zone C<br>

## Here is an example of the completed subnet planning sheet:<br> ##

North Virginia 10.36.0.0/16:<br>
10.36.1.0/24 = subnet public 1A <br>
10.36.2.0/24 = subnet public 1B <br>
10.36.3.0/24 = subnet public 1C <br>

10.36.11.0/24 = subnet private 1A <br>
10.36.12.0/24 = subnet private 1B <br>
10.36.13.0/24 = subnet private 1C <br>

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-vpc/completedplanningsheet.JPG" width="50%" height="50%">

## Step 8: Carefully Enter Subnet Values into AWS ## 

*Note: if you accidentally enter a “white space” you will not see the numerical value here:
Please ensure there are no white spaces or else your network will not work properly!*<br>

*Make the following selections:<br>*
NAT Gateway: (in 1 AZ)<br>
VPC Endpoints: (default)<br>

## Check and RE-CHECK everything before proceeding! ##

## Step 9: Create VPC ##
*Wait several minutes for process to complete*<br>
*As a side note, your NAT will be created during this phase!*<br>

 <img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-vpc/createvpcwaitscreen.png" width="50%" height="50%">








Second Example
In this example we use Tokyo as our region
However. Tokyo does not have a region C
But it will in the future therefore we will plan for a C but noted that it Is not in use!
Tokyo 10.97.0.0/16:
10.97.1.0/24 = subnet public 1A 
10.97.2.0/24 = subnet public 1B 
10.97.3.0/24 = subnet public 1C  not in use
10.97.4.0/24 = subnet public 1D 

10.97.11.0/24 = subnet private 1A 
10.97.12.0/24 = subnet private 1B 
10.97.13.0/24 = subnet private 1C  not in use
10.97.14.0/24 = subnet private 1D

<img src="">
Create Security Groups for each VPC
Configure Network settings

Review your instance to mak sure it in public1 (or any public)
Auto assign pulic 
Enable!!!!

Review you summary detail before launching your instance!


<img src="">
To create an EC2 instance, follow these instructions:
Launch the AWS Management Console.
Navigate to the Amazon EC2 service.
Click on "Launch Instance" to start the instance creation process. In this example we will name our instance "Windowsbox"

<img src="">
Teardown Procedure
There is a specific order that you must follow when tearing down resources in AWS VPC. 
1.	Detach the NAT gateways: If NAT gateways are present in the VPC, delete them. It will take few minutes to be patient.
2.	Release Elastic IPs: You cannot release the Elastic IP’s until the NAT gateways deletion process has completed.
3.	Delete all resources: Before deleting the VPC, ensure that all resources within the VPC are deleted. This includes EC2 instances.
4.	Delete security groups: While you will not be charged for security groups, you can delete all security groups associated with the VPC. (Please do not delete the Default!)
5.	Delete VPC: Finally, delete the VPC itself.
<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-vpc/TeardownJPG.JPG" width="75%" height="75%">
   
