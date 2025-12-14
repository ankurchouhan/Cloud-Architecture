# Cloud-Native Streaming Platform (Architecture Blueprint)

This repository presents a **production-grade, cloud-native streaming platform** cloud architecture & solutions, engineered on **Google Cloud Platform (GCP)**.  

It exemplifies **enterprise-level system design** blending **serverless scalability**, **stateful compute**, **event-driven data pipelines**, and **machine intelligence** — all orchestrated for **high availability**, **global reach**, and **observability at scale**.

---

![GCP](https://img.shields.io/badge/Cloud-Google%20Cloud-blue?logo=googlecloud)
![Architecture](https://img.shields.io/badge/Architecture-Serverless%20%2B%20Compute-orange)
![CI/CD](https://img.shields.io/badge/CI%2FCD-Cloud%20Build%20%2B%20Terraform-brightgreen)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

---

## 🚀 Executive Overview

The architecture emulates a **modern Video-on-Demand (VOD)** ecosystem, built for **elastic growth, operational resilience, and low-latency media delivery**.  

It demonstrates how a cloud-native enterprise might structure services to achieve:
- **Zero-downtime scalability** — elastic scaling under unpredictable load.
- **Low-latency global distribution** — media served from the nearest edge.
- **Separation of concerns** — stateless microservices + stateful persistence.
- **Observability by design** — end-to-end tracing, metrics, and alerts.
- **ML-powered intelligence** — continuous personalization and analytics.

---

## 🧩 Architectural Philosophy

> “Design systems to **scale linearly**, fail **gracefully**, and recover **autonomously**.”

The architecture follows **polyglot persistence** and **hybrid compute** principles — combining serverless services for stateless workloads and dedicated compute clusters for data-heavy streaming and ML inference.  

It’s composed of **seven cooperating layers**, each optimized for cost, performance, and isolation.

| Layer | Primary GCP Services | Objective |
|--------|-----------------------|------------|
| **Client & Edge** | Cloud CDN, HTTPS Load Balancer, Cloud Armor | Global edge delivery & security |
| **Application APIs** | Cloud Run, Cloud Functions, API Gateway | Stateless, scalable microservices |
| **Media Processing** | Transcoder API, GKE, Cloud Storage | Video ingest, encoding, and packaging |
| **Data Layer** | Firestore, Cloud SQL, Spanner | User data, metadata, transactions |
| **Analytics & Events** | Pub/Sub, Dataflow, BigQuery | Real-time analytics pipeline |
| **Machine Learning** | Vertex AI, BigQuery ML | Personalization & recommendations |
| **Operations & Security** | IAM, Secret Manager, Cloud Monitoring | Governance, visibility, and compliance |

---

### 📊 Final Cost Comparison (Monthly)

| Users | GCP Managed Infrastructure | Hyperscale Custom Infrastructure |
|------:|----------------------------|----------------------------------|
| **1 Million Users** | ~$1.5M / month | ~$300K / month |
| **1 Billion Users** | ~$500M / month | ~$30M – $80M / month |

---

### 🧠 What This Comparison Shows

- **GCP Managed Infrastructure**
  - Fast to build and operate
  - Ideal up to **tens of millions of users**
  - Cost dominated by **bandwidth (CDN + egress)**

- **Hyperscale Custom Infrastructure (Netflix / Apple style)**
  - Requires massive engineering investment
  - Uses **private CDN, ISP peering, custom hardware**
  - Achieves **10–15× lower bandwidth cost** at scale

---

## 🗂️ Repository Structure

A well-structured repository mirrors the system’s modular design. Each directory represents a distinct concern within the ecosystem.

## 🗂️ Folder Overview

📁 **architecture/** — Diagrams & documentation  
  ↳ [gcp-service-map.md](gcp-service-map.md) — Full GCP service catalog and responsibilities  
📁 **backend/** — Microservices (Auth, Catalog, Playback)  
📁 **infra/** — Terraform, CI/CD pipelines, GCP setup  
📁 **data/** — Firestore schemas, Pub/Sub topics, BigQuery SQL  
📁 **notebooks/** — ML and analytics Jupyter notebooks  
📄 **README.md** — Main documentation

```bash
streaming-platform-gcp-architecture/
│
├── README.md
├── architecture/
│   ├── high-level-diagram.png          # Macro system view: edge-to-ML flow
│   ├── serverless-vs-compute.png       # Workload classification
│   └── gcp-service-map.md              # Service catalog and responsibilities
│
├── backend/
│   ├── auth-service/                   # Authentication & token issuance
│   ├── catalog-service/                # Content catalog and metadata API
│   ├── playback-service/               # Playback control and signed URL generation
│   ├── Dockerfile                      # Multi-service container build
│   └── docker-compose.yml              # Local orchestration for testing
│
├── infra/
│   ├── terraform/                      # Infrastructure as Code (modularized)
│   ├── gcp-setup.md                    # Environment bootstrap documentation
│   └── ci-cd-pipeline.yaml             # Cloud Build pipeline definition
│
├── data/
│   ├── firestore-schema.json           # Firestore collection schema
│   ├── pubsub-topics.yaml              # Event topics for analytics and notifications
│   └── bigquery-dataset.sql            # Analytical data model for user activity
│
└── notebooks/
    ├── recommendation_model.ipynb      # Collaborative filtering ML demo
    └── analytics_demo.ipynb            # Audience metrics and engagement trends
