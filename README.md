# aws-cloudformation-templates

CloudFormation templates for various infrastructure stacks.

**EC2:**
- [EC2 with ingress on port 80](https://github.com/gabepublic/aws-cloudformation-templates#ec2-with-ingress-on-port-80)
- [EC2 with ingress on port 22](https://github.com/gabepublic/aws-cloudformation-templates#ec2-with-ingress-on-port-22)
- [EC2 with ingress on ports 22 & 80]()

**VPC:**
- [VPC with 4 subnets](https://github.com/gabepublic/aws-cloudformation-templates#vpc-with-4-subnets)
- [VPC with 1 public subnet, igw and EC2 + website](https://github.com/gabepublic/aws-cloudformation-templates#vpc-with-1-public-subnet-igw-and-ec2--website)
- [VPC with 2 public subnets, igw, load balancer, and EC2 + website](https://github.com/gabepublic/aws-cloudformation-templates#vpc-with-2-public-subnets-igw-load-balancer-and-ec2--website)
- [VPC 2 public subnets, bastion host, alb, ec2 + website](https://github.com/gabepublic/aws-cloudformation-templates#vpc-2-public-subnets-bastion-host-alb-ec2--website)
- [TBD VPC with 4 subnets (2 public & 2 private), igw, alb, and EC2 website & APIs ]()


## EC2 with ingress on port 80

Template filename: `templates/ec2-website-port80.yaml`

Deploys a basic EC2 instance with:
- very simple website hosted with httpd
- Security Group to allow ingress port 80 for accessing the website

**Goal:**
- to demonstrate how to setup a basic EC2 instance 
- enable the website, hosted in the EC2 instance, to be accessible from the
  internet through port 80, using the EC2 Security Group configuration
- NOTE: for a more comprehensive setup using VPC, please refer to 
  "VPC with 1 public subnet, internet gateway and EC2 + website" section below.  

**Note:**
With this basic setup, the EC2 instance will automatically be assigned a public
IP address. The egress from the EC2 instance to the internet is also possible 
due to the default "Security - Outbound rules" as shown in the aws console, 
"EC2 - Security" page below; even though it cannot be demonstrated with this 
setup, instead see "EC2 with ingress on ports 22" section below to validate it.  

### Create the stack using aws cli

```
$ aws cloudformation create-stack --stack-name "ec2-website-port80" --template-body file://./templates/ec2-website-port80.yaml
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

- As indicated above, egress to the internet from the ec2 instance is enabled, 
  but it cannot be demonstarted in this setup because the ec2 instance is not
  enabled for `ssh`. The egress will be validated on the next template 
  "EC2 with ingress on port 22".

### CLEANUP using aws cli

```
$ aws cloudformation delete-stack --stack-name "ec2-website-port80"
```


## EC2 with ingress on port 22

Template filename: `templates/ec2-website-port22.yaml`

Deploys a basic EC2 instance with:
- very simple website with httpd & but it's not acessible from the internet 
  because the security group is not enabled for ingress port 80.
  See the "EC2 with ingress on port 80" section above for how to enable access 
  to the website from internet
- the key-pair for ssh into the instance; need to be provided during the stack
  setup, as shown below
- Security group to allow ingress port 22 for ssh into the instance, and
  egress to the internet is enabled by default

**Goal:**
- to demonstrate how to enable SSH (port 22) to the EC2 instance from the
  internet using the ssh key-pair
- the egress to internet from the EC2 instance is enabled by default 


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
  `http://ec2-35-89-113-216.us-west-2.compute.amazonaws.com/index.html`
 
### CLEANUP using aws cli

```
$ aws cloudformation delete-stack --stack-name "ec2-website-port22"
```


## EC2 with ingress on ports 22 & 80

Template filename: `templates/ec2-website-port80.yaml`

Deploys a basic EC2 instance with:
- very simple website hosted with httpd
- the key-pair for ssh into the instance; need to be provided during the stack
  setup, as shown below
- Security Group to allow ingress ports: 22 & 80 for ssh & accessing the 
  website, respectively

**Goal:**
- to demonstrate how to setup a basic EC2 instance 
- enable the website, hosted in the EC2 instance, to be accessible from the
  internet through port 80, using the EC2 Security Group configuration


- to demonstrate how to setup a basic EC2 instance 
- enable the website, hosted in the EC2 instance, to be accessible from the
  internet through port 80, using the EC2 Security Group configuration
- enable ssh to be accessible from the internet via port 22


### Create the stack using aws cli

```
$ aws cloudformation create-stack --stack-name "ec2-website-ports22-80" --parameters ParameterKey=KeyName,ParameterValue=gabe2022oregon --template-body file://./tmp/ec2-website-ports22-80.yaml
``` 

### Validate

- The "Security - Inbound rules" page on the aws console should list two rules
  enabling port: 22 and 80

- The default "Security - Outbound rules" on the aws console allows egress 
  from the EC2 instance to the internet, by default. This can be demonstrated
  by ssh into the EC2 instance and perform:
```
$ curl www.google.com
```
 
### CLEANUP using aws cli

```
$ aws cloudformation delete-stack --stack-name "ec2-website-ports22-80"
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


## VPC with 1 public subnet, igw and EC2 + website 

Template filename: `templates/vpc-1publicsubnet-ec2-website.yaml`

Deploy a basic VPC with:
- one public Subnet
- across a fixed availability zones, `us-west-2a`
- Internet Gateway (igw) to allow the public subnet within the VPC to reach the
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

- Check the website can be reached using the EC2 instance public IP; 
  open browser and go to the url:
  `http://54.245.31.119/index.html`


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


## VPC with 2 public subnets, igw, load balancer, and EC2 + website

Template filename: `templates/vpc-2publicsubnet-alb-ec2-website.yaml`

Deploy a basic VPC with:
- two public subnets
- across a fixed availability zones, `us-west-2a` and `us-west-2b`
- Internet Gateway to allow the public subnets within the VPC to reach the
  internet
- an ec2 instance running in each subnets
- a website hosted by httpd running in the ec2 instance
- add the key-pair into the ec2 instance for ssh; the key-pair name is provided 
  during the `create-stack`; however, for this example we will not be able to
  ssh into the ec2 instance(s). We need another setup (called bastion) to
  enable ssh.

**Goal:**
- to demonstrate how to setup a simple VPC with 2 public subnets
- configure an Application Load Balancer to reach the 2 public subnets, and
- access the website hosted on the EC2 instances in the subnets
  
**References:**
- [Building a VPC with CloudFormation - Part 1](https://www.infoq.com/articles/aws-vpc-cloudformation/)
- [CloudFormation Template for VPC with EC2 and ALB](https://dev.classmethod.jp/articles/cloudformation-template-for-vpc-with-ec2-and-alb/)

### Create the stack using aws cli

```
$ aws cloudformation create-stack --stack-name "vpc-2pubsubnet-alb-ec2" --parameters ParameterKey=KeyName,ParameterValue=gabe2022oregon --template-body file://./templates/vpc-2publicsubnet-alb-ec2-website.yaml
``` 

### Validate

**Stack**

![Stack - Resources](/images/vpc-2publicsubnet-alb-ec2-website-stack-resources.jpg)


**Application Load Balancer**

![ALB - Description](/images/vpc-2publicsubnet-alb-ec2-website-alb-desc.jpg)

![ALB - Listeners](/images/vpc-2publicsubnet-alb-ec2-website-alb-listerners.jpg)

![ALB - Target Group](/imagesvpc-2publicsubnet-alb-ec2-website-alb-targetgroup.jpg)


- Note: the ec2 instances are not accessible from http port 80, even though
  they have assigned public ip address. Their Security Group, inbound rules
  only allow http port 80 from the ALB
  
- Check the website can be reached using the load balancer public IP that 
  can be found from the "AWS console > EC2 > Load Balancers", and select the
  alb name "vpc-2publicsubnet-alb-ec2-website-ALB". From the "Description" tab,
  copy the "DNS name". Open the browser and go to the url:
  `http://vpc-2-appli-9gvoqobobs-538715591.us-west-2.elb.amazonaws.com/`

- the webpage load balance regularlly between the two ec2 instance in the
  two availability zones, for example: us-west-2a, us-west-2b
  
- Confirm ssh is accessible by connecting directly to the ec2 instances
  public ip address. Reasons:
  - the ec2 SecurityGroup (see template `EC2SecurityGroup`) ingress allows
    `tcp:22`; unlike the `tcp:80` only allows source from `ELBSecurityGroup`
  - So, the `ELBSecurityGroup` has no effect to `tcp:80`
```
$ ssh -i "MyKeyPair.pem" ec2-user@<35.90.243.22>
```
  
  
### CLEANUP using aws cli

```
$ aws cloudformation delete-stack --stack-name "vpc-2pubsubnet-alb-ec2"
```


## VPC 2 public subnets, bastion host, alb, ec2 + website

Template filename: `templates/vpc-2publicsubnet-alb-bastion-ec2.yaml`

Deploy a basic VPC with:
- two public subnets

**Goal:**
- to demonstrate how to setup bastion
- bastion and other ec2 instances in the public subnets are reachable via ssh

**References:**
- [Launch and use a Bastion Host on AWS](https://scratchpad.blog/launch-and-use-a-bastion-host-on-aws/)
  - [cf template](https://s3.eu-central-1.amazonaws.com/com.carpinuslabs.cloudformation.templates/ops/bastion-host.yaml)
- [Best Practices for setting up a VPC](https://scratchpad.blog/best-practices-for-setting-up-a-vpc/)
  - [cf template](https://github.com/jenseickmeyer/cloudformation-templates/blob/master/network/vpc.yaml)
- [multi-tier-web-app-in-vpc.template](https://aws.amazon.com/cloudformation/templates/aws-cloudformation-templates-ap-northeast-1/)
  - [cf template](https://s3-ap-northeast-1.amazonaws.com/cloudformation-templates-ap-northeast-1/multi-tier-web-app-in-vpc.template)


### Create the stack using aws cli

```
$ aws cloudformation create-stack --stack-name "vpc-2pubsub-alb-bastion" --parameters ParameterKey=KeyName,ParameterValue=gabe2022oregon --template-body file://./templates/vpc-2publicsubnet-alb-bastion-ec2.yaml
``` 

### Validate

- From the aws console - EC2 page > instances, select "Bastion Host"

- On the EC2 page > instances > Bastion Host page, copy the Public IPv4 address

- Test ssh to bastion host
  - using ssh client, such as termius or putty, ssh into bastion host

- Test egress from bastion host; SUCCESS. Note: `ping` does not work. 
```
$ curl www.google.com
<!doctype html><html itemscope="" itemtype="http://schema.org/WebPage" lang="en"><head>
[...]
;</script>        </body></html>

$ ping www.google.com
PING www.google.com (142.251.33.100) 56(84) bytes of data.
^C
--- www.google.com ping statistics ---
14 packets transmitted, 0 received, 100% packet loss, time 13291ms
```

- Cannot use `ping` to reach the ec2 instances using private IP

- Exit from bastion host

- Next, setup Putty to use SSH agent forwarding;
  For details, see [Securely Connect to Linux Instances Running in a Private Amazon VPC](https://aws.amazon.com/blogs/security/securely-connect-to-linux-instances-running-in-a-private-amazon-vpc/)

- ssh into bastion host using public IP, username: ec2-user, and the ssh key
  ppk file
```
login as: ec2-user
Authenticating with public key "gabe2022oregon" from agent
Last login: Wed Aug 31 00:42:48 2022 from 98.37.95.79

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
No packages needed for security; 4 packages available
Run "sudo yum update" to apply all updates.
[ec2-user@ip-10-1-20-118 ~]$ curl www.google.com
<!doctype html><html itemscope="" itemtype="http://schema.org/WebPage" lang="en"><head>
[...]
;</script>        </body></html>
[ec2-user@ip-10-1-20-118 ~]$ 
```

- From the bastion host console, ssh into the other ec2 instances
```
[ec2-user@ip-10-1-20-118 ~]$ ssh ec2-user@10.1.20.208

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
[ec2-user@ip-10-1-20-208 ~]$ ls -al /var/www/html
total 4
drwxr-xr-x 2 root root 24 Aug 31 00:37 .
drwxr-xr-x 4 root root 33 Aug 31 00:37 ..
-rw-r--r-- 1 root root 38 Aug 31 00:37 index.html
[ec2-user@ip-10-1-20-208 ~]$ cat /var/www/html/index.html
<h1>Hello from Region us-west-2b</h1>
[ec2-user@ip-10-1-20-208 ~]$
[ec2-user@ip-10-1-20-208 ~]$ curl www.google.com
<!doctype html><html itemscope="" itemtype="http://schema.org/WebPage" lang="en"><head>
[...]
;</script>        </body></html>
[ec2-user@ip-10-1-20-208 ~]$ exit
[ec2-user@ip-10-1-20-118 ~]$
```

### CLEANUP using aws cli

```
$ aws cloudformation delete-stack --stack-name "vpc-2pubsub-alb-bastion"
```


## VPC with 4 subnets (2 public & 2 private), igw, alb, and EC2 + website

Demonstrate ssh ec2 in private subnets 

## Other todo setups:
- nat gateway - once bastion is working, setup nat and test private subnet ec2
  to access internet by ssh into the ec2 and curl www.google.com 

- https - need ssl cert and valid domain name
