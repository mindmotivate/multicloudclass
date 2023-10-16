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

<br>

># Concept Scenarios
Before we begin...If any of the terminlogy seems a bit confusing, it is completely ok! I have provided a few example scenarios to help break down the topic in an easily digestible fashion:<br>

 •Scenario 1<br>
 •Scenario 2<br>
 •Scenario 3<br>

# Process Outline
Ok, now that we understand the general concepts, let's apply them by creating our first load balancer!

**Basic Outline**:<br>
 • Sign in to the Azure portal<br>
 • Create a Resource Group<br>
 • Create a Security Group<br>
 • Create the virtual network<br>
 • Create a Load Balancer<br>
 • Create a VM Scale Set<br>
 
 
# 1. **Log in to the Azure portal** <br>
**Go to the [Azure portal](https://portal.azure.com/) and sign in with your credentials.<br>**

<img src="https://github.com/mindmotivate/multicloudclass/assets/130941970/efca307d-db83-4e9d-b93b-6560981b2b09" width="40%" height="40%">


# 2. **Create a Resource Group**: 
**Once you are inside the Azure portal, navigate to "Resource Group"<br>**
    *Please note that there are several ways to access resource groups in Azure!<br>*
     We can simply select the: **"Resource Group icon"** icon if it is already displayed on your dashboard 
     After the "Create Resource" screen appears, select "Create"
     ![resourcegroup](https://github.com/mindmotivate/multicloudclass/assets/130941970/4f68bc09-ebce-4cf4-be71-fea4f58f2425)

***Note:*** In Azure,the majority of your resources will be associated with a subscription. ***Subscriptions*** are a unit of management that allow you to logically organize your resource groups and facilitate billing.

![heirchy](https://github.com/mindmotivate/multicloudclass/assets/130941970/b787abef-5b32-4c57-a24d-7b34daf9009c)

Azure ***Resource Groups** are logical collections of VMs, storage accounts, virtual networks, web apps, databases and database servers.
Within the hierarchy of azure, resource groups sit underneath your subscription*

![createresourcelb](https://github.com/mindmotivate/multicloudclass/assets/130941970/d26da1a8-204b-4413-857b-477f2c4b323c)

**Select "Create a Resource Group"**<br>
Fill out the required fields regarding *Project Details*:<br>
<br>
 • **Subscription:** In order to proceed you must ensure that a Subscription is attached to your Resource.<br>
 • **Resource Groups:** For the purposes of this tutorial,we will name our resource group **"ProdApp1-RG"**<br> 

![prodapp1rgname2](https://github.com/mindmotivate/multicloudclass/assets/130941970/c5055f07-3bb1-46eb-9f6c-7ba2543ed929)


•**Region:** Choose (US)West-US-3<br>
•**Tags:** Let’s add the following tags:<br>
- Name: ProdApp1-RG<br>
- Owner: Chewbacca<br>
- Location: Austin<br>
- Planet: Mustafar<br>

![rgtags](https://github.com/mindmotivate/multicloudclass/assets/130941970/4f9a13b0-df0c-42ab-b4af-b1990fc1554e)

Select: "review an create" once all the required field have been filled<br>
*Wait for validation screen to appear*<br>
Now select “create”:<br>
*Wait for deployment to complete*<br>
You should now see your newly created resource group “ProdApp1-RG” appear under “resource groups”:<br>


![newresourcegroup](https://github.com/mindmotivate/multicloudclass/assets/130941970/791f9716-bcc7-4dbb-964c-75cb8c2dd8a8)




# 3. **Create a Network Security Group** 
Next, we will create our security group<br>
Navigate to network security group and select “create”<br>
For the purposes of this tutorial,we will name our resource group **"ProdApp1-NSG"**<br> 
<br>

![creaetnsh](https://github.com/mindmotivate/multicloudclass/assets/130941970/0deaabe4-35e4-490c-a1fd-abeac78d16c7)

•**Tags:** Let’s add the following tags:<br>
<br>
• Name: ProdApp1-NSG<br>
• Owner: Chewbacca<br>
• Location: Austin<br>
• Planet: Mustafar<br>

•Select: "review an create" once all the required field have been filled<br>
*Wait for validation screen to appear*<br>
•Now select “create”:<br>
![create](https://github.com/mindmotivate/multicloudclass/assets/130941970/726d58da-f6c8-4ce0-b2a4-d64a189497b1)

*Wait for deployment to complete:*<br>
<br>
After the deployment is complete, you may select "go to resource" <br>
You will see what appears to be a “Firewall” environment complete with all of the bells and whistles!<br>
<br>
First...A few ground rules regarding Security Groups.....<br>
>• Never modify your "Deny Rules” such as "Deny all inbound" or "Deny all outbound"<br>
>• Never touch "outbound rules"<br>
>If you break any of these rules, you will run the risk of rendering your firewall useless!<br>
>You are basically locking the door inside your own house and throwing awawy the key!<br>

Regarding the numbers in from of your Firewall rules:<br>
>Notice the order of the numbers, they represent the relative "weight" of the rules<br>
>*The lightest(smaller numbers) stay at the top*<br>
>8The heaviest(larger numbers) stay at the bottom*<br>
<br>
Let's add three inbound rules:<br>
<br>

>**Add Inbound Security Rules**<br>
>Service: SSH<br>
>Priority: 100<br>
>Description: Myhomepage<br>
><br>
![addaninboundsecrule](https://github.com/mindmotivate/multicloudclass/assets/130941970/e82bf86e-9f44-4db8-92c0-418a6013b7d3)

![sshpriorityJPG](https://github.com/mindmotivate/multicloudclass/assets/130941970/2469954c-54e3-4824-a292-70c363510b88)


**Add Inbound Security Rules**<br>
>Service: HTTP<br>
>Priority: 110<br>
>Description: Myhomepage<br>
><br>



![addunboundhhtp](https://github.com/mindmotivate/multicloudclass/assets/130941970/a0501b98-7ba4-47d7-b4b4-11c696d9d7dc)

![allowhttpriority](https://github.com/mindmotivate/multicloudclass/assets/130941970/634537a8-22a5-4904-86b6-29ba4f21cb1b)

**Add Inbound Security Rules**<br>
>Service: RDP<br>
>Priority: 120<br>
>Description: RDP<br>
><br>
![addinboundrulerdp](https://github.com/mindmotivate/multicloudclass/assets/130941970/d49a6cbe-c04b-4e7e-a441-092ff36a27b8)

![PriorityRDP](https://github.com/mindmotivate/multicloudclass/assets/130941970/cd7224ca-18fd-47e2-a099-2f52c09acc9e)

•Select: "review an create" once all the required field have been filled<br>
*Wait for validation screen to appear*<br>

•Now select “create”:<br>
*Wait for deployment to complete:*<br>

![NSGvalidation](https://github.com/mindmotivate/multicloudclass/assets/130941970/910301ce-0291-4908-b451-60eee22b2311)


Next, we will create VM Scale Set<br>
Type "scale ste" into your top search bar and find the "scale set" icon<RB>
<BR>
# 4. **Create a Virtual Network**
Let’s create our VNnet next. Select from portal dashboard or type it into the search bar.<br>
<br>
![vnsearch](https://github.com/mindmotivate/multicloudclass/assets/130941970/918eb192-7ebb-4e64-a24a-06d9992694cb)

Select "Create" button<br>
![createvn](https://github.com/mindmotivate/multicloudclass/assets/130941970/9ec41294-4553-4cd4-9fb2-f56d98d7f4ab)
<br>
•Ensure that the proper subscription and resource groups are selected<br>
•In "instance details" we will name our Vnet: “ProdApp1-Vnet”<br>

![vnetselect](https://github.com/mindmotivate/multicloudclass/assets/130941970/9667dd62-0688-415d-ad43-19fe12ae3b4d)

•The region will remain: (US) West US 3<br>

![vnetvame](https://github.com/mindmotivate/multicloudclass/assets/130941970/e5e16fb0-7c4a-49c8-ab32-7b70cf60f6d8)

•Select “Next”: We will skip the security section at the moment<br>
•Select “Next” again and will proceed to the Ip addresses section<br>
<br>
## Configure Subnet<br>
•*Note: Before Proceeding, go ahead and get your Planning Document<br>
•If you do not have one, create one ASAP!<br>
<br>
>We will begin byb adding add our new CIDR<br>
For this tutorial, we are using: **10.202.0.0/16**<br>
**(Please remember to NEVER use the DEFAULT CIDR that is provided for you, as it lacks professionalism!)** <br>
<br>
**(The new CIDR that we have provided for this tutotrial is: 10.202.0.0/16)** <br>

![16png](https://github.com/mindmotivate/multicloudclass/assets/130941970/6c09672b-b047-4b89-8bf5-0498b04f104f)

**For addtional insights regarding CIDR please click the following link:**[AZURE::CIDR Notation](https://devblogs.microsoft.com/premier-developer/understanding-cidr-notation-when-designing-azure-virtual-networks-and-subnets/)<br>
<br>
<br>
After the new CIDR has been added, delete old one from the subnet list<br>

![deletedefault](https://github.com/mindmotivate/multicloudclass/assets/130941970/e82a155e-d9d7-43a1-9bc4-154510266f9a)

Next, select "Add subnet":<br>
![addsubnetscreen](https://github.com/mindmotivate/multicloudclass/assets/130941970/e4ed08e4-8daa-4d50-a68c-1af18eff9370)

**Subnet IP Addresses**: <br>
Here we will establish our subnet ip addresses<br>
As always, we will plan our subnets by utilizing a planning sheet:<br>
<br>
>Example Planning Document:<br>
>PublicA: 10.202.1.0/24<br>
>PublicB: 10.202.2.0/24<br>
>PublicC: 10.202.3.0/24<br>
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
![24](https://github.com/mindmotivate/multicloudclass/assets/130941970/2b59913e-a3fb-4447-8824-3d081e3d21f9)


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
Patiently wait for your deployment to complete...<br>
![deploymentinprogress](https://github.com/mindmotivate/multicloudclass/assets/130941970/545a6e3d-5418-4709-bbd7-86fe7da9c57a)<br>
<br>
Congrats! you have just sucessfully deployed your Virtual Network<br>
<br>
Next, on to the Load Balancer....<br>
<br>

# 5. **Create a Load Balancer** 
Next, we will create our Load Balancer
Select from portal dashboard or type "Load Balancer" into the search bar.
<br>
Select the "Load Balancer" icon:

![LBicon](https://github.com/mindmotivate/multicloudclass/assets/130941970/7b963d73-d6ae-4504-9494-38099d65e5c7)
![lbcreate](https://github.com/mindmotivate/multicloudclass/assets/130941970/f1748da8-38a5-49ee-906c-1cb9b3b12203)
![loadbalancermkt](https://github.com/mindmotivate/multicloudclass/assets/130941970/7bbc20b7-4113-4d97-9471-0e28907afa2f)

Ensure that the proper subscription and resource groups are selected<br>
Select “create”
For the purposes of this tutorial, we will name our resource group **"ProdApp1-LB"**<br> 
<br>
![LBrequirments](https://github.com/mindmotivate/multicloudclass/assets/130941970/f652ecd5-32a3-40f0-a50c-742ef56f43a1)
<br>
Make the following Basic selections:
SKU: Standard
*Very Important: make sure "type" is set to public!
Type: Public
Tier: Regional



Next, naviagte to "Frontendip configuration"
![feipconfig](https://github.com/mindmotivate/multicloudclass/assets/130941970/0ae3eaa3-181e-4cc8-9fad-173c66abdda1)

Make the following Frontend IP Configuration selections:
For the purposes of this tutorial,we will name our resource group **"ProdApp1-frontendip"**<br> 
Select "create new" under the "Public IP Address" category
![addfeip](https://github.com/mindmotivate/multicloudclass/assets/130941970/48195bf4-32ca-4d7f-9157-7cebbda565c5)

We will add a public ip address
For the purposes of this tutorial,we will name it:"ProdApp1-frontendip-public-address"<br> 
select "OK"

Next, Make the following "Backend Pool" selections:
Press the "Add backend pool" button
Name: For the purposes of this tutorial, we will name it:"ProdApp1-backendpool"<br> 

![addbackendpool](https://github.com/mindmotivate/multicloudclass/assets/130941970/bf60f9e3-ff6a-44d1-b5b7-a01aaf04b84a)
![backendpoolname](https://github.com/mindmotivate/multicloudclass/assets/130941970/dce7cd1c-7c24-4ea4-9ff6-87b3b3899cc6)

Virtual Network: Associate it with you Vnet: (the name of your virtual netowrk should auto populate this field)
Click "Save" when complete

Similar to our security group, the load balancer will also have its own set of inbound rules...
**Add Load Balancing Rules**
Frontend IP address: select the "ProdApp1-frontendip" from the drop down list
Backend Pool: select the "ProdApp1-backendpool" from the drop down list
![addaloadbalancingrule](https://github.com/mindmotivate/multicloudclass/assets/130941970/181383cf-43db-4a7a-87b0-aa5e8a6d711f)

We will create a new health probe, so select "create new"

![createnewhp](https://github.com/mindmotivate/multicloudclass/assets/130941970/106e471e-9099-44f5-b361-04125c9b7c62)

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

![lbvalidaton](https://github.com/mindmotivate/multicloudclass/assets/130941970/894321fe-723c-4e4d-9b19-f90924a851a4)

•Now select “create”:<br>
Patiently wait for your deployment to complete...lol
Congrats! you have just sucessfully created your Load Balancer


![lbcreated](https://github.com/mindmotivate/multicloudclass/assets/130941970/f45f1d2b-0672-490d-aaef-b9743c9c89d3)



# 6. **Create a VM Scale Set** 
Type “virtual machine scale set” in the top search bar and select the scale set icon
![vmscaleset](https://github.com/mindmotivate/multicloudclass/assets/130941970/e20bb20a-6776-40d6-9a2a-a9df3c40f6f4)

We will name our VM Scale Set: "ProdApp1-VMscale"
![crvmscaleset](https://github.com/mindmotivate/multicloudclass/assets/130941970/57b49a40-826b-4288-9da3-ea0b5252bc27)

Select "Create" button<br>
•Subscription: Ensure that the proper subscription and resource groups are selected<br>
•Region: The region will remain: (US) West US 3<br>
Availability Zone: Select Zone1, Zone2 & Zone3
*These are physically separate zones, that lie within an Azure region. They represent the physical locations of the AZ data centers. By having your data stored in multiple locations (ie: redundancy) you drastically improve the avaibility of your resources. There are typically three Availability Zones per supported Azure region.*

Security Type: Standard

Image: Unbuntu Minimal 2204 LTS x64 Gen 2
For image type, select "see all images" We will then select an Unbuntu Machine from the Marketplace. Simply select the words: see all images" to search all marketplace products. Type "Unbuntu in the search bar and locate the image entitled "Unbuntu Minimal 2204 LTS x64 Gen 2

![vmscalesetimage](https://github.com/mindmotivate/multicloudclass/assets/130941970/230a57de-d8c3-4d2a-8c41-aa7440ff9b72)



VM Architecture: Select 64 

Scaling: 
Instance Count: 4
Click "Scaling Configuration": directly underneath "instance count:

![configscalingoptions](https://github.com/mindmotivate/multicloudclass/assets/130941970/5e01cf54-d779-4198-8833-25253befdb8d)

Next, Make the following scaling selections:
Apply fore delete to scale-in operations: check box

![scaleconfig](https://github.com/mindmotivate/multicloudclass/assets/130941970/73d9edcc-0c21-4217-9349-f2d237da10cd)


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
![healthscaleset](https://github.com/mindmotivate/multicloudclass/assets/130941970/7e9daa37-54b8-4dc6-a619-e1ac6d6698e0)

Next: go to Advanced
**Advanced**<br>
We will proceed to the "Advanced" section next: *Click Advanced next* <br>
<img src="https://github.com/mindmotivate/multicloudclass/assets/130941970/2c079a21-b0a0-43ae-ac5e-f1613817bf40" width="25%" height="25%"><br>
<br>
<br>
**Enable Userdata**<br>
Enable User Data: Check Box<br>

<br>![enableuserdata](https://github.com/mindmotivate/multicloudclass/assets/130941970/7930eab2-6a49-4358-a809-a6632e8f03a1)

We will naviagte to the "add user data" box:<br>
![userdataJPG](https://github.com/mindmotivate/multicloudclass/assets/130941970/24fc8b69-1c1f-4343-b6f5-95ab21530358)<br>
<br>
<br>
Please copy and paste the following script in the box:<br>

![enableuserdata1](https://github.com/mindmotivate/multicloudclass/assets/130941970/911ca360-4250-471b-a59e-6545c453df9d)

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
![vmdeployment](https://github.com/mindmotivate/multicloudclass/assets/130941970/9480016d-0f00-456e-8519-ec3c4e22bf72)

 **Find Your IP Adress**<br>
**Locate the IP address for your instance:**<br>

<br>
Type http:// in the search bar first before pasting the public IP address of your new instance<br>


Instance details should be displayed on the screen:<br>
![instancedescription](https://github.com/mindmotivate/multicloudclass/assets/130941970/176d6e39-68cd-4ccc-94bf-5d5d6a4f0f03)


**Teardown Procedure:**<br>
>-Type "resource groups" in the top search bar<br>
>-Select the vpc you want to delete.<br>
>-Click on the "Delete resource group" button.<br>
>-In the confirmation dialog box, paste your copied resource group name<br>
>--Click on Delete to complete the deletion process.<br>






















https://www.youtube.com/watch?v=wJvmXM81tEI
