🎬 MediaCatalog: DynamoDB on AWS (IaC Edition)

A secure, read-only DynamoDB infrastructure project built with AWS CloudFormation.  
Perfect for practicing Infrastructure-as-Code (IaC), IAM, VPC, and EC2 provisioning.

---

🌐 Project Overview

This hands-on project simulates a media company (Let's Go! Media Inc.) needing centralized metadata storage for their movie catalog.

🎯 Goal: Build a secure, automated infrastructure using AWS best practices  
🧩 Challenge: Only allow *read access* from an EC2 instance, deny all writes  
💻 Deployment: Fully automated with a single CloudFormation template

---

🧱 What I Built

- ✅ DynamoDB Table – `MediaCatalogCFN`, preloaded with 10 movies
- ✅ IAM Role + Instance Profile – Read-only access to the table
- ✅ EC2 Instance – Securely configured with SSH access only from a specific IP
- ✅ Security Group – Port 22 open only to my IP
- ✅ CloudFormation Template – Deploys all resources in one stack

---

🔐 Security Features

- ✔️ IAM least privilege** policy (read-only)
- ✔️ Write access blocked – validated with `AccessDeniedException`
- ✔️ Public access restricted via security group + subnet controls
- ✔️ No hardcoded secrets – SSH key stored securely

---

🚀 Deployment Instructions

```bash
# 1. Upload YAML template to AWS CloudShell
# 2. Create stack
aws cloudformation create-stack \
  --stack-name MediaCatalogStack \
  --template-body file://MediaCatalogStack.yaml \
  --capabilities CAPABILITY_NAMED_IAM

# 3. Get EC2 public IP
aws ec2 describe-instances \
  --filters Name=instance-state-name,Values=running \
  --query "Reservations[*].Instances[*].[PublicIpAddress]" \
  --output table

# 4. SSH in
ssh -i MediaCatalogKey.pem ec2-user@<PUBLIC_IP>

# 5. Validate
aws dynamodb scan --table-name MediaCatalogCFN
aws dynamodb put-item --table-name MediaCatalogCFN --item '{"Title":{"S":"ShouldFail"},"Genre":{"S":"Test"},"ReleaseDate":{"S":"2025-01-01"},"Rating":{"S":"N/A"}}'

🛑 The second command should fail with an AccessDeniedException.

💡 Skills Demonstrated

AWS CLI + EC2 provisioning
CloudFormation (YAML-based Infrastructure-as-Code)
IAM roles, instance profiles, and security policies
DynamoDB schema design + access control
Troubleshooting real-world errors (SSH, VPC, IAM)
GitHub version control + README documentation
🧠 Lessons Learned

Challenges AND	Solutions
(Challenge) Stack failed due to name conflicts 	(Solution) Appended -CFN to avoid duplicates
(Challenge) IAM role failed due to timing	(Solution) Added DependsOn to control resource creation order
(Challenge) SSH timed out	(Solution) Verified IP, security group, subnet route, and IGW
(Challenge) Write access worked initially	(Solution) Replaced IAM policy with read-only, validated denial properly


👤 Author
Selina Loggins
Cloud Engineer | DevOps & Security Enthusiast
🔗 Connect with me on LinkedIn (if you have one!)
💬 Passionate about infrastructure, automation, and the power of cloud ☁️

📌 Project Status

✅ Successfully built
✅ Validated & tested
✅ Documented for GitHub and Medium

