# ☁️ GCP-Native Full Architecture  
### (Compute + Serverless + CI/CD + Infrastructure as Code)

This repository defines a **cloud-native video streaming and analytics platform** hosted entirely on **Google Cloud Platform (GCP)**.  
It combines **GKE (Kubernetes)**, **Cloud SQL**, **Pub/Sub**, **Dataflow**, and **BigQuery** to handle scalable compute,  
streaming data, and analytics workloads — all managed through **Terraform**, **Ansible**, and **GCP-native CI/CD** tools  
like **Cloud Build** and **Cloud Deploy**, with optional **GitHub** or **GitLab** integration.

It supports:
- 🧱 **Compute services** — containerized microservices deployed on **Google Kubernetes Engine (GKE)**
- 🌀 **Serverless services** — event-driven workloads via **Cloud Run** and **Cloud Functions**
- ⚙️ **Infrastructure as Code (IaC)** — provisioned and versioned through **Terraform**
- 🔁 **Continuous Integration / Continuous Deployment** — powered by **Cloud Build** and **Cloud Deploy**
- 🔒 **Security, Observability, and Data Analytics** — managed through **IAM**, **Secret Manager**, **Cloud Monitoring**, and **BigQuery / Dataflow**
- ⚒️ **Configuration Management** — handled by **Ansible** for any GCE VM or bastion host

---

```bash
your-project/
├─ docker-compose.yml                       # 🐳 Local dev stack (Postgres, Redis, mock services)
├─ .env                                     # Environment vars for local Docker
├─ .gitignore                               # Ignore local secrets, node_modules, build outputs
├─ README.md                                # Project overview, setup, run instructions
│
├─ services/                                # 🌐 Backend microservices (polyglot)
│  ├─ gateway/                              # Node.js API Gateway (Dockerized, runs in GKE)
│  ├─ auth-service/                         # Python Auth (Flask/FastAPI)
│  ├─ content-service/                      # Go service (content APIs)
│  ├─ billing-service/                      # Java (Spring Boot)
│  ├─ catalog-service/                      # Metadata API
│  ├─ playback-service/                     # Playback URL signing, media serving
│  └─ shared-lib/                           # Shared backend libraries (Node/Python/Go/Java)
│
├─ database/
│  └─ init/init.sql                         # SQL schema (executed on Cloud SQL)
│
├─ data/                                    # 📊 Schemas & specs for GCP data services
│  ├─ firestore-schema.json                 # Firestore collections / indexes (Terraform)
│  ├─ pubsub-topics.yaml                    # Pub/Sub topics + subscriptions (Terraform)
│  └─ bigquery-dataset.sql                  # BigQuery dataset/table DDL (Terraform)
│
├─ redis-data/                              # Local Redis volume for dev (Docker only)
│
├─ frontend/                                # 💻 React frontends (Dockerized, served via GKE + CDN)
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
│  ├─ dev-start.sh                          # Run Docker Compose stack locally
│  ├─ dev-stop.sh                           # Stop local containers
│  ├─ build-all-images.sh                   # Build all Docker images (Docker)
│  ├─ push-all-images.sh                    # Push all images to Artifact Registry (GCP)
│  ├─ gcp-auth.sh                           # gcloud login & set project
│  ├─ gcp-create-infra.sh                   # Terraform wrapper (infra provisioning)
│  ├─ gcp-deploy-gke.sh                     # Deploy Helm charts / manifests to GKE
│  └─ migrate-db.sh                         # Apply DB migrations to Cloud SQL
│
├─ infra/                                   # ☁️ Infrastructure & DevOps (Terraform, Ansible, CI/CD)
│  ├─ terraform/                            # 🧱 Infrastructure as Code — creates ALL GCP resources
│  │  ├─ envs/
│  │  │  ├─ dev/
│  │  │  ├─ staging/
│  │  │  └─ prod/
│  │  │      ├─ main.tf                     # Call infra modules
│  │  │      ├─ variables.tf
│  │  │      ├─ backend.tf                  # GCS backend for TF state
│  │  │      └─ terraform.tfvars            # Env vars (project_id, region, etc.)
│  │  │
│  │  └─ modules/
│  │     ├─ project/                        # Creates GCP project + enables APIs
│  │     ├─ network/                        # Creates VPC, subnets, firewalls
│  │     ├─ gke/                            # Creates GKE cluster + node pools
│  │     ├─ cloud-sql/                      # Creates Cloud SQL (Postgres/MySQL)
│  │     ├─ memorystore/                    # Creates Redis instance
│  │     ├─ firestore/                      # Sets Firestore indexes/config
│  │     ├─ pubsub/                         # Creates Pub/Sub topics + subs
│  │     ├─ storage/                        # Creates GCS buckets (media, logs)
│  │     ├─ bigquery/                       # Creates BigQuery datasets/tables
│  │     ├─ dataflow/                       # Creates Dataflow jobs/templates
│  │     ├─ cloud-cdn-lb/                   # Creates HTTPS LB + Cloud CDN + certs
│  │     ├─ secret-manager/                 # Creates secrets (JWTs, DB creds)
│  │     ├─ iam/                            # Creates IAM roles, bindings
│  │     └─ monitoring/                     # Creates dashboards, alerts (Cloud Monitoring)
│  │
│  │  # 🏗️ Terraform builds and manages EVERYTHING inside GCP:
│  │  #    - VPC, GKE, Cloud SQL, Pub/Sub, BigQuery, GCS, Redis, IAM, Monitoring, etc.
│  │  # ❌ Not responsible for OS configuration, app deployment, or container builds.
│
│  ├─ ansible/                              # ⚒️ Config management (for GCE VMs, not GKE)
│  │  ├─ inventories/
│  │  │  ├─ dev/hosts.ini                   # GCE VM inventory for dev
│  │  │  ├─ staging/hosts.ini
│  │  │  └─ prod/hosts.ini
│  │  ├─ playbooks/
│  │  │  ├─ bootstrap-bastion.yml           # Create users, SSH hardening (Ansible)
│  │  │  ├─ configure-gce-tools.yml         # Install packages/tools on GCE VMs
│  │  │  └─ maintenance.yml                 # System updates, cleanup, cron jobs
│  │  └─ roles/
│  │     ├─ common/                         # OS packages, baseline security
│  │     ├─ app-node/                       # For VM-based apps (if any)
│  │     └─ monitoring-agent/               # Install Cloud Ops agent
│  │
│  │  # ⚙️ Ansible is NOT provisioning infra — it only configures existing VMs.
│  │  # Terraform creates GCE instances, Ansible SSHs in to configure them.
│
│  ├─ kubernetes/                           # ☸️ GKE cluster-level manifests (managed by kubectl/Helm)
│  │  ├─ gke-cluster-config/                # Namespaces, RBAC, Network Policies
│  │  ├─ namespaces/
│  │  │  ├─ gateway.yaml                    # Deploy Gateway
│  │  │  ├─ auth-service.yaml
│  │  │  ├─ content-service.yaml
│  │  │  ├─ billing-service.yaml
│  │  │  ├─ catalog-service.yaml
│  │  │  ├─ playback-service.yaml
│  │  │  ├─ frontend.yaml
│  │  │  └─ cloud-sql-proxy.yaml            # Cloud SQL Auth Proxy sidecar (connect to DB)
│  │  ├─ ingress/ingress.yaml               # GKE Ingress + HTTPS LB (Terraform references)
│  │  ├─ helm/                              # Helm charts for each service
│  │  └─ observability/                     # ConfigMaps for metrics, dashboards
│
│  ├─ cicd/                                 # 🔁 CI/CD pipelines (Cloud Build / Cloud Deploy)
│  │  ├─ cloud-build/
│  │  │  ├─ cloudbuild-apps.yaml            # Cloud Build — builds Docker images, deploys apps
│  │  │  └─ cloudbuild-infra.yaml           # Cloud Build — runs Terraform plan/apply
│  │  ├─ cloud-deploy/
│  │  │  ├─ pipeline.yaml                   # Cloud Deploy pipeline definition (GKE/Cloud Run)
│  │  │  └─ targets/
│  │  │     ├─ dev.yaml                     # Cloud Deploy target for dev
│  │  │     ├─ staging.yaml
│  │  │     └─ prod.yaml
│  │  ├─ github/                            # 🐙 GitHub Actions integration
│  │  │  └─ workflows/
│  │  │     ├─ ci-apps.yml                 # Runs tests, triggers Cloud Build
│  │  │     └─ ci-infra.yml                # Triggers Terraform build via Cloud Build
│  │  └─ gitlab/                            # 🦊 GitLab CI integration
│  │     └─ .gitlab-ci.yml                  # GitLab → Cloud Build / Terraform pipeline
│  │
│  │  # 🚀 CI/CD Flow Summary:
│  │  #    - Code pushed to GitHub/GitLab
│  │  #    - Cloud Build runs CI (tests, builds, push images)
│  │  #    - Cloud Deploy applies to GKE
│  │  #    - Terraform managed infra changes auto-applied via cloudbuild-infra.yaml
│  │  # ❌ Jenkins not required — all GCP-native.
│
│  └─ monitoring-logging/                   # 📈 Observability setup
│     ├─ cloud-monitoring/                  # Dashboards, alerts, uptime checks
│     └─ cloud-logging/                     # Log sinks (to BigQuery, GCS)
│
└─ docs/                                   # 📚 Documentation
   ├─ ARCHITECTURE.md                      # High-level system + GCP architecture diagram
   ├─ DEPLOYMENT.md                        # How to deploy (Cloud Build, Deploy, Terraform)
   ├─ DEVOPS_GUIDE.md                      # Terraform + Ansible + GKE + CI/CD explained
   ├─ MEDIA_PIPELINE.md                    # GCS + CDN + playback-service flow
   ├─ DATA_ANALYTICS.md                    # Pub/Sub → Dataflow → BigQuery pipeline
   ├─ SECURITY.md                          # IAM, Secrets, network security
   ├─ MONITORING.md                        # Cloud Monitoring + Logging setup
   └─ GCP_INFRA.md                         # Detailed Terraform module reference
