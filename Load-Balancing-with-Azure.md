# Load Balancing with Azure

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-azure-vpc/mmlogo.PNG" width="16%" height="16%">

# Introduction

You may be wondering...*What the heck is a Load Balancer?* Well here is a brief introduction along with some helpful terminology to help us make some sense of the topic!

A ***Load Balancer*** is a tool that manages incoming traffic and distributes it across multiple servers in a cloud environment. It performs this function so that that the system does not get overwhelmed and all requests can be handled in an efficient manner. In other words, the work done by a single computer gets distributed among multiple computers which makes the operation more reliable and available. It serves as a single point of contact for your incoming traffic.

***Front end*** is the part of the load balancer that faces the internet and receives incoming traffic. 

***Backend*** is the part of the load balancer that faces a backend pool instances and distributes incoming traffic to them.

A ***Backend Pool*** is a collection of resources that receive incoming traffic from the load balancer. In Azure, backend pool instances can be Azure Virtual Machines or instances in a Virtual Machine Scale Set. The backend pool is a critical component of the load balancer as it defines the group of resources that will serve traffic for a given load-balancing rule.

A ***Load Balancing rule*** is a configuration that defines how incoming traffic is distributed to the instances within the backend pool. It maps a given frontend IP configuration and port to multiple backend IP addresses and ports. (There are several types of rules that can be invoked)

Azure provides multiple Load Balancing services on its platform. However, for the purposes of this tutorial we will be focusing soley in the standard load balancing option.

***For addtional documentation regarding Azure Load Balancing click the following link:***[AZURE::LB](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/load-balancing-overview)<br>


# Concept Scenarios
Before we begin...If any of the terminlogy seems a bit confusing, it is completely ok! I have provided a few example scenarios to help break down the topic in an easily digestible fashion:
Scenario 1
Scenario 2
Scenario 3

# Process Outline
Ok, now that we understand the general concepts, let's apply them by creating our first load balancer!

**Basic Outline**:<br>
 • Sign in to Azure<br>
 • Sign in to the Azure portal<br>
 • Assign a Subscription<br>
 • Create a Resource Group<br>
 • Create the virtual network<br>
 • Configure Subnet<br>
 • Assign NAT Gateway to your Subnet<br>
 • (the nat gateway will take ingerit thename of the VNet therfore if yoiu wan to specify that it is asspciated with public A you must rename<br> 
 • it approriatey)be sure to select create NEW ip<br>
 • Create A VM Scaleset<br>
 • from within your Scalet crate a public LB<br>
 • establish your LB rules<br>
 • Edit NIC<br>
 • from within nic:<br>
 • Create a Security Group(azure create new option)<br>
 • or use existing NSG from earlier?<br>



# 1. **Log in to the Azure portal** <br>
**Go to the [Azure portal](https://portal.azure.com/) and sign in with your credentials.<br>**

<img src="https://github.com/mindmotivate/multicloudclass/assets/130941970/efca307d-db83-4e9d-b93b-6560981b2b09" width="40%" height="40%">


# 2. **Create a Resource Group**: 
**Once you are inside the Azure portal, navigate to "Resource Group"<br>**
    *Please note that there are several ways to access resource groups in Azure!<br>*
     We can simply select the: **"Resource Group icon"** icon if it is already displayed on your dashboard 
     After the "Create Resource" screen appears, select "Create"
     
***Note:*** In Azure,the majority of your resources will be associated with a subscription. ***Subscriptions*** are a unit of management that allow you to logically organize your resource groups and facilitate billing.
Azure ***Resource Groups** are logical collections of VMs, storage accounts, virtual networks, web apps, databases and database servers.
Within the hierarchy of azure, resource groups sit underneath your subscription*

**Select "Create a Resource Group"**<br>
Fill out the required fields regarding *Project Details*:<br>
<br>
 • **Subscription:** In order to proceed you must ensure that a Subscription is attached to your Resource.<br>
 • **Resource Groups:** Here will name our resource group.<br> 
 <br>
For the purposes of this tutorial,we will name our resource group **"ProdApp1-RG"**<br> 
•Region: Choose (US)West-US-3<br>
•Tags: Let’s add the following tags:<br>
•Review & Create: Let’s review an create!<br>
*Wait for validation screen to appear*<br>
•Now select “create”:<br>
*You should now see your newly created resource group “ProdApp1-RG” appear under “resource groups”*:<br>

Never modify: “DENIES”
Deny all inbound
Deny all outbound
As always don’t touch aoutbound rules as well
It will render your firewall useless
You “lock the door inside your own house”
Your in door should obviously have limitations however your out door should not have limitation


The 6500’s represent the relative weight
The lightest stay at the top
The heaviest stay at the bottom
The smaller numbers go first

Inbound Security Rules


Create another inbound rule for HTTP




# 2. **Create a Network Security Group** 
Create a Security Group
Next, we will create our security group
Navigate to network security group and select “create”
TAGS:
Go ahead and create this familiar set of tags
Validation Screen:
If we select go to resource, we will be in what appears to be a “Firewall” environment complete with all of the bells and whistles
# 2. **Create a Virtual Network** 
Let’s create our VNnet next. Select from portal dashboard or type it into the search bar.
<br>
Select "Create" button<br>
<br>
•Ensure that the proper subscription and resource groups are selected<br>
•In instance details we will name our Vnet: “ProdApp1-Vnet”<br>
•The region will remain: (US) West US 3<br>
•Select “Next”: We will skip the security section at the moment<br>
•Select “Next” again and will proceed to the Ip addresses section<br>
•*Note: Before Proceeding, go ahead and get your Planning Document<br>
•If you do not have one, create one ASAP!<br>

## Configure Subnet
We will add our new CIDR<br>
For this tutorial, we are using: **10.202.0.0/16**<br>
**(Please remember to NEVER use the DEFAULT CIDR that is provided for you, as it lacks professionalism!)** <br>
<br>
![donotusedefaultpng](https://github.com/mindmotivate/multicloudclass/assets/130941970/5cb5e27f-ff9b-4965-84b4-0421df9a4d60)<br>
<br>
**(The new CIDR that we have provided for this tutotrial is: 10.202.0.0/16)** <br>
![ciderrangeJPG](https://github.com/mindmotivate/multicloudclass/assets/130941970/1b419ebb-ef2a-4558-839f-12ad07e6af87)<br>
**For addtional insights regarding CIDR please click the following link:**[AZURE::CIDR Notation](https://devblogs.microsoft.com/premier-developer/understanding-cidr-notation-when-designing-azure-virtual-networks-and-subnets/)<br>
<br>
<br>
**Subnet IP Addresses**: <br>
Here we will establish our subnet ip addresses<br>
As always, we will plan our subnets by utilizing a planning sheet:<br>
<br>
Example Planning Document:<br>
PublicA: 10.202.1.0/24<br>
PublicB: 10.202.2.0/24<br>
PublicC: 10.202.3.0/24<br>
<br>
For more info regarding subnet planning please visit the previous tutorial: [subnet design](https://github.com/mindmotivate/multicloudclass/assets/130941970/faf9162d-4155-4810-b0d8-cc20c5a78574)<br>
*(The thought process and rules for building/naming subnets are similar to AWS)*<br>
<br>
<br>
<br>
*Note: We will only utilizing the **"public"** subnet ranges, therefore you will have three subnets total*<br>
<br>

Create Public A
After we create our first IP range for PublicA, will selct "Create a New NAT Gateway"
We will name our NAT gateway: "PublicA-NAT"
We will select "PublicA-publicipAddress" for the public IP address
(azure creates this name based on your previous naming convention)
Note: We will only need to add the NAT gateway name to one public IP address
Create Public B
Create Public C

•Tags: Let’s add the following tags:<br>
•Review & Create: Let’s review an create!<br>
*Wait for validation screen to appear*<br>
•Now select “create”:<br>
*You should now see your newly created resource group “ProdApp1-VNET” appear under “virtual networks”*:<br>

Congrats!, we have just deployed:
Our VNET
The Subnet NAT Gateway
The public IP address for the Subnet


Navigate back to “resource groups” and open the App1-RG group that you created initially
Take a moment to observe the list of resource group contents 
You will see all of the resources you created including you VNet listed here:
A resource group in Azure is a “Logical Container” It other words it is a way to group your related resources together in a collection of sorts



Create a VM Scaleset
Type “virtual machine sclae set” in the top search bar and select the VM icon
In this tutorial we will be creating a “set “ of VM
A set of “identical” vms called a “scale set”
We will name our VM Scale Set ProdApp1-Vnet
Settings:
Availability Zone: These are physically separate zones, that lie within an Azure region. They represent the physical locations of the AZ data centers. By having your data stored in multiple locations (ie: redundancy) you drastically improve the avaibility of your resources. There are typically three Availability Zones per supported Azure region.
Notice that after you select your specificied number of regions
av1, av2, av3
Image:
For image type, we will select an Unbuntu Machine from the Marketplace. Simply select the words: see all images" to search all marketplace products. Type "Unbuntu in the search bar and locate the image entitled "Unbuntu Minimal 2204 LTS x64 Gen 2
VM Architecture:
Select 64 for tis options
Admin Default Category:
For the following categories you will simply use the default settings:

• Authentication Type: SSH Public Key
• Username: azureuser
• SSH Public Key Source: generate new key pair
• Key Pair Name: VMDemo-1




Select the following Orchestration Settings:
Select the following spot discount and scaling settings:
Select the following Admin settnavigateings
We will name our user “unbuntuuser”
Next: go to spot: no changes here
Next: go to disk
Change os disk size to “standard” ssd”
We don’t need a premium option for the purposes of this tutorial
Next we will  to: “Networking”
For the “Virtual Network Configuration” setting, select the name of our previously created Vnet
ProdApp1-Vnet"

Select the previously named and created NIC by checking the appropriate box
for Load Balancing options choose: Azure Load Balancer

Create a Load Balance fron within your scaleset
We will name our Load : “ProdApp1-LB01"

Make sure it is PUBLIC!

we will establish two inbound rules:
Load Balancer
NAT
Create

Next we will
Edit the NIC

disable public IP
disable express

Health: enable health monitoring and autp repairs
Management: enable

ADVANCED:
spreading: fixed
After naming our LB, we must provide our LB with its own IP address
Enable user data:

Similarly in AWS, when you enter in the subnet values, you should see the number of available addresses displayed on the right side of each entry.
In our example you will notice the number "256" to the left of the subnet addresss. Use this as a method of "self checking" your work. If you accidentally type the wrong number or accidentally add whitespace at the end of the entry, you will see a different numerical value.<br>
<br>
<br>
![256addresses](https://github.com/mindmotivate/multicloudclass/assets/130941970/4b5b49f7-3bb8-43c4-99e8-f9eb5258ba25)<br>
<br>
After the subnetting is complete, click Ok: and proceed to "Networking" section<br>
<br>
<br>
**Networking**<br>
Regarding the "Networking" we make the following selections:<br>
• Virtual Network: VMdemo-vnet<br>
• Subnet: PublicA<br>
• Public IP: VMdemo-1-ip, VMdemo-2-ip, VMdemo-3-ip<br>
• NIC network security group: Basic<br>
• Public inbound ports: Allow Selected Ports<br>
• Select inbound ports: HTTP(80), SS(220<b)r>
• Delete NIC when VM is deleted: Check Box<br>
• Enable accelerated networking: Uncheck BoxCheck<br>
<br>



















https://www.youtube.com/watch?v=wJvmXM81tEI
