# 🚀 AWS Blue-Green Deployment with Canary Traffic Shifting

A hands-on AWS DevOps project demonstrating how to deploy, manage, and switch between two application environments using Blue-Green Deployment and Canary Traffic Shifting strategies.

The architecture uses a custom VPC, two public subnets across separate Availability Zones, Amazon EC2 instances, Application Load Balancer, and separate Target Groups to manage application traffic between Version 1 (Blue) and Version 2 (Green).

## 🏗️ Architecture Overview

The infrastructure is designed to maintain two independent application environments. The Blue environment represents the existing application version, while the Green environment hosts the updated version.

An Application Load Balancer acts as the single entry point and distributes incoming HTTP traffic between both environments using weighted target group forwarding.

**Architecture Components:**

- Custom Amazon VPC
- Two Public Subnets
- Two Availability Zones
- Internet Gateway
- Route Table
- Amazon EC2 Instances
- Apache HTTP Server
- Security Groups
- Blue Target Group
- Green Target Group
- Application Load Balancer
- Weighted Traffic Routing

> 📌 Add your AWS Blue-Green architecture diagram or implementation screenshot here.

## 🛠️ AWS Services & Technologies

| **Service / Technology** | **Implementation** |
|---|---|
| Amazon VPC | Custom virtual network (`BG-VPC`) |
| Public Subnets | Two public subnets across separate AZs |
| Availability Zones | `us-east-1a` and `us-east-1b` |
| Internet Gateway | Internet connectivity for public subnets |
| Route Table | Routes internet traffic through the IGW |
| Amazon EC2 | Two Amazon Linux 2023 instances |
| Apache HTTP Server | Hosts Blue and Green application versions |
| Security Groups | Controls HTTP traffic |
| Target Groups | Separate backend registration for each environment |
| Application Load Balancer | Single entry point for application traffic |
| Weighted Forwarding | 80/20 Canary-style traffic distribution |

## ⚙️ Project Implementation

### 1. VPC & Networking

- Created a custom VPC named `BG-VPC` with CIDR `10.0.0.0/16`.
- Created two public subnets across separate Availability Zones.
- Configured `Public-1` with CIDR `10.0.1.0/24`.
- Configured `Public-2` with CIDR `10.0.2.0/24`.
- Enabled automatic public IPv4 address assignment.
- Created and attached an Internet Gateway named `BG-IGW`.
- Configured the `BG-RT` route table with an internet route.
- Associated both public subnets with the route table.

### 2. Blue Environment – Version 1

- Launched an EC2 instance named `Blue-Server`.
- Selected Amazon Linux 2023 with instance type `t3.micro`.
- Deployed the instance in `Public-1`.
- Configured the `Web-SG` security group to allow HTTP traffic on port 80.
- Used EC2 User Data to install and configure Apache HTTP Server.
- Created a blue-themed webpage displaying `VERSION 1 (BLUE)`.

### 3. Green Environment – Version 2

- Launched a second EC2 instance named `Green-Server`.
- Deployed the instance in `Public-2`.
- Reused the existing `Web-SG` security group.
- Configured Apache HTTP Server using EC2 User Data.
- Created a green-themed webpage displaying `VERSION 2 (GREEN)`.

### 4. Target Groups Configuration

Created two separate Target Groups to manage the application environments.

**Blue Target Group:**
- Name: `TG-Blue`
- Protocol: HTTP
- Port: 80
- Registered target: `Blue-Server`

**Green Target Group:**
- Name: `TG-Green`
- Protocol: HTTP
- Port: 80
- Registered target: `Green-Server`

Both target groups were configured to perform health checks on their registered EC2 instances.

### 5. Application Load Balancer

- Created an internet-facing Application Load Balancer named `BlueGreen-ALB`.
- Selected `BG-VPC` as the network.
- Mapped both public subnets across two Availability Zones.
- Configured an HTTP listener on port 80.
- Configured `TG-Blue` as the initial default forwarding target.
- Used the ALB DNS name as the application's public entry point.

### 6. Canary Deployment & Traffic Shifting

Implemented weighted forwarding through the ALB listener to gradually introduce the Green environment.

**Initial Deployment – 100% Blue**

All application traffic was directed to `TG-Blue`, serving Version 1.

**Canary Deployment – 80/20 Traffic Split**

Modified the ALB listener's forwarding configuration:

| Target Group | Traffic Weight |
|---|---:|
| `TG-Blue` | 80% |
| `TG-Green` | 20% |

This configuration allows a portion of incoming traffic to reach the updated application version while the existing Blue environment continues serving most requests.

**Final Cutover – 100% Green**

Updated the forwarding weights:

| Target Group | Traffic Weight |
|---|---:|
| `TG-Blue` | 0% |
| `TG-Green` | 100% |

The ALB was configured to route new eligible requests entirely to the Green environment.

## 🔄 Architecture Workflow

1. Users access the application through the Application Load Balancer DNS endpoint.
2. The ALB receives incoming HTTP requests on port 80.
3. Initially, 100% of traffic is forwarded to `TG-Blue`.
4. The Blue target group forwards requests to `Blue-Server`, serving Version 1.
5. During the Canary deployment stage, traffic is distributed between Blue (80%) and Green (20%).
6. After validating the updated application, traffic is shifted to 100% Green.
7. The Green target group forwards requests to `Green-Server`, serving Version 2.

## 📊 Deployment Traffic Strategy

| Deployment Stage | Blue Environment | Green Environment |
|---|---:|---:|
| Initial Deployment | 100% | 0% |
| Canary Deployment | 80% | 20% |
| Final Cutover | 0% | 100% |

**Deployment Strategy:** Weighted Target Group Forwarding using an Application Load Balancer.

## 🔐 Security & Networking

- Created a dedicated VPC for the deployment environment.
- Configured public subnets across two Availability Zones.
- Used an Internet Gateway for internet connectivity.
- Configured route tables for public subnet routing.
- Used Security Groups to control inbound HTTP traffic.
- Registered EC2 instances behind Application Load Balancer target groups.

**Security improvement for production:** This lab uses a shared security group for the ALB and EC2 instances. A production architecture should use separate security groups, restricting backend HTTP access to traffic originating from the ALB security group. HTTPS and private application subnets should also be considered.

## 📚 Key Learnings

Through this project, I gained practical experience in:

- AWS VPC and subnet configuration.
- Public networking and Internet Gateway setup.
- Route tables and Availability Zones.
- EC2 instance deployment and User Data automation.
- Apache HTTP Server installation and configuration.
- Target Group registration and health checks.
- Application Load Balancer configuration.
- Weighted target group forwarding.
- Blue-Green Deployment concepts.
- Canary-style traffic shifting.
- Application version cutover and traffic management.
- AWS networking and deployment troubleshooting.

## 📖 Project Documentation

For detailed implementation steps, AWS configurations, deployment procedures, and troubleshooting, refer to:

[**AWS Blue-Green Deployment – Detailed Project Documentation**](deployment-and-testing.md)

Additional configuration details:

- [AWS Resource Configuration](aws-resource-configuration.md)
- [Project Overview](project-overview.md)
- [Troubleshooting Guide](troubleshooting.md)

## 👨‍💻 Author

**Shahajan Shaikh**

DevOps & AWS Cloud Enthusiast

- GitHub: [Shahajan1](https://github.com/Shahajan1)
- LinkedIn: [Shahajan Shaikh](https://www.linkedin.com/in/shahajanshaikh/)

---

⭐ If you find this project useful, feel free to explore the repository!
