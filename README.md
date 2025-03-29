# MediaCatalog DynamoDB Project (CloudFormation Edition)

This project builds a secure, read-only AWS DynamoDB-based media catalog infrastructure using Infrastructure-as-Code (IaC) via CloudFormation. It was built to simulate a real-world media company scenario for learning and portfolio demonstration.

---

## 🌐 Project Overview

Let's Go! Media Inc. needs a centralized, secure place to manage metadata for their media catalog (movies, genres, release dates, ratings, etc.). This solution includes:

- ✅ **DynamoDB Table** for storing 10 movie records
- ✅ **EC2 Instance** for access via AWS CLI
- ✅ **IAM Role + Instance Profile** with read-only access
- ✅ **Security Group** for SSH access (locked to specific IP)
- ✅ **CloudFormation YAML template** for full automation

---

## 💻 Technologies Used

- **Amazon DynamoDB**
- **Amazon EC2 (Amazon Linux 2023)**
- **IAM Role and Instance Profile**
- **AWS CLI** (for access and validation)
- **CloudFormation** (YAML-based deployment)

---

## ⚖️ Core Concepts Practiced

- Infrastructure as Code (IaC)
- Role-based Access Control (RBAC) with IAM
- SSH key management and EC2 provisioning
- VPC subnet and security group usage
- CloudFormation dependency management

---

## ⚠️ Key Challenges & Solutions

| Challenge | Solution |
|----------|----------|
| Stack failed due to resource name conflicts | Renamed all resources to unique names (e.g., `MediaCatalogCFN`) |
| EC2 launched before IAM profile ready | Used `DependsOn` directive in CloudFormation |
| SSH failed | Verified public IP, security group, and subnet settings |
| Write access mistakenly allowed | Validated with `AccessDeniedException` by testing a `put-item` command |

---

## 📁 How to Deploy This Project

1. Open [AWS CloudShell](https://console.aws.amazon.com/cloudshell/)
2. Upload `MediaCatalogStack.yaml`
3. Run the following:

```bash
aws cloudformation create-stack \
  --stack-name MediaCatalogStack \
  --template-body file://MediaCatalogStack.yaml \
  --capabilities CAPABILITY_NAMED_IAM
```

4. Wait for `CREATE_COMPLETE`
5. Get public IP of EC2:
```bash
aws ec2 describe-instances \
  --filters Name=instance-state-name,Values=running \
  --query "Reservations[*].Instances[*].[InstanceId,PublicIpAddress]" \
  --output table
```

6. SSH in:
```bash
ssh -i MediaCatalogKey.pem ec2-user@<PUBLIC_IP>
```

7. Run validation commands:
```bash
aws dynamodb scan --table-name MediaCatalogCFN
```

```bash
aws dynamodb put-item \
  --table-name MediaCatalogCFN \
  --item '{
    "Title": { "S": "Unauthorized Write Test" },
    "Genre": { "S": "Drama" },
    "ReleaseDate": { "S": "2025-12-01" },
    "Rating": { "S": "N/A" }
  }'
```

You should receive an `AccessDeniedException` for the write.

---

## 🚀 Future Improvements

- Automate movie data insert using a Lambda function
- Add CloudWatch Logs integration
- Turn into reusable nested stacks with parameters

---

## 📊 Project Status

✅ Fully built, validated, and security-tested  
✅ Documented for Medium + GitHub  
✅ Ready to showcase in interviews!

---

## 👤 Author
**Selina Loggins**  
Aspiring Cloud Engineer | DevOps & Security Enthusiast  
#CloudEngineerInProgress 🌟

