# ☁️ AWS-Native Full Architecture  
### (Compute + Serverless + CI/CD + Infrastructure as Code)

This repository defines a **cloud-native video streaming and analytics platform** fully hosted on **Amazon Web Services (AWS)**.  
It combines **EKS (Kubernetes)**, **RDS**, **ElastiCache**, **Lambda**, and **Kinesis** for scalable compute and serverless workloads — all managed through **Terraform**, **Ansible**, and **AWS CodePipeline/CodeBuild** with optional **GitHub or GitLab CI** integrations.

It supports:
- 🧱 **Compute services** — containerized microservices deployed on **Amazon EKS**
- 🌀 **Serverless services** — asynchronous workloads via **AWS Lambda**
- ⚙️ **Infrastructure as Code** — provisioned via **Terraform**
- 🔁 **Continuous Integration / Continuous Deployment** — powered by **CodeBuild**, **CodePipeline**, or **GitHub/GitLab Actions**
- 🔒 **Security, Observability, and Data Analytics** — managed through **IAM**, **Secrets Manager**, **CloudWatch**, and **Kinesis / Redshift**
- ⚒️ **Configuration Management** — handled by **Ansible** for EC2-based tooling and bastions

---

```bash
your-project/
├─ docker-compose.yml                       # 🐳 Local dev stack (Postgres, Redis, mock services)
├─ .env                                     # Environment vars for local Docker
├─ .gitignore                               # Ignore local secrets, node_modules, build outputs
├─ README.md                                # Project overview, setup, run instructions
│
├─ services/                                # 🌐 Backend microservices (polyglot)
│  ├─ gateway/                              # Node.js API Gateway (Dockerized, runs in EKS)
│  ├─ auth-service/                         # Python Auth (Flask/FastAPI)
│  ├─ content-service/                      # Go service (content APIs)
│  ├─ billing-service/                      # Java (Spring Boot)
│  ├─ catalog-service/                      # Metadata API
│  ├─ playback-service/                     # Playback URL signing, media serving
│  └─ shared-lib/                           # Shared backend libraries (Node/Python/Go/Java)
│
├─ database/
│  └─ init/init.sql                         # SQL schema (executed on Amazon RDS)
│
├─ data/                                    # 📊 Schemas & specs for AWS data services
│  ├─ dynamodb-schema.json                  # DynamoDB table/index definitions (Terraform)
│  ├─ kinesis-streams.yaml                  # Kinesis streams / Firehose / SNS/SQS topics spec
│  └─ redshift-schema.sql                   # Redshift / Athena tables DDL (Terraform)
│
├─ redis-data/                              # Local Redis volume for dev (Docker only)
│
├─ frontend/                                # 💻 React frontends (Dockerized, served via EKS + CloudFront)
│  ├─ users/                                # User portal
│  ├─ team/                                 # Content team management
│  ├─ dev/                                  # Developer console (API monitoring)
│  └─ admin/                                # Admin dashboard
│
├─ shared/                                  # 🎨 Shared UI & logic (frontend only)
│  ├─ ui/
│  ├─ hooks/
│  └─ utils/
│
├─ scripts/                                 # ⚙️ Local + deployment helper scripts
│  ├─ dev-start.sh                          # Run Docker Compose locally
│  ├─ dev-stop.sh                           # Stop local containers
│  ├─ build-all-images.sh                   # Build all Docker images (Docker)
│  ├─ push-all-images.sh                    # Push all images to ECR (Elastic Container Registry)
│  ├─ aws-auth.sh                           # aws configure / STS login / role assumption
│  ├─ aws-create-infra.sh                   # Terraform wrapper (infra provisioning on AWS)
│  ├─ aws-deploy-eks.sh                     # Deploy Helm charts / manifests to EKS
│  └─ migrate-db.sh                         # Apply DB migrations to RDS
│
├─ infra/                                   # ☁️ Infrastructure & DevOps (Terraform, Ansible, CI/CD)
│  ├─ terraform/                            # 🧱 Infrastructure as Code — creates ALL AWS resources
│  │  ├─ envs/
│  │  │  ├─ dev/
│  │  │  ├─ staging/
│  │  │  └─ prod/
│  │  │      ├─ main.tf                     # Calls infra modules
│  │  │      ├─ variables.tf
│  │  │      ├─ backend.tf                  # S3 backend for TF state (+ DynamoDB lock)
│  │  │      └─ terraform.tfvars            # Env vars (account_id, region, CIDRs, etc.)
│  │  │
│  │  └─ modules/
│  │     ├─ account/                        # Account baseline: CloudTrail, Config, tagging
│  │     ├─ vpc/                            # VPC, subnets, routes, NAT, IGW
│  │     ├─ eks/                            # EKS cluster + node groups
│  │     ├─ rds/                            # Amazon RDS (Postgres/MySQL)
│  │     ├─ elasticache/                    # Redis cache
│  │     ├─ dynamodb/                       # NoSQL tables (sessions, metadata)
│  │     ├─ s3/                             # S3 buckets (media, assets, logs)
│  │     ├─ kinesis/                        # Streams / Firehose
│  │     ├─ sns-sqs/                        # Messaging topics/queues
│  │     ├─ redshift-athena/                # Redshift clusters / Athena catalogs
│  │     ├─ cloudfront-alb/                 # Load balancer + CloudFront + ACM certs
│  │     ├─ secrets-manager/                # Secrets (JWTs, API keys)
│  │     ├─ iam/                            # IAM roles & policies
│  │     └─ cloudwatch/                     # Metrics, dashboards, alarms
│  │
│  │  # 🏗️ Terraform provisions all AWS resources:
│  │  #    - EKS, VPC, RDS, S3, Kinesis, DynamoDB, IAM, CloudWatch, etc.
│  │  # ❌ No app deployment or OS config — handled by CI/CD + Ansible.
│
│  ├─ ansible/                              # ⚒️ Config management (for EC2, not EKS pods)
│  │  ├─ inventories/
│  │  │  ├─ dev/hosts.ini
│  │  │  ├─ staging/hosts.ini
│  │  │  └─ prod/hosts.ini
│  │  ├─ playbooks/
│  │  │  ├─ bootstrap-bastion.yml           # SSH/users setup, baseline hardening
│  │  │  ├─ configure-ec2-tools.yml         # Install monitoring, CI runners, etc.
│  │  │  └─ maintenance.yml                 # Patching, cleanup, cron jobs
│  │  └─ roles/
│  │     ├─ common/
│  │     ├─ app-node/
│  │     └─ cloudwatch-agent/               # CloudWatch agent install/config
│  │
│  │  # ⚙️ Terraform = builds EC2s, Ansible = configures EC2s.
│
│  ├─ kubernetes/                           # ☸️ EKS cluster manifests
│  │  ├─ eks-cluster-config/
│  │  ├─ namespaces/
│  │  ├─ ingress/ingress.yaml               # ALB ingress via AWS Load Balancer Controller
│  │  ├─ helm/                              # Helm charts per service
│  │  └─ observability/                     # Prometheus/Grafana configs
│
│  ├─ cicd/                                 # 🔁 CI/CD (CodeBuild / CodePipeline / GitHub / GitLab)
│  │  ├─ codebuild/
│  │  │  ├─ buildspec-apps.yml              # Build & test apps, push Docker images to ECR
│  │  │  └─ buildspec-infra.yml             # Terraform plan/apply
│  │  ├─ codepipeline/
│  │  │  ├─ apps-pipeline.json              # CodePipeline: Git → CodeBuild → EKS
│  │  │  └─ infra-pipeline.json             # CodePipeline: Git → Terraform
│  │  ├─ github/
│  │  │  └─ workflows/
│  │  │     ├─ ci-apps.yml                 # Tests + CodeBuild trigger
│  │  │     └─ ci-infra.yml                # Infra deploy via Terraform/CodeBuild
│  │  └─ gitlab/
│  │     └─ .gitlab-ci.yml                  # GitLab pipeline → CodeBuild/EKS
│
│  │  # 🚀 AWS-native CI/CD flow:
│  │  #    - Push → CodePipeline → CodeBuild
│  │  #    - Build containers → ECR
│  │  #    - Apply Terraform → AWS infra
│  │  #    - Deploy → EKS via Helm
│  │  # Optional: integrate Jenkins or GitHub Actions
│
│  └─ monitoring-logging/                   # 📈 Observability
│     ├─ cloudwatch-metrics/               # Dashboards, alarms, metrics
│     └─ cloudwatch-logs/                  # Log groups, retention, filters
│
└─ docs/                                   # 📚 Documentation
   ├─ ARCHITECTURE.md                      # High-level AWS architecture diagram
   ├─ DEPLOYMENT.md                        # How to deploy (Terraform, CodeBuild, EKS)
   ├─ DEVOPS_GUIDE.md                      # Terraform + Ansible + EKS + CI/CD explained
   ├─ MEDIA_PIPELINE.md                    # S3 + CloudFront + playback-service flow
   ├─ DATA_ANALYTICS.md                    # Kinesis → S3 → Athena/Redshift
   ├─ SECURITY.md                          # IAM, Secrets, VPC, SGs
   ├─ MONITORING.md                        # CloudWatch metrics/logs/alarms
   └─ AWS_INFRA.md                         # Detailed Terraform module reference
