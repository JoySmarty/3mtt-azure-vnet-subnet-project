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