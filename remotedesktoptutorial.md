# Remote Desktop Tutorial: Launching a Virtual Windows Machine: <a name="example-templates-autoscaling"></a>

## Step 1: Create an EC2 Instance
To create an EC2 instance, follow these instructions:
<img src="">
<img src="">
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


6. Configure the security group rules to allow inbound RDP traffic on port 3389.
*We will utlize the security group that we created in the previous tutorial. Please ensure that your tutorial has an inbound rule set for RDP traffic on port 3389.*
<img src="">
<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/existingsecuritygroup.JPG">


7. Review your settings and click on "Launch" to create the EC2 instance.
*Click on the instance id number to access the instance detail screen*

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/launchbutton.JPG">

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/sucessfulintialization.JPG">

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/runninginstanceJPG.JPG">

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/instancedetails.JPG">



## Step 2: Connect to Your Instance
To connect to your EC2 instance using RDP, follow these instructions:


1. From your instance detail screen, click on "Connect"

2. Select the "RDP client" tab.
<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/connecttoinstance.JPG">


4. Click on "Get Password".
<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/getpassword.JPG">

<img src="">
5. Download the password to your desktop
*this may take a few minutes*

6. When prompted, upload your .pem file.
<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/uploadpem.JPG">

<img src="">
7. Decrypt your password
<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/getpassword.JPG">


8. After decrypting, copy the password to your clipboard and paste in a new .txt or in your notepad for safe keeping. You will need to use it again very soon.


## Step 3: Configure RDP Access
To configure RDP access for your EC2 instance, follow these instructions:

1. Open the Amazon EC2 console.

2. In the navigation pane, select "Instances" and choose your instance.

3. Click on "Actions", then "Networking", and then "Change Security Groups".

4. Select the security group associated with your instance and click on "Save".

5. Ensure that inbound RDP traffic from your IP address is allowed in the security group rules.

## Step 4: Connect to Your Instance Using RDP
To connect to your EC2 instance using an RDP client, follow these instructions based on your operating system:


 **Windows**: Windows includes an RDP client by default. Open the "Remote Desktop Connection" application and enter the public IP address or DNS name of your EC2 instance.


<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/Welcome%20to%20your%20virtual%20environment!.JPG">


<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages-rdp/edgebroseronvirtualmachine.JPG">



 **Windows**: Windows includes an RDP client by default. Open the "Remote Desktop Connection" application and enter the public IP address or DNS name of your EC2 instance.

- **Mac OS**: Download and install the Microsoft Remote Desktop app from the Mac App Store. Open the app and add a new connection using the public IP address or DNS name of your EC2 instance.
- **Linux**: Use Remmina or any other RDP client available for Linux. Configure a new connection with the public IP address or DNS name of your EC2 instance.

For additional instructions on how to set up an ssecurity group refer to my previous tutorial:\. 

For additional info on the `RDP`\. please click the following link: [AWS::RDP](https://docs.aws.amazon.com/IAM/latest/UserGuide/RDP.html)