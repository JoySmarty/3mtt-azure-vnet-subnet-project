# Azure Network Configuration

## Overview

This document explains the detailed configuration of the Virtual Network (VNet), subnets, IP addressing, and Network Security Groups (NSGs) used in this project.

---

## Virtual Network Configuration

- Name: vnet-production
- Address Space: 10.0.0.0/16
- Region: East US

The VNet provides a private network boundary for all resources in this architecture.

---

## Subnet Configuration

### Web Subnet

- Name: web-subnet
- CIDR Block: 10.0.1.0/24
- Purpose: Hosts the web server (public-facing layer)

---

### Application Subnet

- Name: app-subnet
- CIDR Block: 10.0.2.0/24
- Purpose: Handles internal application logic

---

### Database Subnet

- Name: db-subnet
- CIDR Block: 10.0.3.0/24
- Purpose: Reserved for database services (restricted access)

---

## IP Addressing Strategy

The network uses private IP addressing based on RFC 1918:

- 10.0.0.0/16 → Entire Virtual Network range
- 10.0.1.0/24 → Web tier
- 10.0.2.0/24 → Application tier
- 10.0.3.0/24 → Database tier

Public IPs are only assigned to the web server for external access.

---

## Network Security Groups (NSGs)

NSGs are used to control inbound and outbound traffic at the subnet level.

---

### nsg-web

Controls access to the web subnet.

Inbound Rules:
- Allow TCP 8080 from Any (web traffic)
- Allow SSH (22) for administration

---

### nsg-app

Controls access to the application subnet.

Inbound Rules:
- Allow traffic from web-subnet (10.0.1.0/24)
- Port: 8080
- Protocol: TCP
- Action: Allow

All other inbound traffic is denied by default.

---

### nsg-db

Controls access to the database subnet.

Inbound Rules:
- Service: MySQL
- Port: 3306
- Source: app-subnet (10.0.2.0/24)
- Action: Allow

All other traffic is blocked.

---

## Security Model

The architecture follows a layered security approach:

Internet → Web Tier → Application Tier → Database Tier

Each layer only communicates with the next allowed layer, reducing attack surface and improving security.

---

## Summary

This configuration ensures:

- Proper network segmentation
- Controlled traffic flow using NSGs
- Secure internal communication
- Restricted database access