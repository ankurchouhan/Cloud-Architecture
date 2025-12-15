
---

### 🔁 3. `docs/DEPLOYMENT.md`

```markdown
# 🔁 CI/CD Pipelines

## Overview

CI/CD is built using **native tools per cloud provider**, with cross-cloud coordination through GitHub or GitLab.

| Cloud | Build Tool | Deployment Target | Infra Managed By |
|--------|-------------|------------------|------------------|
| **GCP** | Cloud Build | GKE / Cloud Run | Terraform |
| **AWS** | CodeBuild + CodePipeline | EKS / Lambda | Terraform |
| **Azure** | Azure DevOps Pipelines | AKS / Functions | Terraform |

---

## Workflow

1. Developer pushes code → GitHub / GitLab  
2. CI job triggers build and test (via native build service)  
3. Container images are pushed to Artifact Registry / ECR / ACR  
4. Terraform pipeline applies infra changes  
5. Deployment pipeline updates workloads (Helm or kubectl)

---

## Cloud-Native Integrations

- **GCP:** Cloud Build + Cloud Deploy  
- **AWS:** CodePipeline + CodeBuild  
- **Azure:** Azure DevOps Pipelines  

---

📄 **Next:** [DevOps & Config Management](DEVOPS_GUIDE.md)
