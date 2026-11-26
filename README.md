<picture><source media="(prefers-color-scheme: dark)" srcset="https://nextwork.ai/static/nextwork-logo-paper.svg" /><img src="https://nextwork.ai/static/nextwork-logo-leather.svg" alt="NextWork" width="300" /></picture>

# VPC Peering

**Project Link:** [View Project](https://nextwork.ai/projects/fd75209e-d2a1-5bdd-90e8-bdb9ee49de16)

---

![](https://nextwork.ai/juanmhdez/uploads/aws-networks-peering_88727bef)

## Introducing Today's Project!

### ✍️ What are we here to do today?

In this project, I will learn how to set up and work with multiple Amazon VPCs at the same time, instead of just a single network. I will practice creating a VPC peering connection so that two separate VPCs can communicate securely with each other over their private IP addresses, without sending traffic over the public internet. I will also learn how to test this VPC peering by running connectivity checks between resources in each VPC, so I can prove that traffic is flowing correctly through my peering connection.

### What is Amazon VPC?

Amazon VPC is a networking service that lets me create my own logically isolated virtual network inside AWS, where I defiene things like IP address ranges, subnets, route tables and gateways for my cloud resources. It is useful because it gives me fine-grained control over how my resources connect to the internet and to each other.

### How I used Amazon VPC in this project

In today's project, I used Amazon VPC to create two separate, isolated virtual networks (each with its own CIDR range, subnets and route tables) and then connect them together using a VPC peering connection so resources could communicate privately across VPCs.

### One thing I didn't expect in this project was...

One thing I didn't expect in this project was using Elastic IPs for the EC2 instances.

### This project took me...

This project took me about 90 minutes to complete.

## In the first part of my project...

### Step 1 - Set up my VPC

In this step, I will create two new VPCs from scratch using the visual VPC resource map option so I can quickly spin up all the core networking components I need in one go.

### Step 2 - Create a Peering Connection

In this step, I will create a VPC peering connection between my two existing VPCs so they can communicate with each other over private IP addresses instead of the public internet. I will do this because the peering connection acts like a private bridge or link between the networks, allowing resources in VPC 1 and VPC 2 to send traffic directly and securely as if they were on the same network.

### Step 3 - Update Route Tables

In this step, I will update the route tables in both of my VPCs to add routes that point traffic for the other VPC's CIDR block through the VPC peering connection. I will do this because the peering link by itself does not move any packets, and each VPC's route table needs an explicit path so traffic from VPC 1 can reach VPC 2 and traffic from VPC 2 can reach VPC 1 over the new private connetion.

### Step 4 - Launch EC2 Instances

In this step, I will launch one EC2 instance into each of my two VPCs. I will do this because I need real instances on both sides of the peering connection to run connectivity tests and prove that traffic can successfully flow between VPC 1 and VPC 2 over the private peering link.

## Multi-VPC Architecture

I started my project by launching two new VPCs using the "VPC and more" creation option, which automatically set up each VPC with its own basic networking components. I created both VPCs in this step so I would have two separate, non-overlapping networks ready for VPC peering later.

The CIDR blocks for VPCs 1 and 2 are unique so that every private IP address inside each VPC is distinct. They have to be unique because VPC peering cannot be created between VPCs if their CIDR blocks overlap, and overlapping IP ranges could cause routing conflicts and connectivity issues.

### I also launched 2 EC2 instances

I didn't set up key pairs for these EC2 instances as I planned to use EC2 Instance Connect to acces Instance 1 directly throught the AWS console, rather than connecting with a downloaded private key.

![Multi-VPC Architecture](https://nextwork.ai/juanmhdez/uploads/aws-networks-peering_11111111)

## VPC Peering

A VPC peering connection is a private networking link between two VPCs that lets me route traffic between them using their private IP addresses, so resources in each VPC can talk to each other as if they were on the same network without going over the public internet.

VPCs would use peering connections to connect separate networks so resources can communicate privately over the cloud provider's backbone instead of traversing the public internet.

The difference between a Requester and an Accepter in a peering connection is that the Requester is the VPC that initiates the peering request, while the Accepter is the VPC that receives that request and must approve it for the connection to become active.

![VPC Peering](https://nextwork.ai/juanmhdez/uploads/aws-networks-peering_1cbb1b88)

## Updating route tables

After accepting a peering connection, my VPCs' route tables were updated because the peering link by itself did not tell traffic how to reach the other VPC's IP range. I added new routes that listed the peer VPC's CIDR block as the destination and the peering connection as the target so instances in each VPC could actually send and receive private traffic over the peering connection.

My VPCs' new routes have a destination of the other VPC's IPv4 CIDR block: 10.2.0.0/16 in VPC 1 and 10.1.0.0/16 in VPC 2. The routes' target was the VPC peering connetion between VPC 1 and VPC 2.

![Updating route tables](https://nextwork.ai/juanmhdez/uploads/aws-networks-peering_4a9e8014)

## In the second part of my project...

### Step 5 - Use EC2 Instance Connect

In this step, I will use EC2 Instance Connect from the AWS console to open an SSH session to my first EC2 instance and verify that I can reach it over the network. I will do this because testing the connection proves that my instance is reachable and correctly set up before I use it to test VPC peering traffic.

### Step 6 - Connect to EC2 Instance 1

In this step, I will use EC2 Instance Connect to connect to my first EC2 instance again so I can test that my updated networking setup is actually letting me reach the server.

### Step 7 - Test VPC Peering

In this step, I will use my fisrt EC2 instance to send test traffic to my second EC2 instance across the VPC peering connection and confirm that the two instances can actually talk to each other over their private IPs.

## Troubleshooting Instance Connect

Next, I used EC2 Instance Connect to open an SSH session to my EC2 instance without having to manage my own SSH keys. I did this so I could quickly verify that my Elastic IP and networking setup were working correctly and confirm that I could reach the instance over its new static public address.

I was stopped from using EC2 Instance Connect as the connection kept failing, which showed that my instance's current public IP and network settings were not letting the console reach it (No public IPv4 address assigned).

![Troubleshooting Instance Connect](https://nextwork.ai/juanmhdez/uploads/fd75209e-d2a1-5bdd-90e8-bdb9ee49de16_7685490c)

## Elastic IP addresses

To resolve this error, I set up Elastic IP addresses. Elastic IP addresses are static public IPv4 addresses that stay attached to my AWS account and can be associated with an EC2 instance so it keeps the same public IP even if it is stopped, started or its underlying host changes.

Associating an Elastic IP address resolved the error because it gave my EC2 instance a public IP that EC2 Instance Connect could reliably reach from the internet.

![Elastic IP addresses](https://nextwork.ai/juanmhdez/uploads/fd75209e-d2a1-5bdd-90e8-bdb9ee49de16_45663498)

## Troubleshooting ping issues

To test VPC peering, I ran the ping command from my first EC2 instance to the private IP address of my second EC2 instance, so I could see wheter ICMP request and replies were able to travel successfully across the peering connection.

A successful ping test would validate my VPC peering connection because it proves that ICMP traffic can travel from an instance in one VPC to an instance in the other VPC and receive replies back over their private IP addresses. That round-trip success shows that the peering link is active, the route tables are correctly sending traffic through the peering connection, and the security groups and networks ACLs on both sides are allowing the traffic end to end.

I had to update my second EC2 instance's security group because its inbound rules were only allowing ICMP traffic from inside its own VPC, which blocked ping requests coming from the first instance over the peering connection. I added a new rule that allows ICMP traffic from the first VPC's IPv4 CIDR range, so Instance 2 can now receive and respond to ping messages sent from Instance 1 across the VPC peering link.

![Troubleshooting ping issues](https://nextwork.ai/juanmhdez/uploads/fd75209e-d2a1-5bdd-90e8-bdb9ee49de16_7a29d352)
