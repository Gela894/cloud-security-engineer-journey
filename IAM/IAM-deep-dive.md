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
  	 ![image](https://github.com/user-attachments/assets/71543773-6813-4207-ae67-3a79f9208b2b)

- Fill out policy name
- Create policy
  
<pre><code> 
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "s3:ListAllMyBuckets",
			"Resource": "*"
		},
		{
			"Effect": "Allow",
			"Action": [
				"s3:ListBucket",
				"s3:GetObject",
				"s3:PutObject"
			],
			"Resource": [
				"arn:aws:s3:::my-secure-dev-bucket",
				"arn:aws:s3:::my-secure-dev-bucket/*"
			],
			"Condition": {
				"Bool": {
					"aws:MultiFactorAuthPresent": "true"
				}
			}
		}
	]
}
</code></pre>

![image](https://github.com/user-attachments/assets/9984cbaa-6a46-4032-ba1f-4037a82556bb)
	- IAM policy created

## Attach Secure Policy to IAM user
Since both dev_user and security_user are in the same group i.e. DevTeam, I attached the policy to the group.
- On IAM dashboard, under user groups, select DevTeam
- Under permission policies > select Add permisions
  	- From Permissions options > Select Attach policies directly
  	- Search for the policy created (i.e. dev_team_policy)
  	- Select Next
- Select add permissions
 ![image](https://github.com/user-attachments/assets/caa1a0c4-29c2-4a8f-af11-30d611db48ae)
	- Policy attached to group

## Monitoring with CloudTrail & CloudWatch
We will be enabling CloudTrail to track IAM actions and monitor logs real-time using CloudWatch. For this, we will create a CloudTrail role for CloudTrail to send logs to CloudWatch.
- Let's begin by creating a custom policy:
  	- From IAM dashboard, select Policies > create policy
  	- Use the JSON policy editor > select next
  	- Fill in policy name "CloudTrailCloudWatchLogsPolicy" > create policy
  ![image](https://github.com/user-attachments/assets/bc02b2b5-033f-4048-8787-fea034fee55c)
  	- Custom Policy created
- For this next step, I created a new IAM role with a different approach thus by using a Custom trust policy instead of AWS service. The reason for this is to allow us customize the trust policy for Cloudtrail instead of using the service template. The AWS service template comes with a pre-attached AWS managed policy for Cloudtrail which isn't modifiable, hence, I decided to use the approach of a Custom trust policy.
- From IAM dashboard, select Policies > create policy
	- In the policy editor, I edited the policy statement as:
 <pre><code> 
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "Statement1",
			"Effect": "Allow",
			"Principal": {},
			"Action": "sts:AssumeRole"
		}
	]
}	 
</code></pre>

- Next Add the AWS managed policy titled "AWSCloudTrail_ReadOnlyAccess" and "CloudTrailCloudWatchLogsPolicy"
- Select next > name policy > review and create role
 ![image](https://github.com/user-attachments/assets/0c237f91-4144-4678-bf70-9e66295b5210)
	- CloudTrail_role created

- Upon executing the activity above, I asked a question "What's a trust relationship?" The answer is, a trust relationship delegate access for defined entities such as users, roles to perform specific IAM role functions. Obviously, a good example is what I have illustrated above, thus a trust relationship between CloudTrail and CloudWatch.

- Next, we are going to configure CloudTrail to start tracking logs in realtime.
	- From AWS management console, search CloudTrail
  	- Create trail > and fill out the following details:
  	  	- Trail name > create or choose existing S3 bucket > disable Log file SSE-KMS encryption > enable Log file 		  validation.
  	  	- Enable CloudWatch logs > choose existing IAM role (i.e.CloudTrail_role) > select next
  	  	- Under Event type, select management events
  	  	- Under Management Events, Choose "Read" and "Write" and select Next
  	  	- Review, create and select Next
     	![image](https://github.com/user-attachments/assets/377ba6d4-ceed-498a-b5c7-cd2ff559c88e)
  	![image](https://github.com/user-attachments/assets/ceafb905-0194-403e-a88a-80df432e93cc)
		- Created trail named "log-management-trail" 

 ## Test CloudTrail and CloudWatch
Now, we are going to perform an activivty to simulate an unauthorized action. We will log in as dev_user to attempt deleting an S3 bucket (specifically "my-secure-dev-bucket"). Then we will navigate to CloudWatch to verify logs for "AccessDenied"

- Log in as dev_user
- Search for S3 in the AWS management console
  	- Under the general purpose bucket, select "my-secure-dev-bucket" and select Delete
  ![image](https://github.com/user-attachments/assets/5aad2d18-4021-4075-9d8b-9c3e71524d48)
   ![image](https://github.com/user-attachments/assets/4ed797ba-a56f-48fe-9eea-3fbbecab4085)

- The image above proves the unathorized action performed by dev_user. As a cloud security engineer, reviewing and monitoring logs is an essential task in our daily job activities. Hence, I will navigate to CloudTrail and CloudWatch to verify any suspicious activities.

### Verify Event History in CloudTrail
- Navigate to CloudTrail and select Event history
- You should see the recent denied activity, click on Event name to view details.
  ![image](https://github.com/user-attachments/assets/92278e07-30c4-4677-83b2-a7b5b72897ef)
  ![image](https://github.com/user-attachments/assets/c6a82de6-15a5-4d39-ab50-04b106c5d3ee)

Now, navigate to CloudWatch to verify log events.
- From AWS console search CloudWatch
- Under Logs, select Log groups
![image](https://github.com/user-attachments/assets/b3f1130b-57c2-4dd6-95e2-2eb8642f981d)
-Open the latest log streams and search for "DeleteBucket" or "AccessDenied" > enter
![image](https://github.com/user-attachments/assets/f4184165-13ed-4218-8186-29bce863563e)
- Upon expanding detals of log events, details in the image below shows the "AccessDenied" error
![image](https://github.com/user-attachments/assets/983c8e31-e303-4f54-8054-ea16c6a50549)

### Conclusion







  





  




