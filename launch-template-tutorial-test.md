# AWS Template Formation: Launching an EC2 from a template: <a name="example-templates-autoscaling"></a>

Greetings, Conclave members! In this tutorial we will walk through and provide documentation regarding the requires steps for creating and launching a template from an EC2 instance. We will begin with the creation of an EC2 instance and associated security group. From there we will able to create a template which we can then use to launch a specified number of EC2 instances with very little effort!\. 

This believe it or not, is one of the beginning stages of Automation in the cloud!\. 

## Please be advised: <a name="example-templates-autoscaling-full-stack-template"></a>

You must have a working AWS account first! Ensure you can log in without issue.\.
For the purposes of this tutorial we will log into our AWS console as (Root) user. (As we become more advanced cloud engineers we will refrain from using the root user)
\.

For more info on the `Rootuser`\. please click the following link: [AWS::RootUser](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html)\.

**Let’s Begin!**

**Login to AWS console**</a>

+ Login:[AWS::RootUser](https://aws.amazon.com/console/)\.


**Create a Security Group:**</a>
One of the first tasks that we will perform, will be to set up a security group for the instance that we will create. Think of a Security Groups as a *mini-firewall*. It is a tool that helps control the inbound & outbound traffic for a Virtual Machine. Security groups are an important component of cloud security.\.

For additional info on the `Securitygroups`\. please click the following link: [AWS::SecurityGroups](https://docs.aws.amazon.com/IAM/latest/UserGuide/securitygroups)\.

+ Navigate to EC2 console (type EC2 in the top search bar)\.

+ Navigate to Security Console (located on the left hand side menu)\.

+ Select Launch a security group

+ Name your security group: In this tutorial we will name our security group: **conclavetemplate**\.

+ Describe you secuirty group: We will also describe it with the same name: **conclavetemplate**\.

**Note:** All the *inbound traffic* to your soon to be created EC2 instance, will be blocked by default.  Additionally, all *outbound traffic* is allowed by default. We will implementing a set of rules that will allow outside traffic from the internet or other locations to access our EC2.\.

**Ok, here are the "rules":**\.

Inbound Rule 1: TCP ingress rule that allows HTTP access (port 80) from anywhere IPv4\.
Inbound Rule 2: TCP ingress rule that allows SSH access (port 22) from anywhere IPv4\.
Inbound Rule 3: TCP ingress rule that allows RDP access (port 3389) from anywhere IPv4\.

You screen should look something like this:\.

Insert graphic:\.

Additonally, you have the option of adding descriptors for each rule:\.

HTTP 80 Anywhere IPv4	Description: **My Homepage**\.
SSH 22 Anywhere IPv4    Description: **SSH**\.
RDP 3389 Anywhere IPv4	Description: **Windows**\.

## Please read CAREFULLY!:\.

If you for any reason, alter the out bound rules in any way...\.

The following will happen...\.

Insert Graphic\.

In all seriousness, do not touch outbound rules until you become an advanced user!\.

Outbound rules will remain untouched!\.

The last step here will be to add some tags (three or more)\.
Get in the habit of using descriptive tags for your security, is a security best practice and it helps with overall\. organization!\.

Let’s add some tags: (at least 3)\.

Name: **pythoncrew**\.
Owner: **Chewbacca**\.
Location: **Colombia**\.
Security: **High**\.

Press the **"Create your security group** button"!\.

Congratulations! You have successfully created a security group!\.

**Create a EC2 Instance:**</a>
Our next tasks will be to create a EC2. EC2 stands for *Elastic Compute Cloud"
It is the Amazon Web Services version of a virtual machine\. 

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

Create a keypair (.pem) : template3\.

Which key pair type should you use?\.

**.pem vs. .ppk**


The .pem and .ppk file formats are both used for storing private key information of asymmetric key pairs, but they differ in the following ways\,

System/platform compatibility: The .pem format is commonly used in Linux BASH and Mac BASH environments, as well as in Windows/Linux/Mac PowerShell environments. On the other hand, the .ppk format is primarily used by Windows PuTTY/Cygwin users.\,

For additional info on the `KeyPairs`\. please click the following link: [AWS::KeyPair](https://docs.aws.amazon.com/IAM/latest/UserGuide/keypair)\.

Select the appropriate key pair for your operating system and download\.

**Network Settings:**</a>\.

Go to Network Settings and Select 'Existing Security Group'\.

Select the security group we made earler convenitenly titled: conclavetemplate\.

**Launch Script**</a.

Scroll down to Advanced Details\.

Here we will add the launch script displayed below: (copy an paste this code into your user data box)\.


**Copy Launch Script Below:**

```
AWSTemplateFormatVersion: 2010-09-09
Parameters:
  InstanceType:
    Description: The EC2 instance type
    Type: String
    Default: t3.micro
    AllowedValues:
      - t3.micro
      - t3.small
      - t3.medium
      
```