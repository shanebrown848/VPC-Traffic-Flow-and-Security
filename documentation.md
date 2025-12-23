<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# VPC Traffic Flow and Security

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-security)

**Author:** Shane Brown  
**Email:** shanebrown848@gmail.com

---

## VPC Traffic Flow and Security

![Image](http://learn.nextwork.org/encouraged_yellow_silly_yeti/uploads/aws-networks-security_92b0b0b4)

---

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC (Virtual Private Cloud) is a service that lets you create your own isolated, private network inside AWS. You control every part of your network setup, including IP address ranges, subnets, route tables, internet gateways, security groups, and more, all inside the AWS cloud.

Why is it useful?
Isolation & Security: Your resources are separated from other AWS customers, and you control who can access them.
Customization: You design your network architecture the way you need it.
Scalability: You can easily expand your network as your applications grow.
Hybrid Ready: You can connect your VPC to on-premises data centers or other cloud environments.
Real-World Production Grade: Almost every serious AWS production deployment involves VPCs.
In simple terms:
Amazon VPC gives you your own private data center in the cloud — fully managed, flexible, and highly secure.

### How I used Amazon VPC in this project

I used Amazon VPC in today's project to create isolated network environments across multiple AWS regions. I deployed new VPCs, attached internet gateways, and set up security groups using the AWS CLI and CloudShell. This allowed me to practice creating private, secure cloud networks that could support real-world applications. I also used EC2 Global View to track all my VPC resources globally, giving me full visibility into my multi region setup, just like large-scale cloud teams would do to ensure global availability, low latency, and disaster recovery.

### One thing I didn't expect in this project was...

Well for me, I didn't realize how there is all these little systems functioning together to make the VPC work and Also with me being in cybersecurity how important it is to make sure we have a good policy for inbound and outbound. Being in control and understanding how your system works gives you a great advantage, especially when you want to protect it.

### This project took me...

For me it took about 1 and 15 mins. I took my time on this project because I wanted to get all the details down. I also did my own Google and Chat GPT research so I was making sure I had a understanding of the VPC system and ecosystem.

---

## Route tables

Route tables are configuration files within a VPC that determine how network traffic is directed. Each route table contains a set of rules (called routes) that specify where traffic destined for a particular IP address range should be sent.

For example:

A route might send traffic destined for the internet (0.0.0.0/0) to an Internet Gateway.

Another route might send traffic for internal resources to a NAT Gateway or VPN connection.

Subnets are associated with route tables to control how traffic flows in and out of that subnet.

Route tables are needed to make a subnet public because they control how traffic leaves the subnet. To make a subnet public, the route table must contain a route that directs internet-bound traffic (0.0.0.0/0) to the Internet Gateway. Without this route, even if the subnet has an Internet Gateway attached to the VPC, the resources inside the subnet wouldn’t know how to reach the internet.

![Image](http://learn.nextwork.org/encouraged_yellow_silly_yeti/uploads/aws-networks-security_0a07b191)

---

## Route destination and target

Routes are defined by their destination and target, which mean:

Destination: This specifies where the traffic is going — it's an IP address range written in CIDR notation. For example:

10.0.0.0/16 = traffic going to resources inside your VPC

0.0.0.0/0 = traffic going to any destination on the internet

Target: This specifies how to get there — it's the resource that handles the traffic. For example:

Internet Gateway (igw-) → for internet traffic

NAT Gateway (nat-) → for private instances that need internet access

Peering Connection (pcx-) → for VPC-to-VPC connections

Local → for traffic inside the VPC (default)

The route in my route table that directed internet-bound traffic to my internet gateway had a destination of 0.0.0.0/0 and a target of my Internet Gateway ID (for example: igw-0c50f5d425bfd2a88).

In simple terms:
"Send all traffic destined for anywhere (the internet) through my Internet Gateway."

![Image](http://learn.nextwork.org/encouraged_yellow_silly_yeti/uploads/aws-networks-security_0a07b191)

---

## Security groups

Security groups are virtual firewalls that control inbound and outbound traffic to AWS resources like EC2 instances. They define what traffic is allowed to enter or leave your resource based on rules you create.

Inbound rules control what traffic is allowed into your instance.

Outbound rules control what traffic is allowed out from your instance.

Key points:

Security groups are stateful — if incoming traffic is allowed, the response is automatically allowed back out.

They work at the instance level, not at the subnet level.

You can attach multiple security groups to a single instance for flexible rule management.

In simple terms:
Security groups say who can talk to your resources, and who your resources can talk to.

### Inbound vs Outbound rules

Inbound rules are firewall settings that define which types of incoming traffic are allowed to reach your resource. They specify things like protocol, port range, and source IP or network.

I configured an inbound rule that allows HTTP traffic (protocol: TCP, port: 80) from any IP address (0.0.0.0/0). This means my server can accept web traffic from anyone on the internet.
In simple terms:
Inbound rules decide who’s allowed to knock on your door.

Outbound rules are firewall settings that define what kind of traffic your resource is allowed to send out. They specify the protocol, port range, and destination IP or network.

By default, my security group's outbound rule allows all outbound traffic (protocol: all, port range: all, destination: 0.0.0.0/0). This means my instance can send traffic to any destination on the internet or any network.
In simple terms:
Outbound rules decide where your resource is allowed to send traffic.

![Image](http://learn.nextwork.org/encouraged_yellow_silly_yeti/uploads/aws-networks-security_92b0b0b4)

---

## Network ACLs

Network ACLs are stateless firewalls that control inbound and outbound traffic at the subnet level within your VPC. Unlike security groups (which are instance-level), network ACLs apply to all resources in the subnet they’re associated with.

Key points:

Stateless: If you allow inbound traffic, you must also explicitly allow the outbound response.

Rules are evaluated in order — from the lowest to the highest rule number.

You can allow or deny specific traffic (security groups only allow, but NACLs allow and deny).

They’re an extra layer of security alongside security groups.
In simple terms:
Network ACLs protect entire subnets, acting like a neighborhood gate, while security groups protect individual houses.

### Security groups vs. network ACLs

The difference between a security group and a network ACL is that security groups operate at the instance level and are stateful, while network ACLs operate at the subnet level and are stateless.

Security Groups:

Apply to individual instances.

Stateful: if traffic is allowed in, the response is automatically allowed out.

Only allow rules (no deny rules).

Simpler and used most often.

Network ACLs:

Apply to entire subnets.

Stateless: both inbound and outbound rules must be explicitly configured.

Allow and deny rules.

Often used as an additional layer of security.

In simple terms:
Security groups are your personal bodyguards; NACLs are the gatekeepers for the whole neighborhood.

---

## Default vs Custom Network ACLs

### Similar to security groups, network ACLs use inbound and outbound rules

By default, a network ACL's inbound and outbound rules will allow all traffic.

The default network ACL that comes with every new VPC allows all inbound and all outbound traffic (0.0.0.0/0, all protocols, all ports). This means nothing is blocked by default until you modify the rules.
In simple terms:
The default NACL starts wide open, and you must tighten it if you want extra security.

In contrast, a custom ACL’s inbound and outbound rules are automatically set to deny all traffic by default.

When you create a new custom network ACL, it starts with no rules, which means:

Inbound traffic: Denied (unless you add allow rules)

Outbound traffic: Denied (unless you add allow rules)

In simple terms:
Custom NACLs start completely locked down. You have to open them up by adding specific allow rules.

![Image](http://learn.nextwork.org/encouraged_yellow_silly_yeti/uploads/aws-networks-security_4faeb056)

---

## Tracking VPC Resources

I created additional VPC resources: a VPC, an Internet Gateway, and a Security Group. Instead of my usual region, I used a new AWS region to simulate a multi-region deployment scenario.

Teams would use multiple regions to improve application performance by reducing latency for users in different parts of the world. It also helps increase fault tolerance and disaster recovery capabilities if one region experiences issues, services in another region can continue operating. This kind of global architecture is essential for businesses that want high availability, resilience, and better user experiences across diverse geographic locations.

EC2 Global View is a tool where you can find all your EC2 resources including instances, VPCs, subnets, security groups, internet gateways, and more, across every AWS region in your account, all in one centralized dashboard. This makes it easy to track what you’ve deployed globally.

I could even narrow down my search by filtering for specific regions, resource types, or tags to quickly locate the resources I want to review or manage.

Without EC2 Global View, you'd have to manually switch between regions one by one in the AWS Console to check on your resources which is time consuming, easy to miss things, and inefficient for global environments.

Now that I've learnt about EC2 Global View, I'd use it again to quickly get a full inventory of all my EC2 and VPC resources across every AWS region. It’s especially useful when managing multi-region architectures, cleaning up unused resources, verifying deployments, or preparing for audits. Instead of switching regions manually, I can instantly see everything from one place, saving time and reducing the risk of missing any hidden resources.

![Image](http://learn.nextwork.org/encouraged_yellow_silly_yeti/uploads/aws-networks-security_b03ea6162)

---

---
