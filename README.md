# aws-cloudformation-templates

CloudFormation templates for various infrastructure stacks.


## EC2 with ingress on port 80

Template filename: `templates/ec2-website-port80.yaml`

Deploys a basic EC2:
- very simple website with httpd
- security group to allow ingress port 80 for accessing the website

**Goal:**
- to demonstrate how to setup EC2 instance 
- enable the website, hosted in the EC2 instance, to be accessible from the
  internet through port 80, using the EC2 Security Group configuration
- NOTE: for a more comprehensive approach using VPC, please refer to 
  "VPC with 1 public subnet, internet gateway and EC2 + website" section below.  

### Create the stack using aws cli

```
aws cloudformation create-stack --stack-name "ec2-website-port80" --template-body file://./templates/ec2-website-port80.yaml
``` 

### Validate

**Stack**

![Stack - EC2](/images/ec2-website-port80-stack.jpg)

![Stack - EC2 Resources](/images/ec2-website-port80-stack-resources.jpg)


**EC2**

![EC2 - Details](/images/ec2-website-port80-ec2-details.jpg)

![EC2 - Security](/images/ec2-website-port80-ec2-security.jpg)

![EC2 - Networking](/images/ec2-website-port80-ec2-networking.jpg)

![EC2 - Storage](/images/ec2-website-port80-ec2-storage.jpg)

![EC2 - Tags](/images/ec2-website-port80-ec2-tags.jpg)


**Webpage**

![Webpage](/images/ec2-website-port80-webpage.jpg)

- Egress to the internet from the ec2 instance is also enabled this security 
  group approach, but cannot be tested in this case (or using this particuar 
  cloudformation template) because the ec2 instance is enabled for `ssh`. 
  The egress will be validated on the next template "EC2 with ingress on port 22".

### CLEANUP using aws cli

```
$ aws cloudformation delete-stack --stack-name "ec2-website-port80"
```


## EC2 with ingress on port 22

Template filename: `templates/ec2-website-port22.yaml`

Deploys a basic EC2:
- very simple website with httpd & but it's not acessible from the internet 
  because the security group is not enabled for ingress port 80.
  See the "EC2 with ingress on port 80" section above how to enable access to
  the website from internet
- the key-pair for ssh into the instance; need to be provided during stack setup,
  either aws console or aws cli
- security group to allow ingress port 22 for ssh into the instance, and
  egress to the internet

**Goal:**
- to demonstrate how to enable SSH through port 22 to the EC2 instance from the
  internet

## Prerequisite - create the key-pair

The key pair is used to connect to the EC2 instance using SSH.
See [AWS - Create key pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/create-key-pairs.html) for details.

### Create the stack using aws cli

```
aws cloudformation create-stack --stack-name "ec2-website-port22" --parameters ParameterKey=KeyName,ParameterValue=gabe2022oregon --template-body file://./templates/ec2-website-port22.yaml
``` 

### Validate

**EC2**

![EC2 Security](/images/ec2-website-port22-ec2-security.jpg)

![EC2 Security](/images/ec2-website-port22-ec2-networking.jpg)


- SSH into the ec2 instance using the private key-pair, and `username: ec2-user`

![SSH Login](/images/ec2-website-port22-ssh-login.jpg)

![SSH](/images/ec2-website-port22-ssh.jpg)

- test the egress to the internet from the ec2 instance is successful by:
```
$ ping www.google.com
```

- Check the website cannot be reached; open browser and go to the url:
  `https://ec2-35-89-113-216.us-west-2.compute.amazonaws.com/index.html`
 
### CLEANUP using aws cli

```
$ aws cloudformation delete-stack --stack-name "ec2-website-port22"
```


## VPC with 4 subnets

Template filename: `templates/vpc-4subnets.yaml`

Deploy a basic VPC with 4 subnets:
- 2 publics and 2 privates
- across 2 fixed availability zones (`us-west-2a`, and `us-west-2b`)

**Goal:**
- to demonstrate how to create the basic barebone VPC with 4 subnets 

### Create the stack

#### Using aws cli

```
aws cloudformation create-stack --stack-name "vpc-4subnets" --template-body file://./templates/vpc-4subnets.yaml
``` 

#### Using aws management console

- Go to AWS CloudFormation console
- Click "Create Stack"
- On the create stack page:
  - Prepare template: Template is ready
  - Specify template - Template source: Upload a template file
  - Click Choose file, select the file and upload
  - Click Next, and provide the Stack name: vpc-4subnets
  - Click Next, and review the "Review vpc-4subnets" page
  - Finally, click "Create stack"


### Validate

**Stack**

![Stack - VPC](/images/vpc-4subnets-stack.jpg)

**VPC**

![VPC - Details](/images/vpc-4subnets-vpc-details.jpg)

![VPC - CIDR](/images/vpc-4subnets-vpc-cidr.jpg)

![VPC - Flow Logs](/images/vpc-4subnets-vpc-flowlogs.jpg)

![VPC - Tags](/images/vpc-4subnets-vpc-tags.jpg)


**VPC Subnet Public**

![VPC Subnet Public - Details](/images/vpc-4subnets-subnet-public-details.jpg)

![VPC Subnet Public - Details](/images/vpc-4subnets-subnet-public-flowlogs.jpg)

![VPC Subnet Public - Details](/images/vpc-4subnets-subnet-public-routetable.jpg)

![VPC Subnet Public - Details](/images/vpc-4subnets-subnet-public-networkacl.jpg)

![VPC Subnet Public - Details](/images/vpc-4subnets-subnet-public-cidrreservation.jpg)

![VPC Subnet Public - Details](/images/vpc-4subnets-subnet-public-sharing.jpg)

![VPC Subnet Public - Details](/images/vpc-4subnets-subnet-public-tags.jpg)


**VPC Subnet Private**

![VPC Subnet Private - Details](/images/vpc-4subnets-subnet-private-details.jpg)

![VPC Subnet Private - Details](/images/vpc-4subnets-subnet-private-flowlogs.jpg)

![VPC Subnet Private - Details](/images/vpc-4subnets-subnet-private-routetable.jpg)

![VPC Subnet Private - Details](/images/vpc-4subnets-subnet-private-networkacl.jpg)

![VPC Subnet Private - Details](/images/vpc-4subnets-subnet-private-cidrreservation.jpg)

![VPC Subnet Private - Details](/images/vpc-4subnets-subnet-private-sharing.jpg)

![VPC Subnet Private - Details](/images/vpc-4subnets-subnet-private-tags.jpg)


**VPC Route table**

![VPC Route table - Details](/images/vpc-4subnets-routetable-details.jpg)

![VPC Route table - Routes](/images/vpc-4subnets-routetable-routes.jpg)

![VPC Route table - Subnet Associations](/images/vpc-4subnets-routetable-subnetassociations.jpg)

![VPC Route table - Edge Associations](/images/vpc-4subnets-routetable-edgeassociations.jpg)

![VPC Route table - Route Propagation](/images/vpc-4subnets-routetable-routepropagation.jpg)

![VPC Route table - Tags](/images/vpc-4subnets-routetable-tags.jpg)


**VPC DHCP option sets**

![VPC DHCP option sets - Details](/images/vpc-4subnets-dhcpoptionsets-details.jpg)

![VPC DHCP option sets - Tags](/images/vpc-4subnets-dhcpoptionsets-tags.jpg)


**VPC Manage Prefix Lists**

![VPC Manage Prefix Lists](/images/vpc-4subnets-manageprefix-list.jpg)

![VPC Manage Prefix Lists - Cloudfront Details](/images/vpc-4subnets-manageprefix-cloudfront-details.jpg)

![VPC Manage Prefix Lists - Cloudfront Entries](/images/vpc-4subnets-manageprefix-cloudfront-entries.jpg)

![VPC Manage Prefix Lists - Cloudfront Tags](/images/vpc-4subnets-manageprefix-cloudfront-tags.jpg)


**VPC Network ACL**

![VPC Network ACL - Details](/images/vpc-4subnets-networkacl-details.jpg)

![VPC Network ACL - Inbound rules](/images/vpc-4subnets-networkacl-inboundrules.jpg)

![VPC Network ACL - Outbound rules](/images/vpc-4subnets-networkacl-outboundrules.jpg)

![VPC Network ACL - Subnet Associations](/images/vpc-4subnets-networkacl-subnetassociations.jpg)

![VPC Network ACL - Tags](/images/vpc-4subnets-networkacl-tags.jpg)


**VPC Security Group**

![VPC Security Group - Details](/images/vpc-4subnets-securitygroup-details.jpg)

![VPC Security Group - Inbound rules](/images/vpc-4subnets-securitygroup-inboundrules.jpg)

![VPC Security Group - Outbound rules](/images/vpc-4subnets-securitygroup-outboundrules.jpg)

![VPC Security Group - Tags](/images/vpc-4subnets-securitygroup-tags.jpg)


### CLEANUP

#### Using aws cli

- Delete the cloudformation stack
```
$ aws cloudformation delete-stack --stack-name "vpc-4subnets"
```

#### Using aws management console

- Delete the CloudFormation stack

- Delete the VPC; all the subnets should also be deleted automatically


## VPC with 1 public subnet, internet gateway and EC2 + website 

Template filename: `templates/vpc-1publicsubnet-ec2-website.yaml`

Deploy a basic VPC with:
- one public Subnet
- across a fixed availability zones, `us-west-2a`
- Internet Gateway to allow the public subnet within the VPC to reach the
  internet
- an ec2 instance running in the subnet
- a website hosted by httpd running in the ec2 instance
- add the key-pair into the ec2 instance for ssh; the key-pair name is provided 
  during the `create-stack`

**Goal:**
- to demonstrate how to setup a simple VPC with 1 subnet
- configure the VPC for accessing the internet using the Internet Gateway
- configure the subnet within the VPC to be public
- configure the public subnet to allow egress access to the internet;
  NOTE: there is an implicit entry included on every route table called the 
  "local" route, all trafffic for 10.1.0.0/16 stays within the VPC.
- configure an EC2 instance running in the public subnet
- configure the EC2 instance to have a public IP
- configure the EC2 Security Group to allow ingress through 
  ports 22 and 80, for ssh and accessing the website hosted on the EC2 instance,
  respectively.
  
**References:**
- [Building a VPC with CloudFormation - Part 1](https://www.infoq.com/articles/aws-vpc-cloudformation/)
- [AWS - Difference between NAT Gateway and Internet Gateway](https://explainexample.com/computers/aws/aws-difference-between-nat-gateway-and-internet-gateway)

### Create the stack using aws cli

```
$ aws cloudformation create-stack --stack-name "vpc-1publicsubnet-ec2-website" --parameters ParameterKey=KeyName,ParameterValue=gabe2022oregon --template-body file://./templates/vpc-1publicsubnet-ec2-website.yaml
``` 

### Validate

- SSH into the ec2 instance using the private key-pair, and `username: ec2-user`

- test the egress to the internet from the ec2 instance is successful by:
```
$ ping www.google.com
```

- Check the website cannot be reached; open browser and go to the url:
  `https://ec2-35-89-113-216.us-west-2.compute.amazonaws.com/index.html`


**Stack**

![Stack - Stack Info](/images/vpc-1publicsubnet-ec2-website-stack-info.jpg)

![Stack - Stack Resources](/images/vpc-1publicsubnet-ec2-website-stack-resources.jpg)


**Internet Gateway**

![Stack - Internet Gateway](/images/vpc-1publicsubnet-ec2-website-igateway.jpg)


**Subnet**

![Stack - Subnet Route Table](/images/vpc-1publicsubnet-ec2-website-subnet-routetable.jpg)

![Stack - Subnet Network ACL](/images/vpc-1publicsubnet-ec2-website-subnet-networkacl.jpg)


**EC2 - Security**

![Stack - EC2 Security](/images/vpc-1publicsubnet-ec2-website-ec2-security.jpg)

![Stack - EC2 Networking](/images/vpc-1publicsubnet-ec2-website-ec2-networking.jpg)

![Stack - EC2 Security Group Inbound Rules](/images/vpc-1publicsubnet-ec2-website-ec2-secgroup-inboundrules.jpg)

![Stack - EC2 Security Group Outbound Rules](/images/vpc-1publicsubnet-ec2-website-ec2-secgroup-outboundrules.jpg)


### CLEANUP using aws cli

```
$ aws cloudformation delete-stack --stack-name "vpc-1publicsubnet-ec2-website"
```


## VPC with 4 subnets, internet gateway and load balancer

```

$ aws cloudformation create-stack --stack-name "vpc-2publicsubnet-alb-ec2-website" --parameters ParameterKey=KeyName,ParameterValue=gabe2022oregon --template-body file://./templates/vpc-2publicsubnet-alb-ec2-website.yaml

``` 

