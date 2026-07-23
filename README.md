Multi-AZ VPC Lab (AWS Console)

A hands-on AWS networking lab: designing and deploying a highly-available VPC across multiple Availability Zones, with public and private subnets, built manually through the AWS Console.

🧰 Tools & Services Used
Tool / Service	Purpose
AWS VPC	Isolated virtual network
AWS Internet Gateway (IGW)	Gives public subnets internet access
AWS NAT Gateway	Gives private subnets outbound-only internet access
Elastic IP (EIP)	Static public IP required by the NAT Gateway
Route Tables	Controls how traffic is routed in/out of each subnet
Subnets	Public and private network segments, one pair per AZ
EC2 (for testing)	Used to verify connectivity from each tier
AWS Console	All resources built manually — no IaC — to reinforce the underlying concepts
