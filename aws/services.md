# AWS - Essential Services

## EC2 (Elastic Compute Cloud)

```bash
# SSH connection
ssh -i my-key.pem ec2-user@<PUBLIC_IP>

# AWS CLI - Launch an instance
aws ec2 run-instances \
    --image-id ami-0abcdef1234567890 \
    --instance-type t2.micro \
    --key-name my-key \
    --security-group-ids sg-12345 \
    --count 1
```

### Common Instance Types

| Type | vCPU | RAM | Usage |
|------|------|-----|-------|
| t2.micro | 1 | 1 GB | Free tier, testing |
| t2.small | 1 | 2 GB | Small apps |
| t3.medium | 2 | 4 GB | Production apps |

## ECR (Elastic Container Registry)

```bash
# Login to ECR registry
aws ecr get-login-password --region eu-central-1 | \
    docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.eu-central-1.amazonaws.com

# Tag and push an image
docker tag my-app:1.0 <ACCOUNT_ID>.dkr.ecr.eu-central-1.amazonaws.com/my-app:1.0
docker push <ACCOUNT_ID>.dkr.ecr.eu-central-1.amazonaws.com/my-app:1.0
```

## VPC (Virtual Private Cloud)

Analogy: A VPC is like a corporate local network in the cloud. Subnets are the floors of the building, the Security Group is the security guard, and the Internet Gateway is the front door.

| Component | Role |
|-----------|------|
| VPC | Isolated network in AWS |
| Subnet | Sub-network (public or private) |
| Internet Gateway | VPC <-> Internet connection |
| NAT Gateway | Allows private instances to access the Internet |
| Route Table | Traffic routing rules |

## Security Groups

```bash
# Create a security group
aws ec2 create-security-group \
    --group-name my-sg \
    --description "My security group" \
    --vpc-id vpc-12345

# Add an inbound rule
aws ec2 authorize-security-group-ingress \
    --group-id sg-12345 \
    --protocol tcp \
    --port 8080 \
    --cidr 0.0.0.0/0
```

Principle: **everything is blocked by default**. Only open the necessary ports.

## IAM (Identity and Access Management)

| Concept | Description |
|---------|-------------|
| User | Person or service that interacts with AWS |
| Group | Collection of users with the same permissions |
| Role | Temporary permissions assignable to a service |
| Policy | JSON document defining permissions |

**Best practice**: Use Roles (not Users) for AWS services. For example, an EC2 instance that needs to access ECR uses an IAM Role, not access keys.
