# aws-cloudformation-templates

CloudFormation templates for various infrastructure stacks.

## VPC with 4 subnets

Template filename: `templates/vpc-4subnets.yaml`

Deploy a basic VPC with 4 subnets:
- 2 publics and 2 privates
- across 2 fixed availability zones (`us-west-2a`, and `us-west-2b`)

### Validate

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


## VPC with 4 subnets and internet gateway

TBD

