# 🎬 Production Base; Cloud-Native Streaming Platform (Netflix / Apple TV)

This project demonstrates a **scalable, production-grade streaming architecture** built on **Google Cloud Platform (GCP)** using a mix of **serverless and compute services**.

![GCP](https://img.shields.io/badge/Cloud-Google%20Cloud-blue?logo=googlecloud)
![Architecture](https://img.shields.io/badge/Architecture-Serverless%20%2B%20Compute-orange)
![CI/CD](https://img.shields.io/badge/CI%2FCD-Cloud%20Build%20%2B%20Terraform-brightgreen)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

---

## 🚀 Overview

This system simulates a **video-on-demand (VOD) platform** — similar to Netflix or Apple TV+ — built on GCP with:
- 🔐 **User authentication** and profiles
- 🎞️ **Video catalog & metadata APIs**
- ☁️ **Media upload, transcoding, and CDN delivery**
- 📊 **Real-time analytics and ML-based recommendations**

---

## 🗂️ Folder Overview

📁 **architecture/** — Diagrams & documentation  
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
│   ├── high-level-diagram.png          # System overview diagram
│   ├── serverless-vs-compute.png       # Comparison of workloads
│   └── gcp-service-map.md              # GCP services used and roles
│
├── backend/
│   ├── auth-service/                   # Authentication microservice
│   ├── catalog-service/                # Content & metadata APIs
│   ├── playback-service/               # Generates signed playback URLs
│   ├── Dockerfile                      # Container build file
│   └── docker-compose.yml              # Local service orchestration
│
├── infra/
│   ├── terraform/                      # Infrastructure as code (IaC)
│   ├── gcp-setup.md                    # Setup & deployment guide
│   └── ci-cd-pipeline.yaml             # Cloud Build CI/CD pipeline
│
├── data/
│   ├── firestore-schema.json           # Firestore schema definition
│   ├── pubsub-topics.yaml              # Event topics configuration
│   └── bigquery-dataset.sql            # Analytics schema for BigQuery
│
└── notebooks/
    ├── recommendation_model.ipynb      # Vertex AI recommendation demo
    └── analytics_demo.ipynb            # Data insights visualization
