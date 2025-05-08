# 🔐 AWS IAM Lab – Identity and Access Management Project

In this hands-on project, I focused on building a secure Identity and Access Management (IAM) setup in AWS. 
I implemented least privilege principles, enforced Multi-Factor Authentication (MFA), and used CloudTrail and IAM policy simulation to monitor and troubleshoot access.

## 📌 Objective

- This project demonstrates how to design and implement AWS IAM best practices, including:
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
- EC2S3AccessRole created

## Attach Role to EC2 Instance
- Select your EC2 instance
- Under Actions dropdown > select security > select Modify IAM role
- Choose IAM role (i.e. EC2S3Accessrole)
- Select Update IAM role
![image](https://github.com/user-attachments/assets/a3721fd2-9f56-40ea-b00b-0e509979ff30)
	- IAM role created

## Create a Secure IAM Policy and Enforce MFA for Sensitive access
I created a policy to ensure least privilege is established and enforced MFA for dev_user and security_auditor when accessing the S3 bucket.
- Go to IAM dashboard
- Select policies from left pane > Create policy
- Select a service (i.e. S3)
  	- Under S3 actions: select GetObject > select PutObject
  	- Under resources: copy and paste bucket ARN
  	- Select Next
  	- ![image](https://github.com/user-attachments/assets/8ad6b21d-d708-4b02-9b53-b16cd5ccac89)
- Fill out policy name
- Create policy
  
<pre><code> 
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject"
            ],
            "Resource": "arn:aws:s3:::my-secure-dev-bucket/my-secure-dev-bucket",
            "Condition": {
                "Bool": {
                    "aws:MultiFactorAuthPresent": "true"
                }
            }
        }
    ]
}
</code></pre>

- ![image](https://github.com/user-attachments/assets/fd662f98-376b-4888-9b05-60724b24f5d9)
	- IAM policy created

## Attach Secure Policy to IAM user
Since both dev_user and security_user are in the same group i.e. DevTeam, I attached the policy to the group.
- On IAM dashboard, under user groups, select DevTeam
- Under permission policies > select Add permisions
  	- From Permissions options > Select Attach policies directly
  	- Search for the policy created (i.e. dev_team_policy)
  	- Select Next
- Select add permissions
- ![image](https://github.com/user-attachments/assets/caa1a0c4-29c2-4a8f-af11-30d611db48ae)
	- Policy attached to group

## Monitoring with CloudTrail & CloudWatch
We will be enabling CloudTrail to track IAM actions and monitor logs real-time using CloudWatch. For this, we will create a CloudTrail role for CloudTrail to send logs to CloudWatch.
- Create a CloudTrail role.
- From IAM dashboard, select the "Create Role" button
  	- Fill out the necessary details:
   	- Trusted Entity = AWS service > Use case = CloudTrail
  	- Select next
   	- Add Permissions: CloudTrailServiceRolePolicy was already autoselected
  	  	** We must add "CloudWatchLogsFullAccess" policy for CloudTrail to send logs to CloudWatch**
  	- Review and select create role.
- ![image](https://github.com/user-attachments/assets/b2cb8ad3-f7ac-4391-b072-a0db25e2b584)
- Now, let us go back to the IAM dashboard and create a custom policy for CloudTrail
- Select policies > create policy > specify permissions > Next
- Enter policy name > create policy
- ![image](https://github.com/user-attachments/assets/19a2f5ee-b585-4bcb-9d7d-78d35fa9cde4)

- To be continued!!


