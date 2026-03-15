# AWS - Services essentiels

## EC2 (Elastic Compute Cloud)

```bash
# Connexion SSH
ssh -i my-key.pem ec2-user@<PUBLIC_IP>

# AWS CLI - Lancer une instance
aws ec2 run-instances \
    --image-id ami-0abcdef1234567890 \
    --instance-type t2.micro \
    --key-name my-key \
    --security-group-ids sg-12345 \
    --count 1
```

### Types d'instances courants

| Type | vCPU | RAM | Usage |
|------|------|-----|-------|
| t2.micro | 1 | 1 Go | Free tier, tests |
| t2.small | 1 | 2 Go | Petites apps |
| t3.medium | 2 | 4 Go | Apps de production |

## ECR (Elastic Container Registry)

```bash
# Login au registry ECR
aws ecr get-login-password --region eu-central-1 | \
    docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.eu-central-1.amazonaws.com

# Tagger et pousser une image
docker tag my-app:1.0 <ACCOUNT_ID>.dkr.ecr.eu-central-1.amazonaws.com/my-app:1.0
docker push <ACCOUNT_ID>.dkr.ecr.eu-central-1.amazonaws.com/my-app:1.0
```

## VPC (Virtual Private Cloud)

Analogie : Un VPC c'est comme un reseau local d'entreprise dans le cloud. Les subnets sont les etages du batiment, le Security Group est le vigile, et l'Internet Gateway est la porte d'entree.

| Composant | Role |
|-----------|------|
| VPC | Reseau isole dans AWS |
| Subnet | Sous-reseau (public ou prive) |
| Internet Gateway | Connexion VPC <-> Internet |
| NAT Gateway | Permet aux instances privees d'acceder a Internet |
| Route Table | Regles de routage du trafic |

## Security Groups

```bash
# Creer un security group
aws ec2 create-security-group \
    --group-name my-sg \
    --description "Mon security group" \
    --vpc-id vpc-12345

# Ajouter une regle inbound
aws ec2 authorize-security-group-ingress \
    --group-id sg-12345 \
    --protocol tcp \
    --port 8080 \
    --cidr 0.0.0.0/0
```

Principe : **tout est bloque par defaut**. On ouvre uniquement les ports necessaires.

## IAM (Identity and Access Management)

| Concept | Description |
|---------|-------------|
| User | Personne ou service qui interagit avec AWS |
| Group | Collection d'users avec les memes permissions |
| Role | Permissions temporaires assignables a un service |
| Policy | Document JSON definissant les permissions |

**Bonne pratique** : Utiliser des Roles (pas des Users) pour les services AWS. Par exemple, une instance EC2 qui doit acceder a ECR utilise un Role IAM, pas des access keys.
