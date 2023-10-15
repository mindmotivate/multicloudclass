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
Sign in to the Azure portal.
<img src="https://github.com/mindmotivate/multicloudclass/assets/130941970/efca307d-db83-4e9d-b93b-6560981b2b09" width="40%" height="40%">


# 2. **Create a Resource Group**: 
**Once you are inside the Azure portal, navigate to "Resource Group"<br>**
    *Please note that there are several ways to access resource groups in Azure!<br>*
     We can simply select the: **"Resource Group icon"** icon if it is already displayed on your dashboard 
     After the "Create Resource" screen appears, select "Create"
     
***Note:*** In Azure,the majority of your resources will be associated with a subscription. ***Subscriptions*** are a unit of management that allow you to logically organize your resource groups and facilitate billing.
Azure ***Resource Groups** are logical collections of VMs, storage accounts, virtual networks, web apps, databases and database servers.
Within the hierarchy of azure, resource groups sit underneath your subscription*

***Select "Create a Resource Group"***<br>
Fill out the required fields regarding *Project Details*:<br>
<br>
 • **Subscription:** In order to proceed you must ensure that a Subscription is attached to your Resource.<br>
 • **Resource Groups:** Here will name our resource group.<br> 
 <br>
For the purposes of this tutorial, we will name our resource group **"ProdApp1-RG"** <br>







https://www.youtube.com/watch?v=wJvmXM81tEI
