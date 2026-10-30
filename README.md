# ATU CloudFormation Lab

## 1. Project Overview

This individual project explores Infrastructure as Code (IaC) using
AWS CloudFormation as part of the Postgraduate Diploma in Computing
in DevOps at Atlantic Technological University (ATU).

The project covers static website hosting, three-tier architecture,
and infrastructure design best practices.

## 2. Student Information

- **Name:** Manuel Sanjose
- **Student Number:** L00205501
- **Course:** Postgraduate Diploma in Computing in DevOps
- **Module:** IaC for DevOps Pipelines

## 3. Project Structure

```text
cloudformation-lab1/
├── static-website/
│     ├── template.yaml
│     └── website/
│         ├── index.html
│         └── style.css
├── three-tier-architecture/
│   ├── template-tta.yaml
│   └── template-tta-backup.yaml
└── README.md
```

## 4. Part A — Static Website

**Status: Implemented and deployed**

The static website is hosted using Amazon S3 and its infrastructure is defined with AWS CloudFormation.

The implementation includes:

- S3 bucket configured for static website hosting.
- Public access configuration for the website.
- HTML and CSS files.
- CloudFormation template to define the infrastructure.

Website endpoint:

http://atu-cloudformation-l00205501.s3-website-eu-west-1.amazonaws.com

## 5. Part A — Three-Tier Architecture

**Status: CloudFormation template implemented and syntax validated. Deployment pending.**

The architecture is defined using AWS CloudFormation and includes:

- Web tier: Application Load Balancer to receive incoming HTTP traffic.
- Application tier: EC2 instance running an Apache web server.
- Database tier: Amazon RDS PostgreSQL database.

The infrastructure also defines a VPC, public and private subnets, route tables, and security groups to control communication between the tiers.

The CloudFormation template has passed template validation. The architecture has not yet been deployed to AWS.

## 6. Part B — Best Practices

The project applies several Infrastructure as Code best practices through AWS CloudFormation.

##Security##

- Database storage encryption is enabled for Amazon RDS.
- The database is configured as non-publicly accessible.
- Security groups restrict database access to the application tier.
- Application traffic is accepted from the Application Load Balancer security group.
- The database password parameter uses CloudFormation’s NoEcho property to prevent normal parameter display.

##Scalability and Availability##

- The infrastructure uses an Application Load Balancer to receive incoming HTTP traffic.
- Public and database subnets are distributed across two Availability Zones.
- The database is deployed in a dedicated DB subnet group.

Current limitations:

- The application tier currently uses a single EC2 instance.
- Auto Scaling is not configured.
- RDS Multi-AZ is disabled.

##Performance and Resource Efficiency##

- The EC2 instance uses the Amazon Linux 2023 image retrieved through AWS Systems Manager Parameter Store.
- The database uses gp3 storage.
- Small instance types are selected to keep lab infrastructure costs under control.

##Infrastructure as Code and Maintainability##

- AWS resources are defined using CloudFormation YAML templates.
- Git and GitHub are used for version control.
- Separate Git branches and pull requests are used to organise development.
- CloudFormation template validation is performed before deployment.

## 7. Tools and Technologies

- AWS CloudFormation
- Amazon S3
- Git
- GitHub
- Visual Studio Code

## 8. Deployment and Testing

Static Website

The static website infrastructure was deployed to AWS using CloudFormation.

Website endpoint:

http://atu-cloudformation-l00205501.s3-website-eu-west-1.amazonaws.com

Three-Tier Architecture

The three-tier architecture has not yet been deployed. Its CloudFormation template has been validated using the AWS CLI.

Validation command:

````bash
aws cloudformation validate-template \
 --template-body file://three-tier-architecture/template-tta.yaml \
 --region eu-west-1 \
 --no-cli-pager

Result: The template passed CloudFormation template validation.

Git Validation

The following command was used to check for whitespace errors:

```bash
git diff --check

Result: No whitespace errors were reported.

## 9. References and AI Usage

Tutorials, external references, and AI assistance will be documented here, including relevant prompts and modifications to tutorial-based examples.

## 10. Project Status

The project is currently in progress.

- Static website CloudFormation template: Completed.
- Static website deployment: Completed.
- Three-tier architecture template: Implemented.
- Three-tier template validation: Passed.
- Three-tier architecture deployment: Pending.
- Best practices documentation: Documented.
- Lab write-up and final documentation: In progress.

The remaining work includes reviewing and testing the three-tier architecture, completing the documentation, and preparing the final submission.
````
