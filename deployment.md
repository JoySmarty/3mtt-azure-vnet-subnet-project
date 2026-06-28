# Azure VNet Deployment Guide

## Overview

This document outlines the step-by-step process used to deploy a Virtual Network (VNet), subnets, virtual machines, and Network Security Groups (NSGs) on Microsoft Azure.

---

## Step 1: Create Resource Group

A resource group was created to hold all related resources.

- Name: rg-webapp-lab
- Region: East US

---

## Step 2: Create Virtual Network (VNet)

The Virtual Network was created with the following configuration:

- Name: vnet-production
- Address Space: 10.0.0.0/16
- Region: East US

---

## Step 3: Create Subnets

The VNet was divided into three subnets:

### Web Subnet
- Name: web-subnet
- CIDR: 10.0.1.0/24

### Application Subnet
- Name: app-subnet
- CIDR: 10.0.2.0/24

### Database Subnet
- Name: db-subnet
- CIDR: 10.0.3.0/24

---

## Step 4: Create Network Security Groups (NSGs)

NSGs were created to control traffic flow:

- nsg-web
- nsg-app
- nsg-db

Each NSG was associated with its corresponding subnet.

---

## Step 5: Configure Inbound Security Rules

### Example (Web NSG Rule)

- Source: Any
- Source Port: *
- Destination: Any
- Service: Custom
- Destination Port: 8080
- Protocol: TCP
- Action: Allow
- Priority: 100
- Name: allow-web-traffic

### Example (App NSG Rule)

- Source: 10.0.1.0/24
- Destination: 10.0.2.0/24
- Port: 8080
- Protocol: TCP
- Action: Allow

### Example (DB NSG Rule - MySQL selected)

- Source: 10.0.2.0/24
- Destination: 10.0.3.0/24
- Service: MySQL
- Action: Allow

---

## Step 6: Deploy Virtual Machines

### Web VM
- OS: Ubuntu Server
- Subnet: web-subnet
- Public IP: Enabled

### App VM
- OS: Ubuntu Server
- Subnet: app-subnet
- Public IP: Disabled

---

## Step 7: Connectivity Testing

The following tests were performed:

- Ping test between Web VM and App VM using private IP
- SSH connection from Web VM to App VM (optional test)
- Verification of subnet isolation
- Confirmation of NSG rule enforcement

---

## Step 8: Validation

The deployment was validated by:

- Successful VM provisioning
- Correct subnet allocation
- Successful ICMP communication between VMs
- Proper restriction of unauthorized traffic