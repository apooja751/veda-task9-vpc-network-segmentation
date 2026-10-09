# Task 9: VPC Network Segmentation

## Internship
**Organization:** VEDA Technology  
**Task:** Task 9 — VPC Network Segmentation  
**Cloud Platform:** Amazon Web Services (AWS)

## Objective
To design and configure a custom Amazon VPC with public and private subnets, route tables, an Internet Gateway, and security groups to demonstrate network segmentation and controlled access to cloud resources.

## Architecture Overview
The solution uses a custom VPC containing:
- A public subnet hosting an EC2 web server.
- A private subnet intended for internal resources such as a database.
- A public route table with a default route to an Internet Gateway.
- A private route table containing only the local VPC route.
- Separate security groups for the web server and database.

## AWS Resources

| Resource | Name / Configuration |
|---|---|
| VPC | `veda-task9-vpc` |
| VPC IPv4 CIDR | `10.0.0.0/16` |
| Public subnet | `veda-task9-public-subnet` |
| Public subnet CIDR | `10.0.1.0/24` |
| Private subnet | `veda-task9-private-subnet` |
| Private subnet CIDR | `10.0.2.0/24` |
| Internet Gateway | `veda-task9-igw` |
| Public route table | `veda-task9-public-rt` |
| Private route table | `veda-task9-private-rt` |
| Web security group | `veda-task9-web-sg` |
| Database security group | `veda-task9-db-sg` |
| EC2 web server | `veda-task9-web-server` |
| Operating system | Amazon Linux 2023 |
| Web server | Nginx |

## Network Configuration

### Public subnet
- Associated with the public route table.
- The route table contains `10.0.0.0/16 → local` and `0.0.0.0/0 → Internet Gateway`.
- Public IPv4 addressing is enabled for the web server.

### Private subnet
- Associated with the private route table.
- The route table contains `10.0.0.0/16 → local`.
- No default Internet Gateway route is configured in the private route table.

**Note:** A private subnet without a default Internet Gateway route does not automatically guarantee complete isolation. Additional routing, security group, and network ACL settings determine effective connectivity.

## Security Groups

### Web server security group
| Type | Port | Source |
|---|---:|---|
| HTTP | 80 | `0.0.0.0/0` |
| SSH | 22 | Restricted to the administrator's public IP `/32` |

### Database security group
| Type | Port | Source |
|---|---:|---|
| MySQL/Aurora | 3306 | `veda-task9-web-sg` security group |

This configuration permits database traffic from resources associated with the web security group rather than directly exposing the database port to the internet.

## Web Server Setup and Testing

1. Launched an EC2 instance named `veda-task9-web-server` in the public subnet.
2. Connected to the instance using SSH and the assigned key pair.
3. Installed Nginx on Amazon Linux 2023.
4. Started the Nginx service.
5. Created a custom HTML page describing VPC Network Segmentation.
6. Verified the page locally using `curl http://localhost`.
7. Opened the instance's public IPv4 address in a browser and confirmed that the custom web page loaded successfully.

## Verification Results
- VPC and subnet configuration: verified in the AWS console.
- Public and private route tables and subnet associations: verified.
- Internet Gateway attachment and public route: verified.
- EC2 instance status checks: passed.
- Nginx service and local HTTP response: verified.
- Public web page: successfully loaded in a browser.
- Web security group inbound rules: verified.
- Database security group inbound rule: verified to reference the web security group.

**Testing limitation:** The database security rule was verified in the console, but an end-to-end database connection and traffic-isolation test were not performed as part of the documented evidence.

## Screenshots
The repository contains 21 screenshots documenting resource creation, configuration, and verification.

| Screenshot range | Evidence |
|---|---|
| `01`–`04` | VPC, subnets, and Internet Gateway |
| `05`–`09` | Route tables and subnet associations |
| `10`–`11` | Security group creation |
| `12`–`14` | EC2 launch, web page test, and network details |
| `15`–`19` | Route verification and Internet Gateway attachment |
| `20`–`21` | Web and database security group inbound rules |

## Conclusion
This task demonstrated how to segment an AWS network using a custom VPC, public and private subnets, route tables, an Internet Gateway, and separate security groups. The Nginx web server was successfully deployed in the public subnet and its custom web page was verified through a browser. The database access rule was also configured to reference the web server security group.

## Technologies Used
- Amazon VPC
- Amazon EC2
- Amazon Linux 2023
- Nginx
- AWS Security Groups
- AWS Route Tables
- Internet Gateway

