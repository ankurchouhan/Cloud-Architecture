# 🎬 Cloud-Native Streaming Platform (Netflix / Apple TV+ Style)

This project demonstrates a **scalable, production-grade streaming architecture** built on **Google Cloud Platform (GCP)** using a mix of **serverless and compute services**.

## 🚀 Overview

The system simulates a video-on-demand (VOD) platform — similar to Netflix or Apple TV+ — with:
- User authentication and profiles
- Video catalog and metadata APIs
- Media upload, transcoding, and delivery via CDN
- Real-time analytics and recommendations

## 🧩 Architecture Diagram
![GCP](https://img.shields.io/badge/Cloud-Google%20Cloud-blue?logo=googlecloud)
![Architecture](https://img.shields.io/badge/Architecture-Serverless%20%2B%20Compute-orange)
![CI/CD](https://img.shields.io/badge/CI%2FCD-Cloud%20Build%20%2B%20Terraform-brightgreen)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

## 📚 Table of Contents
1. [Overview](#-overview)
2. [Architecture Diagram](#-architecture-diagram)
3. [Infrastructure](#-infrastructure)
4. [Data Flow](#-example-data-flow)
5. [Tech Stack](#-tech-stack)
6. [Setup Guide](#-setup-guide)
7. [Author](#-author)



![architecture-diagram](architecture/high-level-diagram.png)

### Core Components

| Layer | GCP Service | Purpose |
|--------|--------------|----------|
| Auth / API | Cloud Run | Stateless microservices |
| Metadata | Firestore | NoSQL metadata store |
| Billing | Cloud SQL | Relational transactions |
| Transcoding | Transcoder API | Media processing |
| Delivery | Media CDN + Cloud Storage | Global streaming |
| Analytics | Pub/Sub + BigQuery | Event pipeline |
| Recommendations | Vertex AI | ML-driven personalization |

## 🏗️ Infrastructure

- **Terraform** for IaC  
- **Cloud Build** for CI/CD  
- **Artifact Registry** for containers  
- **Cloud Logging / Monitoring** for observability  
- **IAM / Secrets Manager** for security

## 🧠 Key Design Principles

- Event-driven architecture (Pub/Sub)
- Serverless for stateless microservices
- Compute Engine / GKE for stateful heavy workloads
- Multi-region failover design
- Data-driven personalization (Vertex AI)

## 📊 Data Flow

1. User logs in → Cloud Run Auth Service → Firestore
2. User starts playback → Playback Service → signed Media CDN URL
3. Player emits events → Pub/Sub → BigQuery
4. ML model in Vertex AI updates recommendations

## 🧰 Tech Stack

- GCP (Cloud Run, Firestore, BigQuery, Pub/Sub, Media CDN, Transcoder API)
- Python / Go / Node.js (backend)
- React / Next.js (frontend)
- Terraform / Cloud Build (infra + CI/CD)
- Vertex AI (machine learning)

## 🧑‍💻 Author

*Ankur Chouhan* — Cloud Architect / Backend Engineer  
📫 *[LinkedIn / Website / Email]*  
