# 3mtt-azure-vnet-subnet-project

# Azure Virtual Network Configuration Project

## Overview

This project demonstrates the design and implementation of a secure and scalable Azure Virtual Network (VNet) environment. It focuses on cloud networking fundamentals such as IP addressing, subnet segmentation, and network security using Network Security Groups (NSGs).

The goal is to simulate a basic three-tier architecture using Azure resources while enforcing proper traffic isolation and security boundaries.

---

## Project Objectives

- Understand and implement CIDR-based IP addressing
- Design a Virtual Network with multiple isolated subnets
- Configure Network Security Groups (NSGs) to control traffic flow
- Differentiate between public and private IP usage
- Deploy and connect virtual machines within a secure VNet
- Validate internal communication between resources

---

## Architecture

The environment consists of:

- A Virtual Network (VNet)
- Three subnets:
  - Web Subnet
  - Application Subnet
  - Database Subnet (reserved/optional)
- Network Security Groups (NSGs) controlling traffic per subnet
- Virtual Machines deployed for testing connectivity

---

## Key Features

- Secure network segmentation using subnets
- Controlled traffic flow using NSGs
- Private communication between internal resources
- Public access restricted to only the web layer
- Internal service communication validated using Azure VMs

---

## Tools & Technologies

- Microsoft Azure
- Azure Virtual Network (VNet)
- Azure Virtual Machines
- Network Security Groups (NSGs)
- SSH (Linux VM access)
- Remote Desktop (Windows VM access)

---

## Connectivity Validation

- Verified internal communication using ICMP (ping)
- Confirmed subnet-to-subnet routing
- Tested NSG rules for traffic control
- Validated secure VM access through Azure

---

## Repository Structure
3mtt-azure-vnet-subnet-project/
│
├── README.md
├── ARCHITECTURE.md
├── deployment.md
├── configuration.md
├── troubleshooting.md
├── monitoring-scaling.md
│
├── diagrams/
│
│
└── screenshots/

---

## Author

JoySmarty

---

## Status

Project completed successfully and deployed on Microsoft Azure.