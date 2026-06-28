# Azure Networking Troubleshooting

## Overview

During the deployment of the Virtual Network and virtual machines, several issues were encountered and resolved. This document outlines the problems, root causes, and solutions applied.

---

## Issue 1: Virtual Machine Creation Quota Error

### Problem
Deployment of resources failed due to quota limitations in the selected region.

### Error Message
- "Operation cannot be completed without additional quota"
- Required VM limit exceeded in the selected region

### Solution
- Changed region to **East US**
- Successfully deployed resources after switching region

---

## Issue 2: SSH Connection Timeout

### Problem
Unable to connect to the web virtual machine using SSH.

### Error Message
- Connection timed out on port 22

### Cause
- Public IP or NSG rules were not properly configured at the time of connection attempt

### Solution
- Verified that the VM had a public IP assigned
- Confirmed NSG allowed inbound SSH (port 22)
- Retried connection after configuration completion

---

## Issue 3: Internal VM Connectivity

### Problem
Testing communication between virtual machines using private IP addresses.

### Observation
Ping initially appeared to hang but later succeeded.

### Result
- 100% packet delivery confirmed
- Successful communication between web and app VMs

### Conclusion
This confirmed that:
- VNet routing is working correctly
- Subnet configuration is valid
- NSG rules are correctly applied

---

## Issue 4: SSH Between VMs (Public Key Issue)

### Problem
SSH from web-vm to app-vm failed with:
- Permission denied (publickey)

### Cause
- SSH key authentication was not configured between virtual machines
- Key file was not shared or installed on target VM

### Resolution
- Verified that SSH between VMs is not required for basic connectivity testing
- Used ICMP (ping) for validation instead

---

## Final Outcome

All networking components were successfully deployed and validated:

- Virtual Network created successfully
- Subnets correctly configured
- NSGs properly applied
- Virtual machines deployed and running
- Internal communication verified using ICMP