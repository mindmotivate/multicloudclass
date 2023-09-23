# AWS Template Formation: Launching an EC2 from a template: <a name="example-templates-autoscaling"></a>

Greetings, Conclave members! In this tutorial we will walk through and provide documentation regarding the requires steps for creating and launching a template from an EC2 instance. We will begin with the creation of an EC2 instance and associated security group. From there we will able to create a template which we can then use to launch a specified number of EC2 instances with very little effort!\. 

This believe it or not, is one of the beginning stages of Automation in the cloud!\. 

## Please be advised: <a name="example-templates-autoscaling-full-stack-template"></a>

You must have a working AWS account to proceed! Ensure you can log in without issue.\.
For the purposes of this tutorial we will log into our AWS console as (Root) user. *(As we become more advanced cloud engineers we will refrain from using the root user)*
\.

For more info on the `Rootuser`\. please click the following link: [AWS::RootUser](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html)\.

## Login to AWS console: <a name="example-templates-autoscaling-full-stack-template"></a>

+ Login Link:[AWS::RootUser](https://aws.amazon.com/console/)\.

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages/rootsignin.JPG">

## Create a Security Group: <a name="example-templates-autoscaling-full-stack-template"></a>

One of the first tasks that we will perform, will be to set up a security group for the instance that we will create. Think of a Security Groups as a *mini-firewall*. It is a tool that helps control the inbound & outbound traffic for a Virtual Machine. Security groups are an important component of cloud security. They are also free... So please use them!\.

For additional info on the `SecurityGroups`\. please click the following link: [AWS::SecurityGroups](https://docs.aws.amazon.com/IAM/latest/UserGuide/securitygroups)\.

+ Navigate to EC2 console (type EC2 in the top search bar)\.

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages/ec2navigate.png">

+ Navigate to Security Console (located on the left hand side menu)\.

+ Select Launch a "create a security group" button\.

+ In the "Basic Details" category, name your security group: In this tutorial we will name our security group: **conclavetemplate**\.

+ Provide a description for your security group: We will also describe it with the same name: **conclavetemplate**\.

+ "VPC" will remain untouched (do not make any changes here)\.

*Your screen should look something like this:\.

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages/basicdetails.JPG">

## Inbound Rules: <a name="example-templates-autoscaling-full-stack-template"></a>

**Note:** All the *inbound traffic* to your soon to be created EC2 instance, will be blocked by default.  Additionally, all *outbound traffic* is allowed by default. We will implementing a set of rules that will allow outside traffic from the internet or other locations to access our EC2.\.

+ Scroll down to the "Inbound rule" section\.

+ Select "Add rule" button\.

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages/addrule.jpg">


Please create the following three inbound rules:\.

Inbound Rule 1: TCP ingress rule that allows HTTP access (port 80) from anywhere IPv4\.
Inbound Rule 2: TCP ingress rule that allows SSH access (port 22) from anywhere IPv4\.
Inbound Rule 3: TCP ingress rule that allows RDP access (port 3389) from anywhere IPv4\.


**Add descriptions to your inbound rules:** 

Additonally, you have the option of adding descriptions for each rule. You may use thes following descriptions or create your own.\.

HTTP 80 Anywhere IPv4	Description: **My Homepage**\.
SSH 22 Anywhere IPv4    Description: **SSH**\.
RDP 3389 Anywhere IPv4	Description: **Windows**\.

*Your screen should look something like this:\.

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages/inboundrulesJPG.JPG">



## Outbound Rules: <a name="example-templates-autoscaling-full-stack-template"></a>

## Please read the following CAREFULLY!\.

If you for any reason, alter the *outbound* rules in any way...\.

The following will happen...\.

<img src="https://media.tenor.com/jWqSNxUmdVsAAAAd/no-hit.gif" alt="Dont Touch GIF" width="300" height="200">

In all seriousness, do not touch outbound rules until you become an advanced user!\.

Outbound rules will remain untouched!\.

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages/Outboundrules.jpg">


**Add Tags:** 

The last step here will be to add some tags (three or more)\.
Get in the habit of using descriptive tags for your security, is a security best practice and it helps with overall organization!\.

Let’s add some tags: (at least 3)\.

Name: **pythoncrew**\.
Owner: **Chewbacca**\.
Location: **Colombia**\.
Security: **High**\.

Press the **"Create your security group** button"!\.

Congratulations! You have successfully created a security group!\.

## Create an EC2 Instance: <a name="example-templates-autoscaling-full-stack-template"></a>

Our next task will be to create an EC2 instance. EC2 stands for *Elastic Compute Cloud"
It is the Amazon Web Services version of a virtual machine.\. 

For additional info on the `EC2` please click the following link: [AWS::EC2](https://docs.aws.amazon.com/IAM/latest/UserGuide/EC2.html)\.

+ Navigate to EC2 console (type EC2 in the top search bar)\.

+ Select Launch EC2 Instance\.

+ Name your EC2: In this tutorial we will name it: **conclavetemplate**\.

+ Do not change the default setting for "Application and OS Images"

+ Do not change the default setting for "Instance Type"

*By keeping our naming conventions consistent we will be able to keep things organized.*\.

You screen should look something like this:\.

Insert graphic:\.


**Key Pair (login):**</a>\.

Next, we will create a "keypair"\.

+ Select "create new key pair"\.

+ Name your key pair: **conclavetemplate**\.

+ Select **RSA** for for "Key pair type" (this option shold already be selected by default)\.

+ You will choose either **.pem** or **.ppk** depending on your OS\.

**.pem vs. .ppk**

The .pem and .ppk file formats are both used for storing private key information of asymmetric key pairs, but they differ in the following ways\,

System/platform compatibility: The .pem format is commonly used in Linux BASH and Mac BASH environments, as well as in Windows 10/Linux/Mac PowerShell environments. On the other hand, the .ppk format is primarily used by Windows PuTTY/Cygwin users.\.


| **File Format** | **System/Platform Compatibility** | **File Contents** |
|-----------------|----------------------------------|-------------------|
| .pem            | Linux BASH, Mac BASH, Windows 10         | Contains private key information in a specific format |
| .ppk            | Windows PuTTY/Cygwin              | Similar to .pem but in a different format for use with PuTTY |


For additional info on the `KeyPairs`\. please click the following link: [AWS::KeyPair](https://docs.aws.amazon.com/IAM/latest/UserGuide/keypair)\.

+ Select the appropriate key pair for your operating system and download\.

**Network Settings:**</a>\.

+ Go to Network Settings and Select 'Existing Security Group'\.

+ Select the security group we made earler convenitenly titled: **conclavetemplate**\.

**Launch Script**</a.

Here we will add a launch script that will intialize our EC2.
This process in known as **bootstrapping**\.
Bootstrapping refers to the process of automatically launching and configuring an EC2 instance using a launch script. It allows you to customize the instance by automatically running commands or scripts when it starts up\.
For additional info on `Bootstrapping`\. please click the following link: [AWS::Bootstrap](https://docs.aws.amazon.com/IAM/latest/UserGuide/bootstrapping.html)\.

+ Scroll down to Advanced Details\.

+ Copy an paste the following code into your user data box:\.


**Copy Launch Script Below:**\.

```
#!/bin/bash
# Use this for your user data (script from top to bottom)
# install httpd (Linux 2 version)
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd

# Get the IMDSv2 token
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")

# Background the curl requests
curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/local-ipv4 &> /tmp/local_ipv4 &
curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/placement/availability-zone &> /tmp/az &
curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/network/interfaces/macs/ &> /tmp/macid &
wait

macid=$(cat /tmp/macid)
local_ipv4=$(cat /tmp/local_ipv4)
az=$(cat /tmp/az)
vpc=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/network/interfaces/macs/${macid}/vpc-id)

echo "
<!doctype html>
<html lang=\"en\" class=\"h-100\">
<head>
<title>Details for EC2 instance</title>
</head>
<body>
<div>
<h1>AWS Instance Details</h1>
<h1>Samurai Katana</h1>
<p><b>Instance Name:</b> $(hostname -f) </p>
<p><b>Instance Private Ip Address: </b> ${local_ipv4}</p>
<p><b>Availability Zone: </b> ${az}</p>
<p><b>Virtual Private Cloud (VPC):</b> ${vpc}</p>
</div>
</body>
</html>
" > /var/www/html/index.html

# Clean up the temp files
rm -f /tmp/local_ipv4 /tmp/az /tmp/macid
      
```








+ Launch your instance

+ Navigate to "Instances" and check the status of your newly created instance\.

+ Ensure that your instance appears in the list and that it is running\.

+ Select the "Instance ID" to acess your instance summary\.

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages/publicIPV4.JPG">

+ Copy the "IPv4 Address" 

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages/CopyIpv4.JPG">

+ Type "http://" into your web browser\.

+ Paste your copied Public IPv4 address into your web browser along with the "http://" prefix\.

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages/http.jpg">

+ After pressing enter, the following messsgae should appear:

<img src="https://raw.githubusercontent.com/mindmotivate/multicloudclass/gh-pages/instdetails.JPG">

## Create a Template from your EC2 instance: <a name="example-templates-autoscaling-full-stack-template"></a>\.
1. Launch the EC2 instance from which you want to create the launch template.
2. Once the instance is running, go to the **EC2 Dashboard** in the AWS Management Console.
3. Select the instance you just launched.
4. Go to **Actions** > **Image and Templates** > **Create Template from Instance**.
5. You will be taken to the **Create Launch Template** page.
6. Provide a name for your launch template (e.g., "template3").
7. Add a description for your template (e.g., "template3 description").
8. The template field should be pre-populated with the name of your instance.
9. Choose a location for your template (e.g., "Austin").
10. Specify the owner of the template (e.g., "Chewbacca").
11. Review the details and make sure that your user data script is populated correctly.
12. Click on **Create Launch Template**.

**Now you have a launch template named "conclavetemplate" that you can use for automation!\.**

To view your launch templates, go to **EC2** > **Launch Templates**.\.

To launch an instance from your template, select the box next to your newly created template and then click on **Actions** > **Launch Instance from Template**.\

All the necessary information will already be populated, and you can create multiple instances from this template if needed.

Let's go ahead and create three instances from your template.\.

After launching an instance from the template, you will have three additional instances.\.

For additional info on the `LaunchTemplates`\. please click the following link: [AWS::LaunchTemplate](https://docs.aws.amazon.com/IAM/latest/UserGuide/launch-templates.html)\.

