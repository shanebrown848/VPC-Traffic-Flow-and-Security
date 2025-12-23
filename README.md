# VPC Traffic Flow and Security

**Project Link:** https://learn.nextwork.org/projects/aws-networks-security  
**Author:** Shane Brown  

---

## Overview

This project focuses on understanding how traffic flows within an Amazon Virtual Private Cloud (VPC) and how AWS networking security controls are applied. The goal is to learn how routing, security groups, and network ACLs work together to protect cloud resources while allowing required access.

This project builds directly on foundational VPC knowledge and introduces real-world security and traffic control concepts used in production AWS environments. :contentReference[oaicite:0]{index=0}

---

## What I built

I worked with multiple VPC components to control traffic flow and enforce security, including:

- Route tables to control how traffic moves within and outside the VPC  
- Internet Gateways to enable external connectivity  
- Security Groups to manage instance-level firewall rules  
- Network ACLs to enforce subnet-level traffic filtering  
- Multi-region VPC resources to simulate global infrastructure  

This setup mirrors how cloud teams design secure and scalable networks in AWS. :contentReference[oaicite:1]{index=1}

---

## Key concepts learned

- How route tables determine traffic paths  
- The difference between route destinations and targets  
- How security groups act as stateful, instance-level firewalls  
- How network ACLs act as stateless, subnet-level firewalls  
- Differences between inbound and outbound rules  
- Default vs custom network ACL behavior  
- How AWS tracks resources across regions using EC2 Global View :contentReference[oaicite:2]{index=2}

---

## Security groups vs Network ACLs

- **Security Groups**
  - Apply to individual instances  
  - Stateful (responses are automatically allowed)  
  - Allow rules only  

- **Network ACLs**
  - Apply to entire subnets  
  - Stateless (inbound and outbound rules must be defined)  
  - Support both allow and deny rules  

Together, these layers provide defense-in-depth for AWS networking security. :contentReference[oaicite:3]{index=3}

---

## Why this project matters

Traffic flow and network security are critical for protecting cloud infrastructure. Understanding how AWS enforces routing and security rules is essential for roles in cloud engineering, DevOps, and cloud security.

This project demonstrates how cloud networks are intentionally designed, monitored, and secured rather than left open by default. :contentReference[oaicite:4]{index=4}

---

## Documentation

📄 **Full project documentation:**  
[documentation.md](./documentation.md)

This file includes detailed explanations, reflections, diagrams, and examples from completing the project.

---

## Credits

Built as part of the **NextWork** AWS networking learning series.
