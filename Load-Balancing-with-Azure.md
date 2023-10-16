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
Before we begin...If any of the terminlogy seems a bit confusing, it is completely ok! I have provided a few example scenarios to help break down the topic in an easily digestible fashion:<br>
 •Scenario 1<br>
 •Scenario 2<br>
 •Scenario 3<br>

# Process Outline
Ok, now that we understand the general concepts, let's apply them by creating our first load balancer!

**Basic Outline**:<br>
 • Sign in to the Azure portal<br>
 • Create a Resource Group<br>
 • Create a Security Group
 • Create the virtual network<br>
 • Create a Load Balancer
 • Create a VM Scale Set
 
 
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
 • **Resource Groups:** For the purposes of this tutorial,we will name our resource group **"ProdApp1-RG"**<br> 
•**Region:** Choose (US)West-US-3<br>
•**Tags:** Let’s add the following tags:<br>
<br>
• Name: ProdApp1-RG<br>
• Owner: Chewbacca
• Location: Austin<br>
• Planet: Mustafar<br>


•Select: "review an create" once all the required field have been filled<br>
*Wait for validation screen to appear*<br>
•Now select “create”:<br>
*Wait for deployment to complete*
*You should now see your newly created resource group “ProdApp1-RG” appear under “resource groups”*:<br>






# 3. **Create a Network Security Group** 
Next, we will create our security group
Navigate to network security group and select “create”
For the purposes of this tutorial,we will name our resource group **"ProdApp1-NSG"**<br> 

•**Tags:** Let’s add the following tags:<br>
<br>
• Name: ProdApp1-NSG<br>
• Owner: Chewbacca
• Location: Austin<br>
• Planet: Mustafar<br>

•Select: "review an create" once all the required field have been filled<br>
*Wait for validation screen to appear*<br>
•Now select “create”:<br>
*Wait for deployment to complete:*

After the deployment is complete, you may select "go to resource" 
You will see what appears to be a “Firewall” environment complete with all of the bells and whistles!

First...A few ground rules regarding Security Groups:
• Never modify your "Deny Rules” such as "Deny all inbound" or "Deny all outbound"
• Never touch "outbound rules"
If you break any of these rules, you will run the risk of rendering your firewall useless!!
You are basically locking the door inside your own house and throwing awawy the key!

Regarding the numbers in from of your Firewall rules:
Notice the order of the numbers, they represent the relative "weight" of the rules
*The lightest(smaller numbers) stay at the top*
8The heaviest(larger numbers) stay at the bottom*

Let's add three inbound rules:

**Add Inbound Security Rules**
Service: SSH
Priority: 100
Description: Myhomepage

**Add Inbound Security Rules**
Service: HTTP
Priority: 110
Description: Myhomepage

**Add Inbound Security Rules**
Service: RDP
Priority: 120
Description: RDP

Next, we will create VM Scale Set
type "scale ste" into your top search bar and find the "scale set" icon

# 4. **Create a Virtual Network** 
Let’s create our VNnet next. Select from portal dashboard or type it into the search bar.
<br>
Select "Create" button<br>
<br>
•Ensure that the proper subscription and resource groups are selected<br>
•In "instance details" we will name our Vnet: “ProdApp1-Vnet”<br>
•The region will remain: (US) West US 3<br>
•Select “Next”: We will skip the security section at the moment<br>
•Select “Next” again and will proceed to the Ip addresses section<br>

## Configure Subnet
•*Note: Before Proceeding, go ahead and get your Planning Document<br>
•If you do not have one, create one ASAP!<br>

We will begin byb adding add our new CIDR<br>
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
After the new CIDR has been added, delete old one from the subnet list

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
Name: PublicA
Starting Address: 10.202.2.0
*check to make sure you "Ip address space says "256 addresses" (avoid accidentally adding a white space)*

After we create our first IP range for PublicA, will select "Create a New NAT Gateway"
We will name our NAT gateway: "PublicA-NAT"
We will select "(new)PublicA-publicipAddress" for the public IP address (this can be found in the associated drop down list)
(azure creates this name based on your previous naming convention)
Network Security Group: Select "ProdApp1-NSG" (this is the securit group you previously created)
***Note: We will only need to add the NAT gateway name to one public IP address***
***The Subnet NAT Gateway bears the public IP address for the entire Subnet***
Press the "Add" button when complete

Create Public B
Name: PublicB
Starting Address: 10.202.2.0
*check to make sure you "Ip address space says "256 addresses" (avoid accidentally adding a white space)*
Press the "Add" button when complete

Create Public C
Name: PublicC
Starting Address: 10.202.3.0
*check to make sure you "Ip address space says "256 addresses" (avoid accidentally adding a white space)*
Press the "Add" button when complete

•Tags: Let’s add the following tags:<br>
• Name: ProdApp1-Vnet<br>
• Owner: Chewbacca
• Location: Austin<br>
• Planet: Mustafar<br>
*tags are optional, however it is good practice to add them*

•Review & Create: Let’s review an create!<br>
*Wait for validation screen to appear*<br>
•Now select “create”:<br>
Patiently wait for your deployment to complete...lol
Congrats! you have just sucessfully deployed your Virtual Network

Next, on to the Load Balancer....


# 5. **Create a Load Balancer** 
Next, we will create our Load Balancer
Select from portal dashboard or type "Load Balancer" into the search bar.
<br>
Ensure that the proper subscription and resource groups are selected<br>
Select “create”
For the purposes of this tutorial,we will name our resource group **"ProdApp1-LB"**<br> 
<br>
Make the following Basic selections:
SKU: Standard
*Very Important: make sure "type" is set to public!
Type: Public
Tier: Regional

Next, Make the following Frontend IP Configuration selections:
For the purposes of this tutorial,we will name our resource group **"ProdApp1-frontendip"**<br> 
Select "create new" under the "Public IP Address" category

We will add a public ip address
For the purposes of this tutorial,we will name it:"ProdApp1-frontendip-public-address"<br> 
select "OK"

Next, Make the following "Backend Pool" selections:
Press the "Add backend pool" button
Name: For the purposes of this tutorial, we will name it:"ProdApp1-backendpool"<br> 
Virtual Network: Associate it with you Vnet: (the name of your virtual netowrk should auto populate this field)
Click "Save" when complete

Similar to our security group, the load balancer will also have its own set of inbound rules...
**Add Load Balancing Rules**
Frontend IP address: select the "ProdApp1-frontendip" from the drop down list
Backend Pool: select the "ProdApp1-backendpool" from the drop down list

We will create a new health probe, so select "create new"
Health Probe: name it: ProdApp1-healthprobe
Click "Save" to save you new health probe name
Check over all you selections
Click "Save" to complete your add load baancing rule selections


The load balancer will also have a NAT rules...
**Add NAT Rule**
Name: We will name it "ProdApp1-NAT"
Target Backend Pool: select the "ProdApp1-backendpool" from the drop down list
Frontend IP Address: select the "ProdApp1-frontendip" from the drop down list
Frontend port rage start: 50,0000
Maximum number of machines: 3,0000
Backend Port: 22 
Leave all other options on their default settings
Press the "Add" button when complete

Outbound Rules: skip past these by pressing "next"

•**Tags:** Let’s add the following tags:<br>
<br>
• Name: ProdApp1-LB<br>
• Owner: Chewbacca
• Location: Austin<br>
• Planet: Mustafar<br>
*tags are optional, however it is good practice to add them*
•Review & Create: Let’s review an create!<br>
*Wait for validation screen to appear*<br>
•Now select “create”:<br>
Patiently wait for your deployment to complete...lol
Congrats! you have just sucessfully created your Load Balancer





# 6. **Create a VM Scale Set** 
Type “virtual machine scale set” in the top search bar and select the scale set icon
We will name our VM Scale Set: "ProdApp1-VMscale"

Select "Create" button<br>
•Subscription: Ensure that the proper subscription and resource groups are selected<br>
•Region: The region will remain: (US) West US 3<br>
Availability Zone: Select Zone1, Zone2 & Zone3
*These are physically separate zones, that lie within an Azure region. They represent the physical locations of the AZ data centers. By having your data stored in multiple locations (ie: redundancy) you drastically improve the avaibility of your resources. There are typically three Availability Zones per supported Azure region.*

Security Type: Standard

Image: Unbuntu Minimal 2204 LTS x64 Gen 2
For image type, select "see all images" We will then select an Unbuntu Machine from the Marketplace. Simply select the words: see all images" to search all marketplace products. Type "Unbuntu in the search bar and locate the image entitled "Unbuntu Minimal 2204 LTS x64 Gen 2

VM Architecture: Select 64 

Scaling: 
Instance Count: 4
Click "Scaling Configuration": directly underneath "instance count:
Next, Make the following scaling selections:
Apply fore delete to scale0in operations: check box

Maintain remaining default settings and click "Save"

Select the following "Admin Account" settings:
Username: We will chage the name of our user from “unbuntuuser” to "prodapp1user"

Next: skip the spot section (no changes will need to be made here)

Next: go to disk
OS Disk Type: Change os disk size to “standard” ssd”
We don’t need a premium option for the purposes of this tutorial

Next: go to Networking
**Networking**<br>
Regarding the "Networking" we make the following selections:<br>
• Virtual Network: select previouslyy create Vnet name
• Network Interface Category:
Select the pencil icon on the far right
This will allow us to configure the NIC

Edit Network Interface:
• NIC network security group: a name will auto-populate
• NIC Security Group: Select "Advanced"
Very Important!:
Public Ip Address: Disabled
Accelerated Networking: Disabled
select "OK"

• Select Load Balancer: select previously created resource
• Select Backend Pool: select previously created resource

Next: go to Health
**Health**<br>
Regarding the "Section" we make the following selection:<br>
• Enable Applicaiton Health Monitoring: Check Box

Next: go to Advanced
**Advanced**<br>
We will proceed to the "Advanced" section next: *Click Advanced next* <br>
<img src="https://github.com/mindmotivate/multicloudclass/assets/130941970/2c079a21-b0a0-43ae-ac5e-f1613817bf40" width="25%" height="25%"><br>
<br>
<br>
**Enable Userdata**<br>
Enable User Data: Check Box
<br>
We will naviagte to the "add user data" box:<br>
![userdataJPG](https://github.com/mindmotivate/multicloudclass/assets/130941970/24fc8b69-1c1f-4343-b6f5-95ab21530358)<br>
<br>
<br>
Please copy and paste the following script in the box:<br>
**Copy Launch Script Below:**\.

```
#!/bin/bash

Remo Script


```

•Tags: Let’s add the following tags:<br>
• Name: ProdApp1-VMscale<br>
• Owner: Chewbacca
• Location: Austin<br>
• Planet: Mustafar<br>
*tags are optional, however it is good practice to add them*


•Review & Create: Let’s review an create!<br>
*Wait for validation screen to appear*<br>
•Now select “create”:<br>

You will be prompted by a "Generate new key pair" message
Select: Download the private key

Once your deployment has completed you will go to your resource and locate the Public IP address

 **Find Your IP Adress**<br>
**Locate the IP address for your instance:**<br>

<br>
Type http:// in the search bar first before pasting the public IP address of your new instance<br>


Instance details should be displayed on the screen:<br>


**Teardown Procedure:**<br>
-Type "resource groups" in the top search bar<br>
-Select the vpc you want to delete.<br>
-Click on the "Delete resource group" button.<br>
-In the confirmation dialog box, paste your copied resource group name<br>
--Click on Delete to complete the deletion process.<br>



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
