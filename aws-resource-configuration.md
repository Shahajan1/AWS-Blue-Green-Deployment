# AWS Resource Configuration

## Region

`us-east-1` (US East — N. Virginia)

## Networking

| Resource | Configuration |
|---|---|
| VPC | `BG-VPC`, `10.0.0.0/16` |
| Public subnet 1 | `Public-1`, `10.0.1.0/24`, `us-east-1a` |
| Public subnet 2 | `Public-2`, `10.0.2.0/24`, `us-east-1b` |
| Internet Gateway | `BG-IGW`, attached to `BG-VPC` |
| Route table | `BG-RT`, route `0.0.0.0/0` to `BG-IGW` |
| Associations | `Public-1` and `Public-2` |

Auto-assign public IPv4 is enabled on both subnets in the lab configuration.

## Compute

| Instance | AMI | Type | Subnet | Web content |
|---|---|---|---|---|
| `Blue-Server` | Amazon Linux 2023 | `t3.micro` | `Public-1` | `VERSION 1 (BLUE)` |
| `Green-Server` | Amazon Linux 2023 | `t3.micro` | `Public-2` | `VERSION 2 (GREEN)` |

Both instances use Apache HTTP Server installed through EC2 User Data.

## Load balancing

| Resource | Configuration |
|---|---|
| ALB | `BlueGreen-ALB` |
| Scheme | Internet-facing |
| Listener | HTTP:80 |
| Blue target group | `TG-Blue`, instance target, HTTP:80 |
| Green target group | `TG-Green`, instance target, HTTP:80 |
| Initial action | Blue 100 / Green 0 |
| Canary-style action | Blue 80 / Green 20 |
| Final action | Blue 0 / Green 100 |

## Security note

The lab used `Web-SG` for the load balancer and EC2 instances, with an HTTP inbound rule from `0.0.0.0/0`. A production design should separate ALB and instance security groups and restrict instance ingress to the ALB security group.
