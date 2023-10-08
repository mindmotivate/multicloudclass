# Remote Desktop Protocal Tutorial: Launching a Virtual Windows Machine <a name="example-templates-autoscaling"></a>
(RDP) is a Microsoft proprietary protocol that enables remote connection to an EC2 instance, typically over TCP port 3389. It provides network access for a remote user over an encrypted channel. For additional info on the `RDP`\. please click the following link: [AWS::RDP](https://learn.microsoft.com/en-us/troubleshoot/windows-server/remote/understanding-remote-desktop-protocol)

## Step 1: Create an EC2 Instance
To create an EC2 instance, follow these instructions:\.

1. Launch the AWS Management Console.

2. Navigate to the Amazon EC2 service.

3. Click on "Launch Instance" to start the instance creation process.
*In this example we will name our instance "Windowsbox"*
<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/launchinstance.JPG">

4. Select the desired Windows-based 2022 AMI (Amazon Machine Image) from the available options.

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/Windowserver.JPG">
5. Choose the appropriate instance type, storage, and other configuration settings as per your requirements.
*We must select a windows machine so that we can properly execute the remote desktop server*
<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/createkeypair.JPG">


6. Select your previously created security group.
*We will utlize the security group that we created in the previous tutorial. Please ensure that your tutorial has an inbound rule set for RDP traffic on port 3389.*
<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/existingsecuritygroup.JPG">

7. Create a new key pair and download it to your computer.
*Note that the OS systen used in this example is Windows 10, therefore we are creating a .pem pair, You do have the option of using a .ppk for use with Putty or another operating system* Additonal info regarding Putty [AWS::Putty](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html#putty-private-key)

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/createnewkeypair.JPG">

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/createkeypair.JPG">


7. Review your settings and click on "Launch" to create the EC2 instance.

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/launchbutton.JPG">

8. View Instance Details.

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/sucessfulintialization.JPG">

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/runninginstanceJPG.JPG">

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/instancedetails.JPG">

*Note: Security groups enable you to control traffic to your instance, including the kind of traffic that can reach your instance*
*To connect to your instance.Ensure that the security group associated with your instance allows incoming RDP traffic (port 3389) from your IP address. The default security group does not allow incoming RDP traffic by default.*

## Step 2: Connect to Your Instance
To connect to your EC2 instance using RDP, follow these instructions:

1. From your instance detail screen, click on "Connect".

2. Select the "RDP client" tab.
<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/connecttoinstance.JPG">

4. Click on "Get Password".
<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/getpassword.JPG">

*You may be prompted to wait a few minutes after clicking on "Get Password"*

5. Download the password to your desktop when prompted.

6. Decrypt your password

8. After decrypting, store it somewhere safe!
*You may wan to copy the password to your clipboard and paste in a new .txt or in your notepad for safe keeping. You will need to use it again very soon.*

6. Upload your .pem file. 
*You download the .pem file during the instance creation step
<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/uploadpem.JPG">

## Step 3: Launch Windows RDP Client
To configure RDP access for your EC2 instance, follow these instructions:

1. Navigate to your Windows task bar and enter "RDP" in the search field
<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/typetosearch.JPG">
<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/rdptaskbarsearch.JPG">

2. Launch the Remote Desktop Application as an Administrator
<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/rdpadmin.JPG">

## Step 4: Configure Your RDP Client
*Next, We will retrieve the necessary credentials to access the remote Desktop Application*

1. Obtain the server name from your instance details.
*Copy the DNS server information*
<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/dnscopied.JPG">

Paste the DNS server name into you RDP client "Computer" field.
<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/rdpconn.JPG">

2. Type in the word "Administrator" as the name in your RDP client
*Be sure to use a capitol "A" when typing Adminstrator*

3. Paste your previously decrypted password in the "password" field
**This passoword saved in a "safe place" earlier 

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/Admincredentials.JPG">

4. Accept the Remote Desktop Connection certificate when prompted to continue.
<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/acceptcertificateJPG.JPG">

5. Congratulations! You have just configured your first virtual environment!
*You should see the familiar windows desktop screen appear on your desktop*


To connect to your EC2 instance using an RDP client, follow these instructions based on your operating system:


<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/Welcome%20to%20your%20virtual%20environment!.JPG">


<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/edgebroseronvirtualmachine.JPG">


For additional instructions regarding Remote Desktop setup for both MAC and Linux Operating Systems refer to the following documentation:

- **Mac OS**: Download and install the Microsoft Remote Desktop app from the Mac App Store. Open the app and add a new connection using the public IP address or DNS name of your EC2 instance.
[AWS::EC2/MAC](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-mac-instances.html)
- **Linux**: Use Remmina or any other RDP client available for Linux. Configure a new connection with the public IP address or DNS name of your EC2 instance.[MEDIUM::REMMINA](https://medium.com/@leopardsaga/remmina-ssh-aws-ec2-instance-463b3f2cad7)

