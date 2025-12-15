# 🔐 Security Design

## Overview

Security is built into every layer of this architecture with **least-privilege IAM**, **encryption**, and **network segmentation**.

---

## Security Layers

| Domain | Cloud Services | Description |
|---------|----------------|-------------|
| **Identity** | IAM, Azure AD, Cloud IAM | Access control and role-based permissions |
| **Secrets** | AWS Secrets Manager, GCP Secret Manager, Key Vault | Store credentials securely |
| **Network** | VPC / VNet / Firewalls | Segmented and secured communication |
| **Data** | CMEK, KMS, DLP | Encryption and protection |
| **App** | JWT, OAuth2 | Secure API communication |

---

## Best Practices

- Use **IAM roles** instead of static keys  
- Apply **least privilege** policies  
- Rotate **API keys** automatically  
- Enable **VPC Service Controls** for sensitive projects  

---

📄 **Next:** [Monitoring & Logging](MONITORING.md)
