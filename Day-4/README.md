
# Day 4 - VPC (Virtual Private Cloud) - Complete Learning Guide

## What is a VPC?
A VPC (Virtual Private Cloud) is a logically isolated section of a public cloud where you can launch resources in a virtual network that you define. It gives you complete control over your virtual networking environment, including IP address ranges, subnets, route tables, and network gateways.

Think of it as your own private data center within the cloud, but without the physical hardware maintenance.

---

## Core Components of VPC

### 1. CIDR Block (IP Address Range)
- **What**: The primary IP address range for your VPC (e.g., 10.0.0.0/16)
- **Size**: Can range from /16 (65,536 IPs) to /28 (16 IPs)
- **Important**: Cannot be changed after creation without recreating the VPC
- **Best Practice**: Use private IP ranges (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16)

### 2. Subnets
- **Definition**: A segment of a VPC's IP address range where you can place groups of isolated resources
- **Types**:
  - **Public Subnet**: Has route to Internet Gateway (IGW) - for web servers, load balancers
  - **Private Subnet**: No direct internet route - for databases, application servers
  - **VPN-only Subnet**: Has route to Virtual Private Gateway only
- **Availability Zone**: Each subnet resides entirely within one AZ (for high availability, use multiple subnets in different AZs)

### 3. Route Tables
- **Function**: Contains rules (routes) that determine where network traffic is directed
- **Main Route Table**: Default table automatically associated with subnets
- **Custom Route Tables**: Created for specific traffic routing needs
- **Key Routes**:
  - Local route: Communication within VPC (automatic)
  - 0.0.0.0/0 to IGW: Internet access
  - 0.0.0.0/0 to NAT Gateway: Outbound-only internet for private subnets

### 4. Internet Gateway (IGW)
- **Purpose**: Enables communication between VPC and the public internet
- **Characteristics**:
  - Horizontally scaled, redundant, highly available
  - Attach one IGW per VPC
  - Does not impose availability risks or bandwidth bottlenecks
- **Requirement**: Must be attached to VPC, and route table must point to it for internet access

### 5. NAT Gateway (Network Address Translation)
- **Purpose**: Allows private subnet instances to connect to internet for updates/patches, but prevents internet from initiating connections to them
- **Comparison to NAT Instances**: Managed service (no maintenance), higher bandwidth, no single point of failure, automatic scaling
- **Placement**: Must be placed in a public subnet
- **Cost**: Hourly charge + data processing fees

### 6. Security Groups (SG)
- **Level**: Instance-level (virtual firewall)
- **Characteristics**:
  - **Stateful**: Return traffic automatically allowed regardless of rules
  - **Allow-only**: Cannot explicitly deny traffic (only allow)
  - **Associated with**: ENIs (network interfaces), not subnets
- **Rules**: Inbound and outbound rules by protocol, port, and source/destination IP or SG reference
- **Default**: Deny all inbound, allow all outbound

### 7. Network ACLs (NACLs)
- **Level**: Subnet-level firewall
- **Characteristics**:
  - **Stateless**: Must explicitly allow return traffic
  - **Allow and Deny**: Can create explicit deny rules
  - **Order matters**: Rules evaluated by rule number (lowest first)
  - **Default NACL**: Allows all inbound and outbound
- **Best Practice**: Use for blocking specific IP ranges at subnet level; use Security Groups for instance-level protection

### 8. Elastic IP Addresses (EIP)
- **Definition**: Static IPv4 addresses designed for dynamic cloud computing
- **Use Case**: Masking failure by remapping address to another instance
- **Association**: Can be attached to instances or NAT Gateways
- **Cost**: Free when attached to running instance, charged when not attached or attached to stopped instance

### 9. Elastic Network Interfaces (ENI)
- **Definition**: Virtual network card attached to instances
- **Attributes**: Primary private IP, secondary private IPs, elastic IP, MAC address, security groups
- **Use Cases**: 
  - Management network (separate NIC for admin traffic)
  - Network appliances that require multiple network interfaces
  - Dual-homed instances

### 10. VPC Peering
- **Purpose**: Connect two VPCs to route traffic using private IP addresses
- **Characteristics**:
  - No transitive peering (A→B and B→C does not mean A→C)
  - No single point of failure or bandwidth bottleneck
  - Can peer across different AWS accounts or regions
- **Requirement**: CIDR blocks must not overlap between peered VPCs

### 11. VPC Endpoints
- **Purpose**: Private connection to AWS services (S3, DynamoDB) without internet gateway, NAT, or VPN
- **Types**:
  - **Gateway Endpoints**: Free, supported for S3 and DynamoDB (modify route table)
  - **Interface Endpoints (PrivateLink)**: Use ENIs with private IPs, support most AWS services, cost per hour and per GB

### 12. VPN Connections
- **Virtual Private Gateway (VGW)**: The AWS-side VPN concentrator
- **Customer Gateway (CGW)**: Physical device or software application on your side of the VPN connection
- **Types**: 
  - Site-to-Site VPN: Connects on-premises data center to VPC
  - Client VPN: Allows remote users to connect securely

### 13. Direct Connect
- **Purpose**: Dedicated physical network connection from on-premises to AWS
- **Benefits**: Reduced network costs, increased bandwidth, consistent latency compared to internet-based VPN

---

## Architecture Patterns

### Three-Tier Architecture
```
Internet → ALB (Public Subnet) → App Servers (Private Subnet) → Database (Private Subnet)
   ↓              ↓                        ↓                           ↓
  IGW         Security Group          Security Group              Security Group
```

### High Availability Design
- Spread subnets across multiple Availability Zones (minimum 2, preferably 3)
- Use multiple NAT Gateways (one per AZ) to avoid single point of failure
- Separate public and private subnets clearly

---

## Security Defense in Depth

1. **Network Layer**: NACLs (subnet level)
2. **Transport Layer**: Security Groups (instance level)
3. **Application Layer**: Web Application Firewall (WAF), ALB
4. **Encryption**: TLS/SSL in transit, KMS at rest

---

## Best Practices

1. **IP Planning**: Reserve large enough CIDR block for future growth
2. **Subnet Sizing**: /24 per subnet is common (251 usable IPs)
3. **Tagging**: Tag VPC resources for cost allocation and management
4. **Flow Logs**: Enable VPC Flow Logs to capture IP traffic information
5. **Private Connectivity**: Use VPC Endpoints instead of public internet for AWS service access
6. **DNS**: Enable DNS hostnames and DNS resolution for easy instance naming
7. **Bastion Hosts**: Use jump boxes in public subnets to access private instances (or use Systems Manager Session Manager)
8. **Network Segmentation**: Separate production, staging, and dev environments

---

## Summary

**VPC** is your isolated network sandbox in the cloud containing these essential building blocks:

- **Subnets** carve up your IP space into public (internet-facing) and private (isolated) zones across Availability Zones
- **Route Tables** direct traffic flow, determining what can reach the internet, other VPCs, or on-premises networks
- **Internet Gateway** provides the doorway to the public internet for public subnets
- **NAT Gateway** provides secure outbound-only internet for private resources
- **Security Groups** act as stateful instance firewalls (allow-only, evaluate all rules)
- **NACLs** act as stateless subnet firewalls (allow/deny, process rules in order)
- **VPC Peering** connects VPCs privately without internet traversal
- **VPC Endpoints** keep AWS service traffic internal to the AWS network
- **VPN/Direct Connect** bridges your on-premises data center to the cloud

**Key Takeaway**: VPC allows you to create a secure, scalable, and highly available network architecture that mirrors traditional on-premises networks while leveraging cloud flexibility. The separation of public and private subnets, combined with layered security (Security Groups + NACLs), creates a robust defense-in-depth strategy for cloud workloads.

**Remember**: Security Groups are stateful and permissive (allow only), while NACLs are stateless and can explicitly deny. Always place databases and sensitive workloads in private subnets, and never store credentials or sensitive data in public-facing instances.

---

*Last Updated: 2024*  
*Author: SalvatoreOps*  
*License: Public Domain / Free to share*

---