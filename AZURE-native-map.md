# ☁️ Azure-Native Full Architecture  
### (Compute + Serverless + CI/CD + Infrastructure as Code)

This repository defines a **cloud-native video streaming and analytics platform** fully deployed on **Microsoft Azure**.  
It combines **Azure Kubernetes Service (AKS)**, **Azure SQL**, **Event Hubs**, and **Azure Functions** for scalable compute and serverless workloads.  
Infrastructure and deployment are fully automated via **Terraform**, **Ansible**, and **Azure DevOps Pipelines** (or optionally **GitHub Actions**).

It supports:
- 🧱 **Compute services** — containerized microservices deployed on **Azure Kubernetes Service (AKS)**
- 🌀 **Serverless services** — event-driven processing via **Azure Functions**
- ⚙️ **Infrastructure as Code (IaC)** — provisioned and version-controlled using **Terraform**
- 🔁 **Continuous Integration / Continuous Deployment** — automated via **Azure DevOps Pipelines** or **GitHub Actions**
- 🔒 **Security, Monitoring, and Analytics** — powered by **Azure Active Directory**, **Key Vault**, **Azure Monitor**, and **Synapse Analytics**
- ⚒️ **Configuration Management** — handled by **Ansible** for VM-based automation (bastions, CI runners, etc.)

---

```bash
your-project/
├─ docker-compose.yml                        # 🐳 Local dev stack (Postgres, Redis, mock services)
├─ .env                                      # Environment vars for local Docker
├─ .gitignore                                # Ignore secrets, build outputs, node_modules
├─ README.md                                 # Overview and setup instructions
│
├─ services/                                 # 🌐 Core backend microservices (polyglot)
│  ├─ gateway/                               # Node.js API Gateway (runs on AKS)
│  ├─ auth-service/                          # Python Auth (Flask/FastAPI)
│  ├─ content-service/                       # Go-based content API
│  ├─ billing-service/                       # Java (Spring Boot)
│  ├─ catalog-service/                       # Metadata API
│  ├─ playback-service/                      # Playback token signing / media access
│  └─ shared-lib/                            # Shared backend libs (Node/Python/Go/Java)
│
├─ database/
│  └─ init/init.sql                          # Schema executed on Azure SQL Database
│
├─ data/                                     # 📊 Schemas & data specs for Azure data services
│  ├─ cosmos-schema.json                     # Cosmos DB logical schema
│  ├─ eventhub-topics.yaml                   # Event Hubs topics & consumers
│  ├─ synapse-schema.sql                     # Synapse Analytics or SQL pool schema
│  └─ storage-containers.yaml                # Blob Storage container structure
│
├─ redis-data/                               # Local Redis volume (dev only)
│
├─ frontend/                                 # 💻 React frontends (served via AKS + Front Door CDN)
│  ├─ users/                                 # User-facing portal
│  ├─ team/                                  # Content management dashboard
│  ├─ dev/                                   # Developer tools console
│  └─ admin/                                 # Admin operations panel
│
├─ shared/                                   # 🎨 Shared frontend logic
│  ├─ ui/
│  ├─ hooks/
│  └─ utils/
│
├─ scripts/                                  # ⚙️ Helper scripts (local & cloud automation)
│  ├─ dev-start.sh                           # Start local stack with Docker Compose
│  ├─ dev-stop.sh                            # Stop local containers
│  ├─ build-all-images.sh                    # Build Docker images for all services
│  ├─ push-all-images.sh                     # Push all images to Azure Container Registry (ACR)
│  ├─ azure-auth.sh                          # az login, set subscription
│  ├─ azure-create-infra.sh                  # Terraform wrapper (infra provisioning)
│  ├─ azure-deploy-aks.sh                    # Deploy Helm charts to AKS
│  └─ migrate-db.sh                          # Apply schema migrations to Azure SQL
│
├─ infra/                                    # ☁️ Infrastructure, Configuration, CI/CD
│  ├─ terraform/                             # 🧱 Terraform IaC for Azure resources
│  │  ├─ envs/
│  │  │  ├─ dev/
│  │  │  ├─ staging/
│  │  │  └─ prod/
│  │  │      ├─ main.tf                      # Calls modular components
│  │  │      ├─ variables.tf
│  │  │      ├─ backend.tf                   # Azure Storage backend for Terraform state
│  │  │      └─ terraform.tfvars             # Env-specific configuration
│  │  │
│  │  └─ modules/
│  │     ├─ project/                         # Resource group, tagging, policies
│  │     ├─ vnet/                            # Virtual Network, subnets, NSGs
│  │     ├─ aks/                             # AKS cluster + node pools
│  │     ├─ sql-database/                    # Azure SQL server and database
│  │     ├─ redis-cache/                     # Azure Cache for Redis
│  │     ├─ storage-account/                 # Blob storage (media, logs)
│  │     ├─ event-hubs/                      # Event Hubs / Kafka topics
│  │     ├─ functions/                       # Azure Functions (serverless endpoints)
│  │     ├─ front-door-or-appgw/             # Azure Front Door / App Gateway + CDN
│  │     ├─ cosmosdb/                        # NoSQL store for metadata
│  │     ├─ key-vault/                       # Key Vault for secrets, certs
│  │     ├─ synapse/                         # Synapse Analytics (ETL & reporting)
│  │     ├─ monitor/                         # Azure Monitor alerts & dashboards
│  │     └─ iam/                             # Managed identities, roles, access policies
│  │
│  │  # 🧱 Terraform creates and manages:
│  │  #    - VNET, AKS, SQL, Redis, Storage, Functions, Event Hubs, CosmosDB, Key Vault
│  │  # ❌ No app builds or container deployment — handled by CI/CD pipelines.
│
│  ├─ ansible/                               # ⚒️ Config management (for Azure VMs)
│  │  ├─ inventories/
│  │  │  ├─ dev/hosts.ini
│  │  │  ├─ staging/hosts.ini
│  │  │  └─ prod/hosts.ini
│  │  ├─ playbooks/
│  │  │  ├─ bootstrap-bastion.yml            # Create users, SSH, harden bastion VMs
│  │  │  ├─ configure-vm-tools.yml           # Install devops tooling or monitoring agents
│  │  │  └─ maintenance.yml                  # OS patching, cleanup
│  │  └─ roles/
│  │     ├─ common/
│  │     ├─ devops-agent/
│  │     └─ monitor-agent/
│  │
│  │  # ⚙️ Ansible only configures OS / tooling on Azure VMs.
│  │  # Terraform provisions VMs; Ansible applies configuration via SSH or Azure Run Command.
│
│  ├─ kubernetes/                            # ☸️ AKS manifests and Helm charts
│  │  ├─ namespaces/
│  │  │  ├─ gateway.yaml
│  │  │  ├─ auth-service.yaml
│  │  │  ├─ content-service.yaml
│  │  │  ├─ billing-service.yaml
│  │  │  ├─ catalog-service.yaml
│  │  │  ├─ playback-service.yaml
│  │  │  ├─ frontend.yaml
│  │  │  └─ sql-proxy.yaml                   # Azure SQL connection proxy config
│  │  ├─ ingress/ingress.yaml                # Nginx / App Gateway ingress for services
│  │  ├─ helm/                               # Helm charts for app components
│  │  └─ observability/                      # Prometheus/Grafana setup
│
│  ├─ cicd/                                  # 🔁 CI/CD Pipelines
│  │  ├─ azure-pipelines/                    # Azure DevOps YAML pipelines
│  │  │  ├─ azure-pipelines-apps.yml         # Build, test, deploy to AKS
│  │  │  └─ azure-pipelines-infra.yml        # Terraform plan/apply for infra
│  │  ├─ github/                             # GitHub Actions integration (optional)
│  │  │  └─ workflows/
│  │  │     ├─ ci-apps.yml                   # Runs tests, triggers ACR + AKS deployment
│  │  │     └─ ci-infra.yml                  # Terraform infra deployment via Azure CLI
│  │  └─ gitlab/                             # GitLab CI integration (optional)
│  │     └─ .gitlab-ci.yml                   # GitLab → Azure DevOps / Terraform pipeline
│
│  │  # 🚀 Azure-native CI/CD Flow:
│  │  #    - Push to GitHub or Azure Repos
│  │  #    - Azure Pipelines (builds Docker, pushes to ACR)
│  │  #    - Deploys via Helm to AKS
│  │  #    - Terraform infra managed through dedicated pipeline
│  │  #    - Optional GitHub Actions or Jenkins integration
│
│  └─ monitoring-logging/                    # 📈 Monitoring & Observability
│     ├─ azure-monitor/                      # Workbooks, alerts, metrics
│     ├─ log-analytics/                      # Log Analytics workspace + queries
│     └─ app-insights/                       # Application Insights for tracing
│
└─ docs/                                    # 📚 Documentation
   ├─ ARCHITECTURE.md                       # Azure architecture overview
   ├─ DEPLOYMENT.md                         # How to deploy (Terraform + AKS + Azure Pipelines)
   ├─ DEVOPS_GUIDE.md                       # Terraform + Ansible + AKS + CI/CD overview
   ├─ MEDIA_PIPELINE.md                     # Blob Storage + CDN + playback-service
   ├─ DATA_ANALYTICS.md                     # Event Hubs → Synapse → Power BI
   ├─ SECURITY.md                           # Key Vault, IAM, network security
   ├─ MONITORING.md                         # Azure Monitor + App Insights
   └─ AZURE_INFRA.md                        # Detailed Terraform module reference
