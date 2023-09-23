# AWS Template Formation: Launching an EC2 from a template: <a name="example-templates-autoscaling"></a>

Greetings, Conclave members! In this tutorial we will walk through and provide documentation regarding the requires steps for creating and launching a template from an EC2 instance. We will begin with the creation of an EC2 instance and associated security group. From there we will able to create a template which we can then use to launch a specified number of EC2 instances with very little effort!\. 

This believe it or not, is one of the beginning stages of Automation in the cloud!\. 

## Please be advised: <a name="example-templates-autoscaling-full-stack-template"></a>

You must have a working AWS account first! Ensure you can log in without issue.\.
For the purposes of this tutorial we will log into our AWS console as (Root) user. (As we become more advanced cloud engineers we will refrain from using the root user)
\.

For more info on the `Rootuser`\. please click the following link: [AWS::RootUser](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html)\.

Let’s Begin!

Login to AWS console

Login:[AWS::RootUser](https://docs.aws.amazon.com/IAM)\.

Navigate to EC2 console: [AWS::EC2](https://docs.aws.amazon.com/)\.

Name your security group: In this tutorial we will name our security group: **conclavetemplate**\.

Describe you secuirty group: We will also describe it with the same name: **conclavetemplate**\.

All the inbound traffic to your soon to be created EC2 instance, will be blocked by default.  Additionally, all outbound traffic is allowed by default. We will implementing a set of rules that will allow outside traffic from the internet or other locations to access our EC2.\.

Ok, here are the "rules":\.

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

IF, you for any reason alter the out bound rules in any way...\.
The following will happen...\.

Insert Graphic\.

In all seriousness, do not touch outbound rules until you become an advanced user!\.

Outbound rules will remain untouched!\.

The last step here will be to add some tags (three or more)\.
Get in the habit of using descriptive tags for your security, is a security best practice and it helps with overall\. organization!\.

Let’s add some tags: (at least 3)\.

Name: pythoncrew\.
Owner: Chewbacca\.
Location: Colombia\.
Security: High\.

Press the "Create your security group button"!\.
