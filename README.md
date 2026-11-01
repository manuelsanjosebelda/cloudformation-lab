# ATU CloudFormation Lab

## 1. Project Overview

This individual project explores Infrastructure as Code (IaC) using AWS CloudFormation as part of the Postgraduate Diploma in Computing in DevOps at Atlantic Technological University (ATU).

The project focuses on two practical implementations:

- Static website hosting using Amazon S3.
- A three-tier cloud architecture using AWS networking, an Application Load Balancer, Amazon EC2 and Amazon RDS.

The infrastructure is defined using CloudFormation YAML templates and deployed to AWS. The project also considers security, scalability, availability and resource efficiency best practices.

## 2. Student Information

- **Name:** Manuel Sanjose
- **Student Number:** L00205501
- **Course:** Postgraduate Diploma in Computing in DevOps
- **Module:** IaC for DevOps Pipelines

## 3. Project Structure

The repository is organised into two main directories, separating the static website implementation from the three-tier architecture.

```text
cloudformation-lab1/
├── static-website/
│     ├── template.yaml
│     └── website/
│         ├── index.html
│         └── style.css
├── three-tier-architecture/
│   ├── template-tta.yaml
└── README.md
```

- static-website/: Contains the CloudFormation template and website files.
- three-tier-architecture/: Contains the CloudFormation template defining the three-tier infrastructure.
- README.md: Provides project documentation, implementation details and testing results.

## 4. Part A — Static Website

**Status: Implemented and deployed**

The static website is hosted using Amazon S3, with its infrastructure defined through AWS CloudFormation.

The implementation includes:

- An S3 bucket configured for static website hosting.
- Public access configuration to make the website accessible through its website endpoint.
- HTML and CSS files stored in the S3 bucket.
- A CloudFormation template defining the required AWS infrastructure.

Website endpoint:

http://atu-cloudformation-l00205501.s3-website-eu-west-1.amazonaws.com

## 5. Part A — Three-Tier Architecture

**Status: Implemented, deployed and tested.**

5.1 Web Tier

The web tier uses an internet-facing Application Load Balancer (ALB).

Its main components include:

- An Application Load Balancer deployed across two public subnets.
- An HTTP listener on port 80.
- A target group forwarding incoming requests to the EC2 application server.
- Health checks to monitor the application server.

  5.2 Application Tier

The application tier consists of an Amazon EC2 instance running Apache HTTP Server.

Configuration:

- Operating system: Amazon Linux 2023.
- Instance type: t3.micro.
- Web server: Apache HTTP Server.
- IAM instance profile for AWS Systems Manager access.
- Session Manager used for remote administration without requiring SSH access.

The EC2 instance is currently deployed in a public subnet to avoid the additional cost of a NAT Gateway.

A private application subnet has also been created and reserved for future expansion.

5.3 Database Tier

The database tier uses Amazon RDS for PostgreSQL.

Configuration:

- Database engine: PostgreSQL 16.15.
- Instance class: db.t3.micro.
- Storage: 20 GB gp3.
- Storage encryption enabled.
- Public accessibility disabled.
- Multi-AZ deployment disabled.

The database is deployed within a dedicated DB subnet group containing two private subnets across different Availability Zones.

5.4 Network Architecture

The infrastructure uses a dedicated VPC with the CIDR range 10.0.0.0/16.

The network includes:

- Two public subnets across different Availability Zones.
- One private application subnet reserved for future use.
- Two private database subnets.
- An Internet Gateway.
- A public route table.
- Security groups controlling communication between the architecture tiers.

  5.5 Security and Communication

Security groups restrict communication between the different components.

| Source                    | Destination               | Protocol / Port |
| ------------------------- | ------------------------- | --------------- |
| Internet                  | Application Load Balancer | HTTP / 80       |
| Application Load Balancer | EC2                       | HTTP / 80       |
| EC2                       | RDS PostgreSQL            | TCP / 5432      |

The database does not accept direct public connections. Access is restricted to resources associated with the application security group.

5.6 Architecture Limitations

The current implementation uses a single EC2 instance and does not include Auto Scaling.

RDS Multi-AZ is also disabled.

The architecture therefore demonstrates the three-tier design but does not provide full high availability or automatic application scaling.

These decisions were made to keep the lab implementation simple and control AWS infrastructure costs.

## 6. Part B — Best Practices

The project applies Infrastructure as Code best practices, with particular attention to security, scalability, availability, performance and maintainability.

6.1 Security

- RDS storage encryption is enabled.
- The database is configured as non-publicly accessible.
- Security groups restrict database access to the application tier.
- Application traffic is accepted only from the Application Load Balancer security group.
- The database password uses CloudFormation’s NoEcho property.
- EC2 administration is performed through AWS Systems Manager Session Manager, avoiding the need for SSH access.

  6.2 Scalability and Availability

- An Application Load Balancer receives incoming HTTP traffic.
- Public and database subnets are distributed across two Availability Zones.
- RDS uses a dedicated DB subnet group containing two subnets in different Availability Zones.

Current limitations:

- The application tier uses a single EC2 instance.
- Auto Scaling is not configured.
- RDS Multi-AZ is disabled.

  6.3 Performance and Resource Efficiency

- Amazon Linux 2023 is used for the EC2 instance.
- The EC2 instance uses the t3.micro instance type.
- RDS uses gp3 storage.
- Small instance types are selected to control infrastructure costs.
- A NAT Gateway has been avoided to reduce additional running costs.

  6.4 Infrastructure as Code and Maintainability

- AWS resources are defined using CloudFormation YAML templates.
- Parameters allow configuration values to be managed separately from resource definitions.
- Outputs expose relevant infrastructure information, including resource IDs and endpoints.
- Git and GitHub are used for version control.
- Separate Git branches are used to organise development.
- CloudFormation template validation is performed before deployment.

## 7. Tools and Technologies

AWS Services

- AWS CloudFormation
- Amazon S3
- Amazon VPC
- Amazon EC2
- Amazon RDS (PostgreSQL)
- Application Load Balancer (ALB)
- AWS IAM
- AWS Systems Manager (SSM)
- AWS Systems Manager Parameter Store

Development and Version Control

- YAML
- Git
- GitHub
- Visual Studio Code
- AWS CLI

## 8. Deployment and Testing

8.1 Static Website Deployment

The static website infrastructure was successfully deployed using AWS CloudFormation.

The website was tested through its public S3 website endpoint.

Website endpoint:

http://atu-cloudformation-l00205501.s3-website-eu-west-1.amazonaws.com

8.2 Three-Tier Architecture Deployment

The three-tier architecture was successfully deployed using AWS CloudFormation in the Ireland region (eu-west-1).

CloudFormation stack: atu-three-tier

The deployment includes the VPC, subnets, security groups, Application Load Balancer, EC2 instance and RDS PostgreSQL database.

8.3 Infrastructure Testing

The following tests were performed:

| Test                                  | Result   |
| ------------------------------------- | -------- |
| CloudFormation template validation    | Passed   |
| Application Load Balancer HTTP access | Passed   |
| Apache HTTP Server status             | Active   |
| AWS Systems Manager connectivity      | Passed   |
| EC2 to RDS TCP connectivity           | Passed   |
| RDS public accessibility              | Disabled |

Additional checks:

- The application was accessed through the Application Load Balancer DNS endpoint.
- The EC2 instance was accessed using AWS Systems Manager Session Manager.
- The Apache service was confirmed as active.
- Network connectivity from EC2 to RDS was confirmed on TCP port 5432.

Note: Database network connectivity was verified, but database authentication and SQL queries were not tested.

8.4 CloudFormation Template Validation

The following AWS CLI command was used to validate the three-tier CloudFormation template:

```text
aws cloudformation validate-template \
  --template-body file://three-tier-architecture/template-tta.yaml \
  --region eu-west-1
```

The validation completed successfully.

8.5 Git Validation

The following command was used to check for whitespace errors:

```text
git diff --check
```

No whitespace errors were reported during the check.

## 9. References and AI Usage

9.1 AI Usage

ChatGPT was used as a support tool during the project, mainly to troubleshoot technical problems encountered during implementation.

One of the main challenges involved AWS IAM user permissions, which caused several difficulties when validating and deploying CloudFormation stacks and accessing AWS services.

ChatGPT was used to help understand permission-related errors and identify possible solutions.

9.2 References

- AWS CloudFormation official documentation.
- AWS documentation for Amazon S3, EC2, RDS, VPC and IAM.
- Git and GIthub official documentation.

## 10. Project Status

The main project requirements have been implemented:

| Requirement                                | Status    |
| ------------------------------------------ | --------- |
| Static website using Amazon S3             | Completed |
| CloudFormation template for static website | Completed |
| Three-tier architecture design             | Completed |
| Three-tier CloudFormation deployment       | Completed |
| Application Load Balancer configuration    | Completed |
| EC2 application server deployment          | Completed |
| RDS PostgreSQL deployment                  | Completed |
| Security groups and network configuration  | Completed |
| Infrastructure testing                     | Completed |
| README documentation                       | Completed |

The implementation has been tested in AWS. Some production-level features, such as HTTPS, Auto Scaling and RDS Multi-AZ, are outside the scope of the current lab implementation.

## 11. Conclusion

This project provided practical experience with Infrastructure as Code using AWS CloudFormation.

Both the static website and the three-tier architecture were successfully implemented and deployed.

The project helped develop practical skills in AWS networking, IAM, EC2, S3, RDS and CloudFormation. It also provided experience troubleshooting deployment and permission issues.

Overall, the lab demonstrated how CloudFormation can be used to define, deploy and manage AWS infrastructure through YAML templates.
