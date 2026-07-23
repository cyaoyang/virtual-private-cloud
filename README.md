Multi-AZ VPC Lab (AWS Console)

A hands-on AWS networking lab: designing and deploying a highly-available VPC across multiple Availability Zones, with public and private subnets, built manually through the AWS Console.

Definitions
🌐 VPC (Virtual Private Cloud)
A logically isolated virtual network within AWS that you fully control — you define its IP range, and everything else (subnets, gateways, route tables) lives inside it. Think of it as your own private data center carved out of AWS's infrastructure.

🚪 Internet Gateway (IGW)
A component attached to a VPC that allows resources in public subnets to communicate directly with the internet, in both directions (inbound and outbound). Without it, nothing in your VPC can reach the internet at all.

🧩 Subnet
A subdivision of your VPC's IP range, tied to a single Availability Zone. Subnets are labeled "public" or "private" based on whether their route table sends internet-bound traffic to an Internet Gateway (public) or not (private) — it's the routing, not any inherent property of the subnet, that makes it public or private.

🗺️ Route Table
A set of rules ("routes") that determine where network traffic from a subnet is directed. Each subnet is associated with exactly one route table, which decides things like "traffic headed to 0.0.0.0/0 (the internet) goes to the Internet Gateway" or "...goes to the NAT Gateway."

🏢 Availability Zone (AZ)
One or more physically separate data centers within an AWS Region, each with independent power, cooling, and networking. Spreading resources across multiple AZs is how you achieve high availability — if one AZ has an outage, the others keep running.

🔁 NAT Gateway (Network Address Translation)
A managed AWS service placed in a public subnet that lets resources in private subnets initiate outbound connections to the internet (e.g. to download software updates) while remaining unreachable from the internet inbound. It's billed hourly plus data processing charges.

📌 Elastic IP (EIP)
A static, public IPv4 address that you allocate and can attach to resources like a NAT Gateway. Unlike a regular auto-assigned public IP, it stays the same even if the underlying resource is stopped/restarted — a NAT Gateway requires one to function.

🎯 Scenario
> A company is launching a new web application on AWS. 
>
> Requirements:
> - The application must remain available even if one Availability Zone goes down, so it needs to span **at least two AZs**.
> - Web/app servers that need to be reachable from the internet must sit in **public subnets**.
> - Databases and internal services must **never be directly reachable from the internet**, but still need outbound access (e.g. for software updates) — so they belong in **private subnets** routed through a NAT Gateway.
> - The network should follow AWS best practices for a production-style environment: correct CIDR planning, one route table per tier, and no unnecessary public exposure.
>
> **Goal:** Design and build the underlying VPC network that satisfies these requirements.
>
> 1. VPC created sitting in region: Singapore (ap-southeast-1) with IPV4 CIDR 10.0.0.0/16.
>    <img width="725" height="712" alt="image" src="https://github.com/user-attachments/assets/7a34fa0c-1782-4168-b37b-b919e523a91f" />

