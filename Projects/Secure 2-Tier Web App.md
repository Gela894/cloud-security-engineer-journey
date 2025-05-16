###📌 Project Overview:
You are working as a Cloud Security Engineer for a tech startup called SecureNext Technologies. The company is planning to launch a new SaaS product called "DataVault Lite", which allows businesses to securely store and manage customer data with a simplified 2-Tier architecture. Your responsibility is to ensure that the solution adheres to Defense in Depth (DiD) principles, is fully monitored for threats, and aligns with the AWS Shared Responsibility Model.

###🎯 Project Goals:
- Security by Design: Implement security at every layer (network, application, and data).

- Monitoring and Logging: Enable real-time threat detection and centralized logging.

- Compliance and Visibility: Ensure the architecture meets compliance standards (e.g., CIS, NIST).

- Threat Detection and Response: Set up alerts for potential threats and vulnerabilities.

###🗺️ Background:
- SecureNext Technologies is developing "DataVault Lite," a lightweight data storage and management solution. Unlike its larger sibling, DataVault, this version is optimized for fast deployment and scalability, utilizing a 2-Tier architecture:

  * Web/Application Layer: Application Load Balancer (ALB) + Auto Scaling Group (ASG) in private subnets

  * Database Layer: Amazon RDS (MySQL) with Multi-AZ deployment

- The infrastructure must be highly secure, monitored for real-time threats, and designed with AWS best practices. You have been tasked with architecting and securing the entire stack.
