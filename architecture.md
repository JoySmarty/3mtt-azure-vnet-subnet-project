# Azure Virtual Network Architecture

## Overview

This project demonstrates a secure and scalable cloud network architecture using Microsoft Azure Virtual Network (VNet). The design follows a basic three-tier networking model consisting of web, application, and database layers separated using subnets and secured using Network Security Groups (NSGs).

---

## Architecture Design

The architecture is built around a single Virtual Network:

- **Virtual Network Name:** vnet-production  
- **Address Space:** 10.0.0.0/16  
- **Region:** East US  

This VNet provides a private network boundary for all deployed resources.

---

## Subnet Design

The Virtual Network is divided into multiple subnets to isolate workloads:

| Subnet | CIDR Block | Purpose |
|--------|------------|--------|
| web-subnet | 10.0.1.0/24 | Hosts the web server (public-facing) |
| app-subnet | 10.0.2.0/24 | Hosts application server (internal logic layer) |
| db-subnet | 10.0.3.0/24 | Reserved for database layer (no public access) |

---

## Virtual Machines

Two virtual machines were deployed for testing and validation:

| VM | Subnet | Public IP | Role |
|----|--------|-----------|------|
| web-vm | web-subnet | Yes | Handles external traffic |
| app-vm | app-subnet | No | Processes internal application logic |

---

## Network Security Groups (NSGs)

Security is enforced using NSGs at the subnet level.

### nsg-web
- Allows inbound HTTP/application traffic (port 8080)
- Allows SSH (port 22) for administration

### nsg-app
- Allows traffic only from web-subnet (10.0.1.0/24)
- Restricts direct internet access

### nsg-db
- Allows MySQL traffic (port 3306)
- Only allows traffic from app-subnet (10.0.2.0/24)

---

## Traffic Flow

The communication flow is designed as:

Internet → Web VM → App VM → (Database layer reserved)

This ensures controlled and secure data flow between tiers.


---

## Connectivity Validation

The following tests were performed:

- Successful deployment of Virtual Network and subnets
- Virtual machines deployed in correct subnets
- Internal communication verified using ICMP (ping)
- NSG rules confirmed correct traffic control
- App VM successfully reachable from Web VM via private IP

---

## Summary

This architecture demonstrates core cloud networking principles including:

- Network segmentation using subnets
- Traffic control using NSGs
- Private and public IP separation
- Secure internal communication within a VNet

## Virtual Network Peering

To demonstrate inter-network connectivity, a second Virtual Network named **vnet-secondary** was created with the address space **10.1.0.0/16**. This network was connected to the primary Virtual Network (**vnet-production**) using Azure Virtual Network Peering.

The peering configuration enables private communication between both VNets over the Microsoft Azure backbone network without requiring a VPN Gateway or public internet connectivity.

### Peering Configuration

| Source VNet     | Destination VNet | Status    |
| --------------- | ---------------- | --------- |
| vnet-production | vnet-secondary   | Connected |
| vnet-secondary  | vnet-production  | Connected |

The following peering options were configured:

* Allow virtual network access: Enabled
* Allow forwarded traffic: Disabled
* Allow gateway transit: Disabled
* Use remote gateway: Disabled

This configuration demonstrates how Azure Virtual Network Peering can be used to securely connect separate virtual networks while maintaining low-latency communication.

---

## Route Table (User Defined Route)

A custom Azure Route Table was created and associated with the application subnet to demonstrate traffic management using User Defined Routes (UDRs).

### Route Table Configuration

| Property          | Value      |
| ----------------- | ---------- |
| Route Table Name  | vnet-rt    |
| Region            | East US    |
| Associated Subnet | app-subnet |

A custom route was added to define routing toward the secondary Virtual Network.

| Destination Prefix | Next Hop        |
| ------------------ | --------------- |
| 10.1.0.0/16        | Virtual Network |

Although Azure automatically provides system routes, User Defined Routes allow administrators to customize traffic flow between network segments or direct traffic through security appliances such as Azure Firewall or Network Virtual Appliances (NVAs).

Implementing a Route Table demonstrates an understanding of advanced Azure networking concepts and provides greater control over network routing behavior.
                    Internet
                        │
                 Public Web VM
                        │
                vnet-production
                10.0.0.0/16
                        │
     ┌──────────┬──────────────┬──────────┐
     │          │              │
 web-subnet  app-subnet    db-subnet
10.0.1.0/24 10.0.2.0/24 10.0.3.0/24
     │          │
  Web VM     App VM
     │
     └─────────────── VNet Peering ───────────────┐
                                                  │
                                      vnet-secondary
                                        10.1.0.0/16
                                              │
                                      secondary-subnet
                                        10.1.1.0/24

## Route Table (Advanced Concept)

A custom route table can be used to control traffic flow between subnets.

For example:
- Force traffic from Web → App through a firewall
- Block direct subnet-to-subnet communication
- Control outbound internet traffic



## VNet Peering (Advanced Concept)

VNet Peering allows communication between two Azure Virtual Networks in the same or different regions.

This was not implemented in this project, but it is commonly used in enterprise architectures to connect:
- Separate environments
- Different regions
- Microservices architectures
VNet peering was not implemented in this project because all resources were deployed within a single Virtual Network.

In real-world scenarios, VNet peering is used to connect multiple Virtual Networks across regions or environments (e.g., development, staging, production).

