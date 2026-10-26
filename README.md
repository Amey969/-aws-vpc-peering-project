# -aws-vpc-peering-project
    AWS VPC Peering Project using Amazon VPC, EC2, Route Tables, and Security Groups.
# AWS VPC Peering Project

A beginner\-friendly AWS networking project demonstrating how to connect
two separate Amazon Virtual Private Clouds \(VPCs\) using **VPC Peering**
and test private communication between two EC2 instances\.

## Project Overview

This project creates two VPCs in the **US East \(N\. Virginia\) —**
**`us-east-1`** region\. Each VPC contains a subnet and an EC2 instance\. A
VPC peering connection allows the instances to communicate using their
**private IPv4 addresses**, once the required routes and security rules
are configured\.

## Architecture

```text
VPC-A (10.0.0.0/16)                     VPC-B (10.1.0.0/16)
└── Subnet-A (10.0.1.0/24)              └── Subnet-B (10.1.1.0/24)
    └── EC2-A                               └── EC2-B
          |                                      |
          └──────── VPC Peering Connection ──────┘
```

> The CIDR ranges above reflect the project configuration shown in the
> screenshots. Confirm the exact VPC CIDRs in the AWS console if you
> change the configuration.

## AWS Services and Concepts Used

- **Amazon VPC** — created two isolated virtual networks\.
- **Subnets** — created a subnet in each VPC\.
- **Amazon EC2** — launched two Amazon Linux instances, EC2\-A and
  EC2\-B\.
- **VPC Peering** — connected VPC\-A and VPC\-B\.
- **Route Tables** — added routes so traffic destined for the other
  VPC’s CIDR uses the peering connection\.
- **Security Groups** — configured permitted traffic, including ICMP
  for ping testing\.
- **Network ACLs** — reviewed the network ACLs associated with the
  subnets\.
- **SSH / EC2 Instance Connect** — accessed the instances to run
  networking commands\.

## Configuration Summary

---

Resource                            Configuration

---

AWS Region                          US East \(N\. Virginia\) —
`us-east-1`

VPC\-A                               `10.0.0.0/16`

Subnet\-A                            `10.0.1.0/24`

VPC\-B                               `10.1.0.0/16`

Subnet\-B                            `10.1.1.0/24`

EC2 instances                       EC2\-A and EC2\-B

Operating system                    Amazon Linux 2023

## Connectivity test                   `ping` to the other instance’s private IPv4 address

## Implementation Steps

1. Created **VPC\-A** and **VPC\-B** with non\-overlapping IPv4 CIDR
   ranges\.
2. Created **Subnet\-A** in VPC\-A and **Subnet\-B** in VPC\-B\.
3. Created and attached an internet gateway to each VPC for the
   intended internet access configuration\.
4. Created route tables and associated them with the appropriate
   subnets\.
5. Launched EC2\-A in Subnet\-A and EC2\-B in Subnet\-B\.
6. Created a VPC peering connection between VPC\-A and VPC\-B and
   accepted the connection\.
7. Added routes in both VPC route tables:
  - In VPC\-A’s route table, route the VPC\-B CIDR \(`10.1.0.0/16`\)
    through the peering connection\.
  - In VPC\-B’s route table, route the VPC\-A CIDR \(`10.0.0.0/16`\)
    through the peering connection\.
8. Updated the security groups to permit the required traffic from the
   other VPC’s private CIDR\. ICMP must be allowed for ping tests\.
9. Connected to an EC2 instance and tested connectivity using the
   **other instance’s private IPv4 address**\.

## Connectivity Test

From EC2\-A, run:

```bash
ping <EC2-B-private-IP>
```

From EC2\-B, run:

```bash
ping <EC2-A-private-IP>
```

Replace each placeholder with the destination instance’s **private IPv4**
**address** from the EC2 console\.

A successful test shows replies similar to:

```text
64 bytes from <destination-private-IP>: icmp_seq=1 ttl=... time=... ms
```

Stop the test with `Ctrl + C`\.

### If ping does not work

Check the following:

- The peering connection state is **Active**\.
- Both VPCs have non\-overlapping CIDR ranges\.
- Each route table has a route to the other VPC’s CIDR through the
  peering connection\.
- The EC2 security group allows inbound ICMP from the other VPC’s
  CIDR\.
- Network ACL inbound and outbound rules allow the relevant ICMP
  traffic\.
- You are pinging the destination’s **private IP**, not its public IP\.

## Screenshots

Create a folder named `screenshots` in this repository and upload your
AWS console screenshots\. Suggested filenames:

```text
screenshots/
├── 01-vpcs.png
├── 02-subnets.png
├── 03-route-tables.png
├── 04-internet-gateways.png
├── 05-security-groups.png
├── 06-network-acls.png
├── 07-ec2-instances.png
└── 08-ping-test.png
```

After uploading the images, you can display them in this README by
adding Markdown image references, for example:

```markdown
![VPC configuration](screenshots/01-vpcs.png)
```

## What I Learned

- Creating and configuring VPCs and subnets in AWS\.
- Understanding IPv4 CIDR ranges and private IP addressing\.
- Connecting two VPCs with VPC Peering\.
- Configuring route tables for communication between networks\.
- Using security groups and network ACLs to control traffic\.
- Accessing Amazon Linux EC2 instances and troubleshooting
  connectivity with `ping`\.

## Important Notes

- VPC Peering is **not transitive**\. A peering connection between two
  VPCs does not automatically provide access to a third VPC\.
- VPC Peering does not merge the VPCs; they remain separate networks\.
- The routes and security rules must be configured on both sides for
  communication to work\.
- This is a learning project\. Review AWS charges and delete unused
  resources when you finish\. EC2 instances and other resources may
  incur charges while they are running\.

## Author

**Amey Kundekar**
Beginner AWS Networking Project — VPC Peering
