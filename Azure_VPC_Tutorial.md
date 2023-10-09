
# Creating an Azure Virtual Private Network using your Azure Account

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-azure-vpc/mmlogo.PNG" width="15%" height="15%">

In this tutorial we will walk through the various steps requiredfor creating and launching a Virtual Private Network on the Microsoft Azure Cloud. We will begin with the creation of a "Virtual Machine" (in the world of AWS, a virtual machine would be referred to as an EC2) and then we will proceed to create our first Virtual Private Cloud.<br>

*For addtional documentation regarding Azure Virtual Machines click the following link:[AZURE::VM](https://learn.microsoft.com/en-us/azure/virtual-machines/overview)<br>*

*For addtional documentation regarding Azure Virtual Private Clouds click the following link:[AZURE::VPC](https://www.c-sharpcorner.com/blogs/azure-vpc2#:~:text=Azure%20Virtual%20Private%20Cloud%20(VPC)%20provides%20a%20way%20to%20isolate,create%20subnets%2C%20and%20configure%20routing.).)<br>*

Ok, Let's Begin!<br>

# 1. **Log in to the Azure portal** <br>
**Go to the [Azure portal](https://portal.azure.com/) and sign in with your credentials.<br>**

<img src="https://github.com/mindmotivate/multicloudclass/assets/130941970/efca307d-db83-4e9d-b93b-6560981b2b09" width="40%" height="40%">

# 2. **Create a Virtual Machine**: 
**Once you are inside the Azure portal, you can then proceed to create a "Virtual Machine"<br>**
    *Please note that there are several ways to access and create various resources in Azure!<br>*
    We can simply select the: **"Create a Resource"** icon and naviagte to the **"Virtual Machine"** icon:<br>
    <br>
    <br>
    **Alternate Method 1:** we can manually type in the words "virtual machine" via the portal search bar:<br>
![vmportal](https://github.com/mindmotivate/multicloudclass/assets/130941970/fc539366-77d8-4002-9dbe-0840198cdc0d)
    <br>
    **Alternate Method 2:** If you have previously used the virtual machine feature in the past, the icon may already be displayed on your dashboard!:<br> 
![vmportal3](https://github.com/mindmotivate/multicloudclass/assets/130941970/2ab965b4-8222-4f0b-91f1-c8ae5e5ee363)
    <br>
    **Alternate Method 3:**
    Upon selecting the "three lines" at the top left corner of your screen, you will review a list of resources and services which includes virtual machines:<br>
  ![threelinesicon](https://github.com/mindmotivate/multicloudclass/assets/130941970/ee5b6268-c90d-452c-8570-9a789c595dca)
<br>  
<img src="https://github.com/mindmotivate/multicloudclass/assets/130941970/bf49c6c9-3632-4343-b5b6-fb6f1ff2959a" width="50%" height="50%"><br>

**Alternate Method 4:**
   Although we will not cover this method in this tutotial, you have the ability to create a VM using the "CLI"(command line interface)<br>
    <br>
    For more infrmation regarding the creation of a VM using Azure CLI refer to the following documentation:[AZURE::CLI](https://learn.microsoft.com/en-us/azure/virtual-network/quick-create-cli)<br>
    <br>
    ***As you can see, there are a variety of menu options at your disposal!<br>***

# 3. **Create a resource group**
Fill out the required fields reagrding project details:<br>

**Project Details**<br>
    <br>
    **Subscription:** In order to proceed you must ensure that a Subscription is attached to your VM.<br>
    **Resource Groups:** Here will name our resource group. Select "Create New" (in blue letters)<br>
    <br>
    *(For the purposes of this tutorial we will name our resource group "VMdemo")*<br>
    <img src="https://github.com/mindmotivate/multicloudclass/assets/130941970/a9846886-74b3-44f6-8372-eea35191eb9a" width="75%" height="75%">
    <br>
    **Region:** Azure provides one or more geographical regions in which you may build your network. Regions allow you to meet specific data and compliance requirements.<br> 
   <img src="https://github.com/mindmotivate/multicloudclass/assets/130941970/9e961514-6bcd-4ded-9389-74ccf5835853" width="75%" height="75%">

  **Availability Options:** Multiple Availability Zones provide an increased level of control & stability over your data.<br> 
<img src="https://github.com/mindmotivate/multicloudclass/assets/130941970/2e744396-168f-4693-a3f3-1c1dcf5951bb" width="75%" height="75%">
<br>
  **Availability Zone:** These are physically separate zones, that lie within an Azure region. They repsrecent the phyiscal locations of the AZ data centers.  By having your data stored in multiple locations (ie:redundancy) you n drastically improve avaibility. There are typically three Availability Zones per supported Azure region.<br>
<br>
<img src="https://github.com/mindmotivate/multicloudclass/assets/130941970/56832148-bd83-4ef0-81a5-f8a9efad06d6" width="75%" height="75%">
<br>
  ***Notice that after you select your specificied number of regions, You will see the coresponding number of VM names displayed in the "Virtual Machine Name" box: VMdemo-1, VMdemo-2, VMdemo-3***<br>
<br>
<img src="https://github.com/mindmotivate/multicloudclass/assets/130941970/56832148-bd83-4ef0-81a5-f8a9efad06d6" width="75%" height="75%">
<br>
<br>
    **Security Type:<br>**
    Choose "Standard" for your security type<br>
    ![standardsecurity](https://github.com/mindmotivate/multicloudclass/assets/130941970/857ad278-2e26-4952-8500-00ec3045bdb9)
    <br> 
    **Image:<br>**
    For image type, we will select an Unbuntu Machine from the Marketplace. Simply select the words: see all images" to search all marketplace products.
    Type "Unbuntu in the search bar and locate the image entitled ***"Unbuntu Minimal 2204 LTS x64 Gen 2"<br>***
    ![seeallimages](https://github.com/mindmotivate/multicloudclass/assets/130941970/b06a13c7-9221-425c-befb-c06c9a8d458d)<br>
    <br>
    <img src="https://github.com/mindmotivate/multicloudclass/assets/130941970/95cb8692-a104-439c-bb13-d662feffd027" width="55%" height="55%"><br>
    <br>
    <img src="https://github.com/mindmotivate/multicloudclass/assets/130941970/35da8f80-187f-40a6-83a5-8a1428df8ca3" width="35%" height="35%"><br>
    <br>
    **VM Architecture:<br>**
    Select 64 for tis options<br>
    ![VMarchitecture](https://github.com/mindmotivate/multicloudclass/assets/130941970/cac296dd-6c77-422d-b191-35dc601d53e9)<br>
    <br>
    <br>
    <br>
    **Run with acure spot discount:**<br> 
    Leave the checkbox unchecked <br>
    <br>![spotdiscount](https://github.com/mindmotivate/multicloudclass/assets/130941970/54ce64c8-73be-4fd9-9fb4-ad6cc5503146)
    <br>
    <br>
    **Size:** We will use the default option for this category <br>
    ![sizevm](https://github.com/mindmotivate/multicloudclass/assets/130941970/0f2deea2-b153-4861-a624-7a2d56722dbb)<br>



    
**Admin Default Category**: <br>
For the following categories you will simply use the default settings:<br>
       <br>
       • **Authentication Type:** SSH Public Key<br>
       • **Username:** azureuser <br>
       • **SSH Public Key Source:** generate new key pair <br>
       • **Key Pair Name:** VMDemo-1<br>

Your screen should look similar to this:<br>
![Adminaccountdefaults](https://github.com/mindmotivate/multicloudclass/assets/130941970/a5ca4907-e9d9-4ea2-81a7-5358333a7ac3)





**Inbound Port Rules**: <br>
• For the "Inbound Port Rules" we will be selecting<br>
• Public inbound ports: choose allow selected port<br>
• Select inbound ports: select both HTTP(80) and SSH(22) options<br>
<br>
Your dashboard should look similar to this:<br>
<br>
![inboundrules](https://github.com/mindmotivate/multicloudclass/assets/130941970/7ddef758-e251-4836-926e-0dc38a643523)
<br>
<br>
**Disk:**
<br>
Please skip past the "Disk" category as we will not make any changes at this time<br>

# 4. **Create a Virtual Network**<br>
   Next we will fill out the required field regarding out Virtual Network. We will name our subnet and then establish our CIDR and our subnets.<br>
   <br>
    **Select "Create New**<br>
   We will name our Vnet using the same name we previously created for our virtual network with the addition of "vnet"<br>
   Therefore our Vnet name will be:"VMdemo-vnet"<br>
    <img src ="https://github.com/mindmotivate/multicloudclass/assets/130941970/e01e56c1-d8b3-4b4b-9422-1457dadcc796">
   **Address Space**: <br>
Here, we will add our new CIDR<br>
For this tutorial, we are using: **10.202.0.0/16**<br>
**(Please remember to NEVER use the DEFAULT CIDR that is provided for you)** <br>

![donotusedefaultpng](https://github.com/mindmotivate/multicloudclass/assets/130941970/5cb5e27f-ff9b-4965-84b4-0421df9a4d60)<br>
<br>
![ciderrangeJPG](https://github.com/mindmotivate/multicloudclass/assets/130941970/1b419ebb-ef2a-4558-839f-12ad07e6af87)<br>

   **Subnets**: <br>
Here we will establish our subnets<br>
As always, we will plan our subnets by utilizing a planning sheet:<br>
<br>
Example Planning Document:<br>
PublicA: 10.202.1.0/24<br>
PublicA: 10.202.2.0/24<br>
PublicA: 10.202.3.0/24<br>
<br>
For more info reagrding subnet planning please visit the previous tutorial:<br>
*(The thought process and rules for building/naming subnets are similar to AWS)*<br>
<br>
<br>
![subnetnames](https://github.com/mindmotivate/multicloudclass/assets/130941970/faf9162d-4155-4810-b0d8-cc20c5a78574)<br>
<br>
*Note: We will only utilizing the **"public"** subnet ranges, therefore you will have three subnets total*<br>
<br>
Similar to AWS when you enter in the subnet values, you should see the number of available addresses displayed on the right side of each entry
In our example you will notice the number "256" to the left of the subnet addresss. Use this as a method to "self check" your work. If you accidentally type the wrong number or accidentally add whitespace at the ened of the entry, you will see a different numerical value.<br>
<br>
<br>
![256addresses](https://github.com/mindmotivate/multicloudclass/assets/130941970/4b5b49f7-3bb8-43c4-99e8-f9eb5258ba25)<br>
<br>
After the subnetting is complete, click Ok: and proceed to Networking section<br>
<br>
**Networking**<br>
Regarding the "Networking" we make the following selections:
• Virtual Network: VMdemo-vnet<br>
• Subnet: PublicA<br>
• Public IP: VMdemo-1-ip, VMdemo-2-ip, VMdemo-3-ip<br>
• NIC network security group: Basic<br>
• Public inbound ports: Alloe Selected Ports<br>
• Select inbound ports: HTTP(80), SS(220<b)r>
• Delete NIC when VM is deleted: Check Box<br>
• Enable accelerated networking: Uncheck BoxCheck<br>
<br>
**Load Balancing**<br>
• Load Balancing options: None<br>
<br>
Ater the previous selections have been made, your screen should look similar to this:
<br>
Next, we will proceed to the Management section<br>
<br>
4. **Management**<br>
No changes will be made here. Simply ensure that your subscription is under the "Basic Plan" 
Otherwise continue to the next section which is: "Monitoring"<br>
<br>
<br>
Click Monitoring next<br>
![monitoringnext](https://github.com/mindmotivate/multicloudclass/assets/130941970/770ff8a8-1b08-4f21-bf60-298b75e9d601)
<br>
4. **Monitoring**<br>
On the Monitoring page, make the following selections:<br>
<br>
We will enable recommended alert rules by clicking the checkbox:<br>
<br>![enablealerts](https://github.com/mindmotivate/multicloudclass/assets/130941970/8110afaf-56bd-4edf-b3da-208d4ff66fd8)
<br>
After clicking you will see another menu appear. We wiill simply click "Save"
<br>
![alertsave](https://github.com/mindmotivate/multicloudclass/assets/130941970/761011fa-6a0b-4bee-bb0b-3cfbebba66cc)
<br>
![alertsavetwo](https://github.com/mindmotivate/multicloudclass/assets/130941970/b77564c2-eb43-4cc7-acdd-e3c3f54c757d)
<br>
We will process to the "Advanced" section next<br>
<br>![advancednext](https://github.com/mindmotivate/multicloudclass/assets/130941970/e200cd5a-4e6c-43d3-a1eb-49fc6bd3eb0c)
<br>
Click Advanced next<br>
![advancednext](https://github.com/mindmotivate/multicloudclass/assets/130941970/2c079a21-b0a0-43ae-ac5e-f1613817bf40)
<br>
<br>
**Enter Userdata**<br>
<br>
We will naviagte to the "add user data" box:<br>
![userdataJPG](https://github.com/mindmotivate/multicloudclass/assets/130941970/24fc8b69-1c1f-4343-b6f5-95ab21530358)<br>
<br>
<br>
Please copy and paste the following script in the box:<br>
**Copy Launch Script Below:**\.

```
#!/bin/bash

# Update system and install Apache2 and jq
apt-get update -y
apt-get install -y apache2 jq

# Ensure Apache2 is running and enabled on boot
systemctl start apache2
systemctl enable apache2

# Fetch Azure VM metadata
METADATA=$(curl -H Metadata:true -s "http://169.254.169.254/metadata/instance?api-version=2021-01-01")

# Log metadata for debugging purposes
echo "$METADATA" > /tmp/metadata.json

# Extract data from the fetched metadata
local_ipv4=$(echo "$METADATA" | jq -r '.network.interface[0].ipv4.ipAddress[0].privateIpAddress')
az=$(echo "$METADATA" | jq -r '.compute.location')
vm_id=$(echo "$METADATA" | jq -r '.compute.vmId')

# Generate an HTML file with the extracted data
cat <<EOF > /var/www/html/index.html
<!doctype html>
<html lang="en" class="h-100">
<head>
<title>Details for Azure VM</title>
</head>
<body>
<div>
<h1>Azure Instance Details</h1>
<h1>Samurai Katana</h1>

<p><b>Instance Name:</b> $(hostname -f)</p>
<p><b>Instance Private IP Address:</b> ${local_ipv4}</p>
<p><b>Availability Zone:</b> ${az}</p>
<p><b>Virtual Machine ID:</b> ${vm_id}</p>
</div>
</body>
</html>
EOF

# Remove the temporary file
rm /tmp/metadata.json

```
<br>
<br>
<br>
Entire script shold be pasted into the box as shown here:<br>
<img src="https://github.com/mindmotivate/multicloudclass/assets/130941970/fa539fe0-819a-4c04-9856-a4c5650651aa" width="50%" height="50%">
<br>


4.**Tags**<br>
On the "Tags" page, create the following tags:<br>
<br>
• Service: Homepage<br>
• Company: Microsoft<br>
• Location: Austin<br>
<br>

![createtags](https://github.com/mindmotivate/multicloudclass/assets/130941970/e6104c56-002a-4267-b571-bc11920f3320)
<br>
After applying Tags we will click "Review + Create"<br>
<br>
![reviewcreate](https://github.com/mindmotivate/multicloudclass/assets/130941970/08169e98-9189-4cfe-9870-b85ec5eaee86)
<br>



# 5. **Launch Your Virtual Machine**

After we create our VM:<br>
![createvm](https://github.com/mindmotivate/multicloudclass/assets/130941970/2ac9b913-f19a-4241-bff4-e1e2ccef81e0)<br>

We will see a pop up menu asking if we want to download private keys. Select yes and download them to your computer"<br>
![downloadkey](https://github.com/mindmotivate/multicloudclass/assets/130941970/c1ce3f70-a4a6-4ae0-a478-cfd238c04688)

Initial Deployment in Progress:<br>

Deployment in Progress:<br>
![deploymentinprogressJPG](https://github.com/mindmotivate/multicloudclass/assets/130941970/5551c734-9ec6-4715-80d4-cf7172d29d29)<br>
![deploymentinprogress](https://github.com/mindmotivate/multicloudclass/assets/130941970/16055edf-bfb1-4502-95eb-e2b026fc8c7b)<br>

Final Deployment in Progress:<br>![deploymentinprogress](https://github.com/mindmotivate/multicloudclass/assets/130941970/d6bf3721-7794-4965-93fd-93b861260e00)<br>

![deploymentcreate3](https://github.com/mindmotivate/multicloudclass/assets/130941970/c8e0093a-23fa-4e90-9cd2-ab89499a2fe1)<br>

Wait a few moments for your instance to be created:<br>

Validation Complete Screen:<br>
<br>
![resourcedashboardJPG](https://github.com/mindmotivate/multicloudclass/assets/130941970/48f574d5-0521-4bd8-9310-b13e137d3336)<br>
<br>
![ipaddress](https://github.com/mindmotivate/multicloudclass/assets/130941970/23d96bd5-4fe5-4004-a08d-f310a5b538b9)<br>
<br>

Type http:// in the search bar first before pasting the public IP address of your new instance<br>
![http](https://github.com/mindmotivate/multicloudclass/assets/130941970/52cd78bf-e351-4086-8138-95190df0fd03)<br>

Instance details should be displayed on the screen:<br>

<br>
![InstanceDetails](https://github.com/mindmotivate/multicloudclass/assets/130941970/7b5369f9-0347-4362-aa2f-90bd03f493ee)
<br>


10. **Teardown Procedure:**<br>
Type "resource groups" in the top search bar<br>
Select the vpc you want to delete.<br>
Click on the Delete resource group button.<br>
In the confirmation dialog box,paste your copied resource group name<br>
If you want to delete all associated resources with the virtual machine, select the Delete all resources checkbox.<br>
Click on Delete to complete the deletion process.<br>

<br>

![resourcegroups](https://github.com/mindmotivate/multicloudclass/assets/130941970/9af3b2c9-d830-421c-9ea5-ff1d011d340e)<br>
<br>
![clickresourcename](https://github.com/mindmotivate/multicloudclass/assets/130941970/dda08429-fc8e-42cf-919c-38584cd8427f)<br>
<br>
![deleteresourcegroup](https://github.com/mindmotivate/multicloudclass/assets/130941970/2e119c40-db01-4042-b05c-b07bc1588a32)<br>
<br>
![deletegroupsteps](https://github.com/mindmotivate/multicloudclass/assets/130941970/5190e31b-a0a7-4758-910b-36b6d9703a7d)<br>
<br>

**Cheat Sheet:**
1. Sign in to your Azure account and navigate to the Azure portal.
2. In the search box at the top of the portal, enter "Virtual machine" and select "Virtual machines" from the search results.
3. Select "+ Add" to create a new virtual machine.
4. In the "Basics" tab, enter or select the following values:
    - Subscription: Select your subscription.
    - Resource group: Create a new resource group with a name of your choice.
    - Virtual machine name: Enter a name for your virtual machine.
    - Region: Select "East US".
    - Availability options: Select "Availability zone".
    - Availability zone: Select "1", "2", and "3".
    - Image: Select "Amazon Linux 2023 AMI".
    - Size: Choose a size for your virtual machine.
    - Authentication type: Select "SSH private key".
    - Username: Enter a username of your choice.
    - SSH public key source: Select "Generate new key pair".
    - Key pair name: Enter a name for your key pair.
5. In the "Networking" tab, enter or select the following values:
    - Virtual network: Create a new virtual network with a name of your choice.
    - Subnet name: Enter a name for your subnet.
    - Public IP address: Create a new public IP address with a name of your choice.
    - NIC network security group: Create a new network security group with a name of your choice.
6. In the "Management" tab, enter or select the following values:
    - Boot diagnostics: Enable boot diagnostics if desired.
7. In the "Advanced" tab, enter or select the following values:
    - Inbound port rules:
        * HTTP (80): Allow traffic from any source IP address (0.0.0.0/0).
        * SSH (22): Allow traffic from any source IP address (0.0.0.0/0).
8. Review and create your virtual machine.

That's it! You should now have an Azure virtual machine instance created in the US East region with availability zones in three regions, two inbound rules (HTTP and SSH), and an Amazon Linux 2023 AMI machine image.


```
