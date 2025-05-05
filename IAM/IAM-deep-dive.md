# 🔐 AWS IAM Lab – Identity and Access Management Project

In this hands-on project, I focused on building a secure Identity and Access Management (IAM) setup in AWS. 
I implemented least privilege principles, enforced Multi-Factor Authentication (MFA), and used CloudTrail and IAM policy simulation to monitor and troubleshoot access.

## 📌 Objective

This project demonstrates how to design and implement AWS IAM best practices, including:
	- Create IAM users, roles, and groups with least-privilege policies
	- Enforce MFA on sensitive roles
	- Use CloudTrail and IAM policy simulator to detect and respond to access issues
  - Simulate an insider threat and demonstrate how AWS security tools help prevent misuse


---

## 🧱 Architecture Overview

- I started by creating:
  - Two IAM users: dev_user, security_auditor
  - One IAM group: DevTeam
  - A custom IAM role for EC2 instances to access S3 securely

## Create IAM users and group
  - Navigate to AWS console
  - Search IAM
  - Select users under Access management in left pane
  - Select create user, specify permissions, assign user to DevTeam group
      - Ensure to create DevTeam group in order to add user to group.
        
![image](https://github.com/user-attachments/assets/8189f2ea-035f-41c5-b98f-6b0c5e764636)

* Created dev_user and security_auditor and assigned to DevTeam group.

## Create custom IAM role for EC2 to access S3 securely
  - On IAM dashboard, in the left pane, select Roles.
  - Select the "Create Role" button
  - Fill out the necessary details:
      - Trusted Entity = AWS service > Use case = EC2
      - Add Permissions = search and attach AmazonS3ReadOnlyAccess
      - Add Role name, review and select create role

![image](https://github.com/user-attachments/assets/7de21ba4-d789-4035-821f-16a0c964235a)

![image](https://github.com/user-attachments/assets/8e14cf0f-d3ea-4048-b36d-0af56dcb58d0)
- EC2S3AccessRole created.

## Attach Role to EC2 Instance
- Select EC2 instance
-   To be continued!


