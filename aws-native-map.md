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
│  ├─ ui/                                   # Reusable React components
│  ├─ hooks/                                # Common frontend hooks
│  └─ utils/                                # Shared helpers
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
│  │  ├─ envs/                              # Environment-specific infra (state split)
│  │  │  ├─ dev/
│  │  │  ├─ staging/
│  │  │  └─ prod/
│  │  │      ├─ main.tf                     # Calls infra modules
│  │  │      ├─ variables.tf
│  │  │      ├─ backend.tf                  # S3 backend for TF state (+ DynamoDB lock)
│  │  │      └─ terraform.tfvars            # Env vars (account_id, region, CIDRs, etc.)
│  │  │
│  │  └─ modules/                           # Modular reusable AWS infra components
│  │     ├─ account/                        # Optional: account baseline, CloudTrail, Config, tags
│  │     ├─ vpc/                            # Creates VPC, subnets, route tables, NAT, IGW
│  │     ├─ eks/                            # Creates EKS cluster + node groups
│  │     ├─ rds/                            # Creates Amazon RDS (Postgres/MySQL)
│  │     ├─ elasticache/                    # Creates ElastiCache Redis cluster
│  │     ├─ dynamodb/                       # Creates DynamoDB tables (metadata, sessions, etc.)
│  │     ├─ s3/                             # Creates S3 buckets (media, assets, logs)
│  │     ├─ kinesis/                        # Creates Kinesis streams / Firehose delivery streams
│  │     ├─ sns-sqs/                        # SNS topics & SQS queues (notifications, async)
│  │     ├─ redshift-athena/                # Redshift clusters or Athena catalogs
│  │     ├─ cloudfront-alb/                 # App Load Balancer + CloudFront distributions + ACM certs
│  │     ├─ secrets-manager/                # Secrets (JWT, DB creds, API keys)
│  │     ├─ iam/                            # IAM roles, policies, instance profiles
│  │     └─ cloudwatch/                     # CloudWatch alarms, dashboards, log groups, metrics
│  │
│  │  # 🏗️ Terraform builds and manages EVERYTHING inside AWS:
│  │  #    - VPC, EKS, RDS, ElastiCache, DynamoDB, S3, Kinesis, SNS/SQS, IAM, CloudWatch, etc.
│  │  # ❌ Not responsible for OS configuration, app deployment, or container builds.
│
│  ├─ ansible/                              # ⚒️ Config management (for EC2 instances, not EKS pods)
│  │  ├─ inventories/
│  │  │  ├─ dev/hosts.ini                   # EC2 inventory for dev (bastions, tools, runners)
│  │  │  ├─ staging/hosts.ini
│  │  │  └─ prod/hosts.ini
│  │  ├─ playbooks/
│  │  │  ├─ bootstrap-bastion.yml           # Create users, SSH hardening, baseline security
│  │  │  ├─ configure-ec2-tools.yml         # Install tools/agents on EC2 utility instances
│  │  │  └─ maintenance.yml                 # Patching, cleanup, cron jobs
│  │  └─ roles/
│  │     ├─ common/                         # OS packages, security hardening
│  │     ├─ app-node/                       # If any apps run directly on EC2 (not in EKS)
│  │     └─ cloudwatch-agent/               # Install/Configure CloudWatch agent on EC2
│  │
│  │  # ⚙️ Ansible is NOT creating AWS infra — only configuring EC2 instances.
│  │  # Terraform creates EC2 instances, Ansible SSHs into them to configure.
│
│  ├─ kubernetes/                           # ☸️ EKS cluster-level manifests (managed by kubectl/Helm)
│  │  ├─ eks-cluster-config/               # Namespaces, RBAC, NetworkPolicies for EKS
│  │  ├─ namespaces/
│  │  │  ├─ gateway.yaml                    # Deploy Gateway
│  │  │  ├─ auth-service.yaml
│  │  │  ├─ content-service.yaml
│  │  │  ├─ billing-service.yaml
│  │  │  ├─ catalog-service.yaml
│  │  │  ├─ playback-service.yaml
│  │  │  ├─ frontend.yaml
│  │  │  └─ rds-proxy.yaml                  # RDS Proxy or sidecar configuration for DB access
│  │  ├─ ingress/ingress.yaml               # Ingress resources -> ALB (via AWS Load Balancer Controller)
│  │  ├─ helm/                              # Helm charts for each service (values per env)
│  │  └─ observability/                     # ConfigMaps for metrics, Prometheus scraping, etc.
│  │
│  │  # ☸️ EKS is created by Terraform. Deployments handled by Helm/kubectl/Cloud-native CI/CD.
│  │  # ❌ Ansible doesn't manage Kubernetes; it only handles EC2-level config.
│
│  ├─ cicd/                                 # 🔁 CI/CD pipelines (CodeBuild / CodePipeline + GitHub/GitLab)
│  │  ├─ codebuild/                         # AWS CodeBuild definitions (buildspecs)
│  │  │  ├─ buildspec-apps.yml              # Build & test services, push images to ECR
│  │  │  └─ buildspec-infra.yml             # Run Terraform plan/apply from CodeBuild
│  │  ├─ codepipeline/                      # AWS CodePipeline definitions (optional)
│  │  │  ├─ apps-pipeline.json              # Pipeline: Git -> CodeBuild -> EKS deploy
│  │  │  └─ infra-pipeline.json             # Pipeline: Git -> CodeBuild (Terraform) -> Apply
│  │  ├─ github/                            # 🐙 GitHub Actions integration
│  │  │  └─ workflows/
│  │  │     ├─ ci-apps.yml                 # Runs tests, triggers CodeBuild or kubectl deploy
│  │  │     └─ ci-infra.yml                # Triggers Terraform/CodeBuild for infra
│  │  └─ gitlab/                            # 🦊 GitLab CI integration
│  │     └─ .gitlab-ci.yml                  # GitLab -> CodeBuild/EKS pipeline
│  │
│  │  # 🚀 CI/CD Flow Summary (AWS-native option):
│  │  #    - Code pushed to GitHub/GitLab
│  │  #    - CodePipeline or webhook triggers CodeBuild
│  │  #    - CodeBuild:
│  │  #         - builds & pushes Docker images to ECR
│  │  #         - applies Terraform for infra changes
│  │  #         - deploys to EKS via kubectl/Helm
│  │  # ❌ No Jenkins required — AWS-native + GitHub/GitLab integration.
│
│  └─ monitoring-logging/                   # 📈 Observability setup
│     ├─ cloudwatch-metrics/               # CloudWatch dashboards, metrics, alarms
│     └─ cloudwatch-logs/                  # Log groups, retention, subscription filters
│
│     # 👁️ All created via Terraform (cloudwatch/ module) + some hand-tuned configs.
│
└─ docs/                                   # 📚 Documentation
   ├─ ARCHITECTURE.md                      # High-level system + AWS architecture diagram
   ├─ DEPLOYMENT.md                        # How to deploy (CodeBuild/CodePipeline/Terraform/EKS)
   ├─ DEVOPS_GUIDE.md                      # Terraform + Ansible + EKS + CI/CD explained
   ├─ MEDIA_PIPELINE.md                    # S3 + CloudFront + playback-service flow
   ├─ DATA_ANALYTICS.md                    # Kinesis/Firehose/S3 → Athena/Redshift
   ├─ SECURITY.md                          # IAM, Secrets Manager, VPC, SGs
   ├─ MONITORING.md                        # CloudWatch metrics/logs/alarms setup
   └─ AWS_INFRA.md                         # Detailed Terraform module reference for AWS
