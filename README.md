# Multi-AZ VPC Lab (AWS Console)
### A hands-on AWS networking lab: designing and deploying a highly-available VPC across multiple Availability Zones, with public and private subnets, built manually through the AWS Console.

---
## 📖 Definitions
### 🌐 VPC (Virtual Private Cloud)
A logically isolated virtual network within AWS that you fully control — you define its IP range, and everything else (subnets, gateways, route tables) lives inside it. Think of it as your own private data center carved out of AWS's infrastructure.

### 🚪 Internet Gateway (IGW)
A component attached to a VPC that allows resources in public subnets to communicate directly with the internet, in both directions (inbound and outbound). Without it, nothing in your VPC can reach the internet at all.

### 🧩 Subnet
A subdivision of your VPC's IP range, tied to a single Availability Zone. Subnets are labeled "public" or "private" based on whether their route table sends internet-bound traffic to an Internet Gateway (public) or not (private) — it's the routing, not any inherent property of the subnet, that makes it public or private.

### 🗺️ Route Table
A set of rules ("routes") that determine where network traffic from a subnet is directed. Each subnet is associated with exactly one route table, which decides things like "traffic headed to 0.0.0.0/0 (the internet) goes to the Internet Gateway" or "...goes to the NAT Gateway."

### 🏢 Availability Zone (AZ)
One or more physically separate data centers within an AWS Region, each with independent power, cooling, and networking. Spreading resources across multiple AZs is how you achieve high availability — if one AZ has an outage, the others keep running.

---
## 🎯 Scenario
A company is launching a new web application on AWS.

**Requirements:**
- The application must remain available even if one Availability Zone goes down, so it needs to span **at least two AZs**.
- Web/app servers that need to be reachable from the internet must sit in **public subnets**.
- Databases and internal services must **never be directly reachable from the internet**. For this lab, private subnets are fully isolated from the internet (no outbound access) to keep the build cost-free — in a production environment, a NAT Gateway would typically be added here to allow controlled outbound access.
- The network should follow AWS best practices for a production-style environment: correct CIDR planning, one route table per tier, and no unnecessary public exposure.

**Goal:** Design and build the underlying VPC network that satisfies these requirements.

---
## 🔧 Solutions

### 1. 🌐 VPC 
VPC named 'Multi-AZ VPC' created sitting in region: Singapore (ap-southeast-1) with IPV4 CIDR 10.0.0.0/16.
<img width="725" height="712" alt="image" src="https://github.com/user-attachments/assets/7a34fa0c-1782-4168-b37b-b919e523a91f" />

### 2.🚪 Internet Gateway
Internet gateway created and attached to Multi-AZ VPC.

<img width="956" height="608" alt="image" src="https://github.com/user-attachments/assets/e1575e2a-0c2a-418e-be16-05acb907abb4" />

### 3. 🔓 Public Subnets
2 Public Subnets created in Singapore region (ap-southeast-1).

| Subnet Name | AZ | CIDR |
|---|---|---|
| public-subnet-1 | ap-southeast-1a | 10.0.1.0/24 |
| public-subnet-2 | ap-southeast-1b | 10.0.0.0/24 |

> <img width="728" height="862" alt="image" src="https://github.com/user-attachments/assets/d77d7766-7589-42a2-8812-6e98bdbdc08b" />

> <img width="737" height="867" alt="image" src="https://github.com/user-attachments/assets/17829c10-d1af-4648-b3ba-a146c69846f0" />

### 4. 🔒 Private Subnets

2 Private Subnets are created in Singapore region (ap-southeast-1) and are attached to Multi-AZ VPC. Private-subnet-1 with IPV4 CIDR 10.0.10.0/24 sits in availability zone (ap-southeast-1a) and private-subnet-2 with IPV4 CIDR 10.0.11.0/24 sits in availability zone (ap-southeast-1b).

| Subnet Name | Availability Zone | CIDR Block |
|---|---|---|
| private-subnet-1 | ap-southeast-1a | 10.0.10.0/24 |
| private-subnet-2 | ap-southeast-1b | 10.0.11.0/24 |

<img width="729" height="851" alt="image" src="https://github.com/user-attachments/assets/868149cd-0f0d-4abb-aac7-82f4a008ba62" />

<img width="738" height="904" alt="image" src="https://github.com/user-attachments/assets/9ac832e7-6acb-44c6-a1ed-8d0ca0de38a4" />

#### 📌Auto-assign public IPv4 gives instances a public IP so the internet can reach them.
Public subnet: enable it — these resources (web servers, load balancers) need to be reachable from outside.
Private subnet: leave it disabled — these resources (app servers, databases) should stay unreachable from outside, even though they can still reach out to the internet via the NAT Gateway.

### 5. Create the Public Route Table

A route table named `public-rt` was created and associated with `Multi-AZ-VPC`. A route was added to direct all internet-bound traffic (`0.0.0.0/0`) to the Internet Gateway, and the table was associated with both public subnets.

| Route Table | Destination | Target |
|---|---|---|
| public-rt | 0.0.0.0/0 | Internet Gateway |

**Subnets associated:** `public-subnet-1`, `public-subnet-2`

<img width="735" height="914" alt="image" src="https://github.com/user-attachments/assets/5521e950-e8bd-4f15-9d8d-ff6efc98c0dc" />

### 6. Create the Private Route Table

A route table named `private-rt` was created and associated with `Multi-AZ VPC`. No additional routes were added — only the default **local** route is present, meaning private subnet resources can communicate within the VPC but have no path to the internet.

This was a deliberate choice for this lab to avoid NAT Gateway costs. In a production deployment, a NAT Gateway route (`0.0.0.0/0 → NAT Gateway`) would typically be added here to allow controlled outbound internet access while keeping the subnet unreachable from outside.

| Route Table | Destination | Target |
|---|---|---|
| private-rt | 10.0.0.0/16 | local |

**Subnets associated:** `private-subnet-1`, `private-subnet-2`

<img width="718" height="910" alt="image" src="https://github.com/user-attachments/assets/eb25e8ff-7d0e-4e69-b11c-2148a6cab16e" />










