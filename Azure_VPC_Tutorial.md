
# Creating an Azure Virtual Private Network using your Azure Account


<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-azure-vpc/mmlogo.PNG" width="15%" height="15%">

In this tutorial we will walk through the various steps requiredfor creating and launching a Virtual Private Network on the Microsoft Azure Cloud. We will begin with the creation of a "Virtual Machine" (in the world of AWS, a virtual machine would be referred to as an EC2) and then we will proceed to create our first Virtual Private Cloud.<br>

*For addtional documentation regarding Azure Virtual Machines click the following link:[AZURE::VM](https://learn.microsoft.com/en-us/azure/virtual-machines/overview)<br>*

*For addtional documentation regarding Azure Virtual Private Clouds click the following link:[AZURE::VPC](https://www.c-sharpcorner.com/blogs/azure-vpc2#:~:text=Azure%20Virtual%20Private%20Cloud%20(VPC)%20provides%20a%20way%20to%20isolate,create%20subnets%2C%20and%20configure%20routing.).)<br>*

Ok, Let's Begin!<br>

1. **Log in to the Azure portal**: Go to the [Azure portal](https://portal.azure.com/) and sign in with your credentials.<br>

<img src="https://github.com/mindmotivate/multicloudclass/assets/130941970/efca307d-db83-4e9d-b93b-6560981b2b09" width="40%" height="40%">

2. **Select "Virtual Machine" Icon**: Once you are inside the Azure portal, you can then proceed to create a "Virtual Machine"<br>
    Please note that there are several ways to access and create various resources in Azure!<br>
    We can simply select the: **"Create a Resource"** icon and naviagte to the **"Virtual Machine"** icon:<br>


    **Alternate Method 1:** we can manually type in the words "virtual machine" via the portal search bar:
![vmportal](https://github.com/mindmotivate/multicloudclass/assets/130941970/fc539366-77d8-4002-9dbe-0840198cdc0d)

    **Alternate Method 2:** If you have previously used the virtual machine feature in the past, the icon may already be displayed on your dashboard!:<br> 
![vmportal3](https://github.com/mindmotivate/multicloudclass/assets/130941970/2ab965b4-8222-4f0b-91f1-c8ae5e5ee363)

    **Alternate Method 3:**
    Upon selecting the "three lines" at the top left corner of your screen, you will review a list of resources and services which        includes virtual machines:<br>
<img src="https://github.com/mindmotivate/multicloudclass/assets/130941970/bf49c6c9-3632-4343-b5b6-fb6f1ff2959a" width="50%" height="50%"><br>

    **Alternate Method 4:**  Although we will not cover this method in this tutotial, you have the ability to create a VM using the **( 
    "CLI"(command line interface)**

 *As you can see, there are a variety of menu options at your disposal!<br>*

7. select **Create a resource** > **Networking** > **Virtual network**. Fill out the required fields, including the name of your virtual network, the address space, and the subnet details. Make sure to select **US East** as the region for your virtual network.
Create a Virtual Network:
•	Log in to the Azure portal.
•	Select Create a resource > Networking > Virtual network.
•	Fill out the required fields, including the name of your virtual network, the address space, and the subnet details.
•	Make sure to select US East as the region for your virtual network.
•	Click on Review + create and then click on Create.
8. **Create availability zones**: In the Azure portal, select **Create a resource** > **High availability** > **Availability Zones**. Fill out the required fields, including the name of your availability zone and the region details. Make sure to select **US East** as the region for your availability zone.

9. **Create inbound rules**: In the Azure portal, select your virtual network and then select **Firewalls and virtual networks** > **Inbound security rules**. Create two inbound rules: one for HTTP and one for SSH, both allowing traffic from anywhere on IPV4.

10. **Choose an Amazon Linux 2023 AMI machine image**: In the Azure portal, select **Create a resource** > **Compute** > **Virtual machine**. Choose an Amazon Linux 2023 AMI machine image from the list of available images.
•	In the Azure portal, select Create a resource > Compute > Virtual machine.
•	Choose an Amazon Linux 2023 AMI machine image from the list of available images.
•	Fill out the required fields, including the name of your virtual machine, username, password, and disk type.
•	Make sure to select your virtual network and availability zone in this step.
•	Click on Review + create and then click on Create.


11. **Configure your virtual machine**: Fill out the required fields, including the name of your virtual machine, username, password, and disk type. Make sure to select your virtual network and availability zone in this step.

12. **Connect to your virtual machine**: Once your virtual machine is created, you can connect to it using SSH or RDP.

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


## Plugins

Dillinger is currently extended with the following plugins.
Instructions on how to use them in your own application are linked below.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |


## Development

Want to contribute? Great!

Dillinger uses Gulp + Webpack for fast developing.
Make a change in your file and instantaneously see your updates!

Open your favorite Terminal and run these commands.

First Tab:

```sh
node app
```

Second Tab:

```sh
gulp watch
```

(optional) Third:

```sh
karma test
```

#### Building for source

For production release:

```sh
gulp build --prod
```

Generating pre-built zip archives for distribution:

```sh
gulp build dist --prod
```
