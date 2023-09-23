# AWS Template Formation: Launching an EC2 from a template: <a name="example-templates-autoscaling"></a>

Greetings, Conclave members! In this tutorial we will walk through and provide documentation regarding the requires steps for creating and launching a template from an EC2 instance. We will begin with the creation of an EC2 instance and associated security group. From there we will able to create a template which we can then use to launch a specified number of EC2 instances with very little effort!\. 

This believe it or not, is one of the beginning stages of Automation in the cloud!\. 

## Please be advised: <a name="example-templates-autoscaling-full-stack-template"></a>

You must have a working AWS account first! Ensure you can log in without issue.\.
For the purposes of this tutorial we will log into our AWS console as (Root) user. (As we become more advanced cloud engineers we will refrain from using the root user)
\.

For more info on the `Rootuser`\. please click the following link: [AWS::RootUser](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html)\.

## Login to AWS console: <a name="example-templates-autoscaling-full-stack-template"></a>

+ Login Link:[AWS::RootUser](https://aws.amazon.com/console/)\.

## Create a Security Group: <a name="example-templates-autoscaling-full-stack-template"></a>

One of the first tasks that we will perform, will be to set up a security group for the instance that we will create. Think of a Security Groups as a *mini-firewall*. It is a tool that helps control the inbound & outbound traffic for a Virtual Machine. Security groups are an important component of cloud security. They are also free... So please use them!\.

For additional info on the `Securitygroups`\. please click the following link: [AWS::SecurityGroups](https://docs.aws.amazon.com/IAM/latest/UserGuide/securitygroups)\.

+ Navigate to EC2 console (type EC2 in the top search bar)\.

+ Navigate to Security Console (located on the left hand side menu)\.

+ Select Launch a security group

+ Name your security group: In this tutorial we will name our security group: **conclavetemplate**\.

+ Describe you secuirty group: We will also describe it with the same name: **conclavetemplate**\.

**Note:** All the *inbound traffic* to your soon to be created EC2 instance, will be blocked by default.  Additionally, all *outbound traffic* is allowed by default. We will implementing a set of rules that will allow outside traffic from the internet or other locations to access our EC2.\.

## Inbound Rules: <a name="example-templates-autoscaling-full-stack-template"></a>

We will begin with the Inbound Rules. Please enter the following using the drop down menus:/.

Inbound Rule 1: TCP ingress rule that allows HTTP access (port 80) from anywhere IPv4\.
Inbound Rule 2: TCP ingress rule that allows SSH access (port 22) from anywhere IPv4\.
Inbound Rule 3: TCP ingress rule that allows RDP access (port 3389) from anywhere IPv4\.

You screen should look something like this:\.

Insert graphic:\.

**Add descriptions to your inbound rules:** 

Additonally, you have the option of adding descriptions for each rule. You may use thes following descriptions or create your own.\.

HTTP 80 Anywhere IPv4	Description: **My Homepage**\.
SSH 22 Anywhere IPv4    Description: **SSH**\.
RDP 3389 Anywhere IPv4	Description: **Windows**\.

## Outbound Rules: <a name="example-templates-autoscaling-full-stack-template"></a>

## Please read the following CAREFULLY!\.

If you for any reason, alter the out bound rules in any way...\.

The following will happen...\.

<img src="https://media.tenor.com/jWqSNxUmdVsAAAAd/no-hit.gif" alt="Dont Touch GIF" width="300" height="200">

In all seriousness, do not touch outbound rules until you become an advanced user!\.

Outbound rules will remain untouched!\.


**Add Tags:** 

The last step here will be to add some tags (three or more)\.
Get in the habit of using descriptive tags for your security, is a security best practice and it helps with overall\. organization!\.

Let’s add some tags: (at least 3)\.

Name: **pythoncrew**\.
Owner: **Chewbacca**\.
Location: **Colombia**\.
Security: **High**\.

Press the **"Create your security group** button"!\.

Congratulations! You have successfully created a security group!\.

## Create an EC2 Instance: <a name="example-templates-autoscaling-full-stack-template"></a>

Our next tasks will be to create a EC2. EC2 stands for *Elastic Compute Cloud"
It is the Amazon Web Services version of a virtual machine.\. 

For additional info on the `EC2`\. please click the following link: [AWS::EC2](https://docs.aws.amazon.com/IAM/latest/UserGuide/EC2.html)\.

+ Navigate to EC2 console (type EC2 in the top search bar)\.

+ Select Launch EC2 Instance\.

+ Name your EC2: In this tutorial we will name it: **conclavetemplate**\.

+ Describe EC2: We will also describe it as: **conclavetemplate**\.

By keeping our naming conventions consistent we will be able to keep things organized.\.

You screen should look something like this:\.

Insert graphic:\.

The follwing categories will remain untouched:\.



**Key Pair Creation:**</a>\.

Next, we will create a keypair

Which key pair type should you use?\.

Great question! It depends on your operating system, or in somee instances it is a matter of personal preference.\.

**.pem vs. .ppk**

The .pem and .ppk file formats are both used for storing private key information of asymmetric key pairs, but they differ in the following ways\,

System/platform compatibility: The .pem format is commonly used in Linux BASH and Mac BASH environments, as well as in Windows/Linux/Mac PowerShell environments. On the other hand, the .ppk format is primarily used by Windows PuTTY/Cygwin users.\,

For additional info on the `KeyPairs`\. please click the following link: [AWS::KeyPair](https://docs.aws.amazon.com/IAM/latest/UserGuide/keypair)\.

+ Select the appropriate key pair for your operating system and download\.

**Network Settings:**</a>\.

+ Go to Network Settings and Select 'Existing Security Group'\.

+ Select the security group we made earler convenitenly titled: conclavetemplate\.

**Launch Script**</a.

Here we will add a launch script that will intialize our ec2:\.
This process in known as bootstrapping
For additional info on `Bootstrapping`\. please click the following link: [AWS::Bootstrap](https://docs.aws.amazon.com/IAM/latest/UserGuide/bootstrapping.html)\.

+ Scroll down to Advanced Details\.

+ Copy an paste this code provided below into your user data box\..


**Copy Launch Script Below:**

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




Launch Instance
Click on your instance
Click on your IPv4 address to copy it
Go to your web browser and type in http:// first, then paste you address
http://54.221.3.90
Make sure it works!

Return to instances
Select template3
Go to Actions   Image and Templates  Create template from instance
You will be on: Create Launch Template

Call it template3
Description template3
Template: template3
Location: Austin
Owner: Chewbacca
Notice that template3 is chosen by default
The user data script should populate as well 
considering we using our original instance as the template for this image
Create Launch Template
Now we have a launch template named:  template3
This will help with automation!
Now go to EC2/launch templates
Check the box next to your newly created instance
Then select

Go to actions  launch instance from template
Now all of the info you need has already been populated
You can create multiple instances from this template if you choose to
After Launching instance, The result will be 5 additional, Identical instances
Now let’s see if we can SSH into our instance!
Go back to EC2 Dashboard and select your EC2 instance: template3
Select your original instance
Select connect:

Connect to Instance
Connect
You should see the “bird” 
