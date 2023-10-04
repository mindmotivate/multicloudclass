
# Creating an Azure Virtual Private Network using your Azure Account

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-azure-vpc/mmlogo.PNG" width="15%" height="15%">

1. **Log in to the Azure portal**: Go to the [Azure portal](https://portal.azure.com/) and sign in with your credentials.

2. **Create a virtual network**: In the Azure portal, select **Create a resource** > **Networking** > **Virtual network**. Fill out the required fields, including the name of your virtual network, the address space, and the subnet details. Make sure to select **US East** as the region for your virtual network.
Create a Virtual Network:
•	Log in to the Azure portal.
•	Select Create a resource > Networking > Virtual network.
•	Fill out the required fields, including the name of your virtual network, the address space, and the subnet details.
•	Make sure to select US East as the region for your virtual network.
•	Click on Review + create and then click on Create.
3. **Create availability zones**: In the Azure portal, select **Create a resource** > **High availability** > **Availability Zones**. Fill out the required fields, including the name of your availability zone and the region details. Make sure to select **US East** as the region for your availability zone.

4. **Create inbound rules**: In the Azure portal, select your virtual network and then select **Firewalls and virtual networks** > **Inbound security rules**. Create two inbound rules: one for HTTP and one for SSH, both allowing traffic from anywhere on IPV4.

5. **Choose an Amazon Linux 2023 AMI machine image**: In the Azure portal, select **Create a resource** > **Compute** > **Virtual machine**. Choose an Amazon Linux 2023 AMI machine image from the list of available images.
•	In the Azure portal, select Create a resource > Compute > Virtual machine.
•	Choose an Amazon Linux 2023 AMI machine image from the list of available images.
•	Fill out the required fields, including the name of your virtual machine, username, password, and disk type.
•	Make sure to select your virtual network and availability zone in this step.
•	Click on Review + create and then click on Create.


6. **Configure your virtual machine**: Fill out the required fields, including the name of your virtual machine, username, password, and disk type. Make sure to select your virtual network and availability zone in this step.

7. **Connect to your virtual machine**: Once your virtual machine is created, you can connect to it using SSH or RDP.

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
