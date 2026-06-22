<div align="center">

# Russell Viccaro
### Backend Engineer · Data Engineer
`FastAPI` · `Django` · `ETL Pipelines` · `Python`

![Python](https://img.shields.io/badge/Python_3.12-3776AB?style=flat&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat&logo=fastapi&logoColor=white)
![Django](https://img.shields.io/badge/Django-092E20?style=flat&logo=django&logoColor=white)
![Apache Airflow](https://img.shields.io/badge/Airflow-017CEE?style=flat&logo=apacheairflow&logoColor=white)
![Apache Spark](https://img.shields.io/badge/Spark-E25A1C?style=flat&logo=apachespark&logoColor=white)
![Kafka](https://img.shields.io/badge/Kafka-231F20?style=flat&logo=apachekafka&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat&logo=postgresql&logoColor=white)
![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8?style=flat&logo=snowflake&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat&logo=amazonaws&logoColor=white)
![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=flat&logo=terraform&logoColor=white)

</div>

---

## About Me

I'm a **Backend & Data Engineer** based in the United States, specializing in high-performance REST APIs, asynchronous microservices, and large-scale ETL/ELT data pipelines. I design systems that handle millions of events per day — from ingestion through transformation to analytical delivery — with a focus on reliability, observability, and clean architecture.

- 🔧 Building production APIs with **FastAPI** and **Django REST Framework**
- 🔄 Designing and operating **ETL / ELT pipelines** at scale (batch & streaming)
- ☁️ Cloud-native infrastructure on **AWS** and **GCP**
- 📊 Enabling data teams with warehouse-first architectures using **dbt**, **Airflow**, and **Spark**
- 🧪 Advocate for test-driven development, typed Python, and CI/CD automation

---

## Architecture Diagrams

### 1 · FastAPI Microservice Architecture

A production-grade FastAPI service with async request handling, JWT auth, Redis caching, Celery background workers, and PostgreSQL persistence — deployed behind CloudFront and AWS ALB.

![FastAPI Microservice Architecture](./FastAPI%20Microservice%20Architecture.png)

---

### 2 · Django REST Framework — Multi-Tenant SaaS Backend

Schema-per-tenant PostgreSQL isolation, RBAC middleware, pluggable installed apps split across Core / Business / API layers, Celery task queues, and external service integrations.

![Django REST Framework - Multi-Tenant SaaS Backend](./Django%20REST%20Framework%20-%20Multi-Tenant%20SaaS%20Backend.png)

---

### 3 · ETL / ELT Data Pipeline Architecture

End-to-end batch and streaming pipeline: 6 source types → Kafka/Airbyte ingestion → Airflow DAG orchestration → Spark + dbt transformation → Great Expectations quality → Snowflake warehouse → BI / reverse-ETL serving.

![ETL ELT Data Pipeline Architecture](./ETL%20ELT%20Data%20Pipeline%20Architecture.png)

---

### 4 · Streaming Event Pipeline — Kafka + Flink + ClickHouse

Sub-second real-time pipeline processing 500k+ events/minute: producers → Kafka topics with Schema Registry → Flink jobs (enrich, dedup, aggregate, anomaly detect) with RocksDB state → ClickHouse OLAP + Redis counters → Grafana dashboards.

![Streaming Data Pipeline Architecture](./Streaming%20data%20piepline%20architecture.png)

---

### 5 · Full Tech Stack Map

![Tech Stack](./Tech%20Stack.png)

---

## Technology Stack

### Languages & Runtimes

| Category | Technologies |
|---|---|
| **Primary** | Python 3.12, SQL (PostgreSQL dialect, BigQuery SQL, Snowflake SQL) |
| **Secondary** | Bash / Shell, Go (basics), TypeScript (basics) |
| **Python Ecosystem** | asyncio, typing, pydantic, dataclasses, mypy, black, ruff |

### Backend Frameworks

| Framework | Use Case | Key Libraries |
|---|---|---|
| **FastAPI** | Async REST APIs, microservices, ML serving endpoints | Uvicorn, Gunicorn, SQLAlchemy async, Alembic, Pydantic v2, python-jose, passlib, slowapi |
| **Django** | Full-stack SaaS apps, admin-heavy platforms, multi-tenant systems | DRF, drf-spectacular, SimpleJWT, Celery, django-tenants, django-storages |
| **Flask** | Lightweight internal tools, simple webhook receivers | Flask-RESTful, Flask-SQLAlchemy |

### Data Engineering

| Layer | Technologies |
|---|---|
| **Orchestration** | Apache Airflow 2.x, Prefect, Dagster |
| **Batch Processing** | Apache Spark (PySpark), dbt Core, Pandas, Polars |
| **Stream Processing** | Apache Flink, Kafka Streams, Spark Structured Streaming |
| **Message Bus** | Apache Kafka, AWS SQS/SNS, Redis Streams |
| **CDC / Replication** | Debezium, AWS DMS, Airbyte |
| **Transformation** | dbt (staging → intermediate → marts), custom PySpark jobs |
| **Data Quality** | Great Expectations, dbt Tests, Soda Core |

### Databases & Storage

| Type | Technologies |
|---|---|
| **Relational** | PostgreSQL (primary), MySQL, SQLite (testing) |
| **OLAP / Warehouse** | Snowflake, Google BigQuery, ClickHouse, AWS Redshift |
| **Lakehouse** | Delta Lake, Apache Iceberg, AWS Glue Data Catalog |
| **Cache / KV** | Redis, Memcached |
| **Search** | Elasticsearch, OpenSearch |
| **Document** | MongoDB, DynamoDB |
| **Object Store** | AWS S3, GCS, MinIO |

### Cloud & Infrastructure

| Category | Technologies |
|---|---|
| **AWS** | EC2, ECS, EKS, RDS/Aurora, S3, Glue, EMR, Lambda, SQS, SNS, CloudWatch, IAM |
| **GCP** | BigQuery, Cloud Composer (Airflow), Dataflow, Cloud Run, GCS |
| **Containers** | Docker, Docker Compose, Kubernetes (EKS/GKE), Helm |
| **IaC** | Terraform, AWS CDK, CloudFormation |
| **CI/CD** | GitHub Actions, GitLab CI, ArgoCD, Jenkins |

### Observability & Monitoring

| Tool | Purpose |
|---|---|
| **Prometheus** | Metrics collection and alerting rules |
| **Grafana** | Dashboards — infrastructure, pipeline health, API latency |
| **Loki** | Log aggregation and querying |
| **Sentry** | Error tracking and performance monitoring |
| **OpenTelemetry** | Distributed tracing across microservices |
| **DataDog** | APM and full-stack observability (enterprise projects) |
| **Airflow UI / Flower** | Pipeline and task monitoring |

### API Design & Tooling

| Concern | Approach / Tool |
|---|---|
| **API Style** | REST (primary), GraphQL (selective), WebSocket (real-time) |
| **Documentation** | OpenAPI / Swagger (drf-spectacular, FastAPI native) |
| **Authentication** | JWT (SimpleJWT, python-jose), OAuth2, API Keys |
| **Rate Limiting** | Redis-backed token bucket, slowapi (FastAPI) |
| **Serialization** | Pydantic v2, DRF Serializers, marshmallow |
| **Testing** | pytest, pytest-asyncio, factory_boy, Hypothesis, httpx |

---

## Featured Projects

### 🚀 High-Throughput Event Ingestion Pipeline
Kafka → Flink → ClickHouse pipeline processing **500k+ events/minute** with real-time anomaly detection and sub-second dashboard latency. Built deduplication using bloom filters and stateful windowing with RocksDB-backed Flink state.

**Stack:** Python · Apache Kafka · Apache Flink · ClickHouse · Redis · Grafana

---

### 🔄 Multi-Source ETL Platform
Airflow-orchestrated ETL platform ingesting from 12 source systems into Snowflake, serving a 15-person analytics team. Introduced dbt for SQL transformation governance and Great Expectations for automated data quality SLAs.

**Stack:** Python · Apache Airflow · dbt · Snowflake · Great Expectations · AWS S3 · Airbyte

---

### ⚡ FastAPI SaaS Backend
Async REST API backend for a B2B SaaS product — 100k+ daily active users. Implemented async SQLAlchemy with connection pooling, Redis cache-aside pattern, and Celery for background report generation. P99 latency under 120ms.

**Stack:** FastAPI · PostgreSQL · Redis · Celery · AWS ECS · Terraform · GitHub Actions

---

### 🏢 Django Multi-Tenant Platform
PostgreSQL schema-per-tenant Django application supporting 80+ enterprise tenants. Designed custom `TenantMiddleware` for schema routing, role-based permission system, and Stripe billing integration with webhook handling.

**Stack:** Django · DRF · PostgreSQL · Celery · Stripe · Docker · AWS RDS

---

## GitHub Stats

<div align="center">

![GitHub Stats](https://github-readme-stats.vercel.app/api?username=russellviccaro&show_icons=true&theme=tokyonight&hide_border=true&count_private=true)
![Top Languages](https://github-readme-stats.vercel.app/api/top-langs/?username=russellviccaro&layout=compact&theme=tokyonight&hide_border=true)

</div>

---

<div align="center">
<sub>Built with Python, powered by coffee ☕</sub>
</div>
