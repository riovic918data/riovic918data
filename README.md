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

```mermaid
graph TB
    subgraph CLIENT["Client layer"]
        direction LR
        WEB["Web / SPA"]
        MOB["Mobile client"]
        EXT["External API consumer"]
    end

    subgraph GATEWAY["API gateway & edge"]
        direction LR
        CDN["CloudFront CDN"]
        WAF["WAF + rate limiting"]
        LB["AWS ALB"]
    end

    subgraph FASTAPI["FastAPI application — Uvicorn workers"]
        direction TB

        subgraph MW["Middleware chain"]
            direction LR
            CORS["CORS"]
            JWT["JWT auth"]
            LOG["Structured logging"]
            OTEL["OpenTelemetry"]
        end

        subgraph HANDLERS["Route handlers"]
            direction LR
            R1["/api/v1/users"]
            R2["/api/v1/orders"]
            R3["/api/v1/products"]
            R4["/ws real-time"]
        end

        subgraph SVC["Service layer"]
            direction LR
            S1["Auth service"]
            S2["Business logic"]
            S3["Notification service"]
        end

        subgraph DI["Dependency injection"]
            direction LR
            D1["DB session"]
            D2["Cache provider"]
            D3["Current user"]
        end
    end

    subgraph WORKERS["Background workers — Celery + Redis"]
        direction LR
        W1["Email worker"]
        W2["Report worker"]
        W3["Webhook dispatcher"]
        BEAT["Celery Beat"]
    end

    subgraph DATA["Data layer"]
        direction LR
        PGW[("PostgreSQL write")]
        PGR[("PostgreSQL read")]
        RDS[("Redis cache")]
        RDQ[("Redis queue")]
        S3[("S3 object store")]
    end

    subgraph OBS["Observability"]
        direction LR
        PROM["Prometheus"]
        GRAF["Grafana"]
        SENTRY["Sentry"]
        LOKI["Loki"]
    end

    WEB & MOB & EXT --> CDN --> WAF --> LB
    LB --> MW
    MW --> CORS --> JWT --> LOG --> OTEL
    OTEL --> HANDLERS
    HANDLERS --> SVC --> DI
    DI --> D1 & D2 & D3
    D1 --> PGW & PGR
    D2 --> RDS
    SVC --> RDQ
    RDQ --> W1 & W2 & W3
    BEAT --> RDQ
    W2 --> S3
    HANDLERS --> PROM --> GRAF
    FASTAPI --> LOKI --> GRAF
    FASTAPI --> SENTRY

    style CLIENT   fill:#1e1b4b,stroke:#818cf8,color:#e0e7ff
    style GATEWAY  fill:#0c1a2e,stroke:#38bdf8,color:#e0f2fe
    style FASTAPI  fill:#0f2840,stroke:#60a5fa,color:#dbeafe
    style MW       fill:#1e3a5f,stroke:#93c5fd,color:#dbeafe
    style HANDLERS fill:#1e3a5f,stroke:#93c5fd,color:#dbeafe
    style SVC      fill:#1e3a5f,stroke:#93c5fd,color:#dbeafe
    style DI       fill:#1e3a5f,stroke:#93c5fd,color:#dbeafe
    style WORKERS  fill:#1c1917,stroke:#f59e0b,color:#fef3c7
    style DATA     fill:#052e16,stroke:#4ade80,color:#dcfce7
    style OBS      fill:#2d1b00,stroke:#fb923c,color:#ffedd5
```

---

### 2 · Django REST Framework — Multi-Tenant SaaS Backend

Schema-per-tenant PostgreSQL isolation, RBAC middleware, pluggable installed apps split across Core / Business / API layers, Celery task queues, and external service integrations.

```mermaid
graph TB
    NGINX["Nginx reverse proxy"]

    subgraph MW["Django middleware chain"]
        direction LR
        SEC["Security"]
        SESS["Session"]
        TEN["TenantMiddleware"]
        PERM["RBAC perms"]
    end

    subgraph APPS["Installed apps — Gunicorn WSGI"]
        direction TB

        subgraph CORE["Core apps"]
            direction LR
            USR["users"]
            TNT["tenants"]
            ACL["permissions"]
        end

        subgraph BIZ["Business apps"]
            direction LR
            BILL["billing"]
            ORD["orders"]
            PROD["products"]
            REP["reports"]
        end

        subgraph API["API layer"]
            direction LR
            DRF["DRF"]
            SPEC["drf-spectacular"]
            SJWT["SimpleJWT"]
            STR["django-storages"]
        end
    end

    subgraph PERSIST["Persistence"]
        direction LR
        PGB["PgBouncer"]
        subgraph SCHEMAS["PostgreSQL — tenant schemas"]
            direction LR
            PUB[("public")]
            TA[("tenant_acme")]
            TN[("tenant_n")]
        end
        RDS[("Redis")]
        MNO[("S3 / MinIO")]
    end

    subgraph CEL["Celery + Redis queues"]
        direction LR
        QD["default"]
        QH["high-priority"]
        QE["email"]
        QB["Celery Beat"]
        FLW["Flower monitor"]
    end

    subgraph EXT["External integrations"]
        direction LR
        STRIPE["Stripe"]
        SG["SendGrid"]
        TW["Twilio"]
        WH["Outbound webhooks"]
    end

    NGINX --> MW
    SEC --> SESS --> TEN --> PERM --> APPS
    APPS --> PGB --> SCHEMAS
    APPS --> RDS
    APPS --> MNO
    BIZ --> QD & QH & QE
    QB --> QD
    QD & QH & QE --> FLW
    BILL --> STRIPE
    QE --> SG & TW
    ORD --> WH

    style MW      fill:#0f172a,stroke:#818cf8,color:#e0e7ff
    style APPS    fill:#0c1a2e,stroke:#38bdf8,color:#e0f2fe
    style CORE    fill:#1e3a5f,stroke:#93c5fd,color:#dbeafe
    style BIZ     fill:#1e3a5f,stroke:#93c5fd,color:#dbeafe
    style API     fill:#1e3a5f,stroke:#93c5fd,color:#dbeafe
    style PERSIST fill:#052e16,stroke:#4ade80,color:#dcfce7
    style SCHEMAS fill:#065f46,stroke:#6ee7b7,color:#d1fae5
    style CEL     fill:#1c1917,stroke:#f59e0b,color:#fef3c7
    style EXT     fill:#3b0764,stroke:#c084fc,color:#f3e8ff
```

---

### 3 · ETL / ELT Data Pipeline Architecture

End-to-end batch and streaming pipeline: 6 source types → Kafka/Airbyte ingestion → Airflow DAG orchestration → Spark + dbt transformation → Great Expectations quality → Snowflake warehouse → BI / reverse-ETL serving.

```mermaid
graph TB
    subgraph SRC["Data sources"]
        direction LR
        PG[("PostgreSQL")]
        MY[("MySQL")]
        REST["REST APIs"]
        KFK["Kafka events"]
        S3I["S3 / SFTP files"]
        WBH["Webhooks"]
    end

    subgraph ING["Ingestion layer"]
        direction LR
        CDC["Debezium CDC"]
        BUS["Apache Kafka"]
        AIR["Airbyte"]
        PY["Custom Python"]
    end

    subgraph ORCH["Orchestration — Apache Airflow"]
        direction LR
        D1["ingestion_dag"]
        D2["transformation_dag"]
        D3["quality_check_dag"]
        D4["export_dag"]
    end

    subgraph TRANS["Transformation — Spark + dbt"]
        direction LR
        SS["Spark Streaming"]
        SB["Spark Batch"]
        DS["dbt staging"]
        DI["dbt intermediate"]
        DM["dbt mart"]
        PD["Pandas / Polars"]
    end

    subgraph QC["Data quality — Great Expectations"]
        direction LR
        SCH["Schema check"]
        NUL["Null / complete"]
        RNG["Range + unique"]
        FRH["Freshness SLA"]
        ALT["Alerts"]
    end

    subgraph WH["Data warehouse — Snowflake"]
        direction LR
        RAW[["RAW schema"]]
        STG[["STAGING schema"]]
        COR[["CORE schema"]]
        MRT[["MART schema"]]
    end

    subgraph SERVE["Serving & consumption"]
        direction LR
        BI["BI / Looker"]
        JUP["Jupyter"]
        RETL["Reverse ETL"]
        FAPI["FastAPI analytics"]
        FS["Feature store"]
    end

    PG & MY --> CDC --> BUS
    REST & WBH --> PY --> BUS
    S3I --> AIR
    KFK --> BUS
    BUS & AIR --> ORCH
    ORCH --> TRANS
    SS & SB & DS & DI & DM & PD --> QC
    QC --> ALT
    QC --> RAW --> STG --> COR --> MRT
    MRT --> BI & JUP & RETL & FAPI & FS

    style SRC   fill:#1e1b4b,stroke:#818cf8,color:#e0e7ff
    style ING   fill:#042f2e,stroke:#2dd4bf,color:#ccfbf1
    style ORCH  fill:#0c1a2e,stroke:#38bdf8,color:#e0f2fe
    style TRANS fill:#1c1917,stroke:#f59e0b,color:#fef3c7
    style QC    fill:#450a0a,stroke:#f87171,color:#fee2e2
    style WH    fill:#052e16,stroke:#4ade80,color:#dcfce7
    style SERVE fill:#2d1b00,stroke:#fb923c,color:#ffedd5
```

---

### 4 · Streaming Event Pipeline — Kafka + Flink + ClickHouse

Sub-second real-time pipeline processing 500k+ events/minute: producers → Kafka topics with Schema Registry → Flink jobs (enrich, dedup, aggregate, anomaly detect) with RocksDB state → ClickHouse OLAP + Redis counters → Grafana dashboards.

```mermaid
graph TB
    subgraph PROD["Event producers"]
        direction LR
        BE["Backend services"]
        MOB["Mobile SDK"]
        IOT["IoT / Edge"]
        CS["Clickstream tracker"]
    end

    subgraph KFK["Apache Kafka — 6 brokers · KRaft"]
        direction TB
        subgraph TOPICS["Topics"]
            direction LR
            TR["raw.events  ·  24 partitions"]
            TU["user.actions  ·  12 partitions"]
            TD["dlq.errors  ·  dead letter"]
        end
        SR["Schema Registry — Avro"]
    end

    subgraph FLINK["Apache Flink — stream processing"]
        direction TB
        subgraph JOBS["Flink jobs"]
            direction LR
            JE["Enrichment job"]
            JD["Deduplication job"]
            JA["Aggregation job"]
            JM["Anomaly detection"]
        end
        subgraph STATE["State management"]
            direction LR
            RDB["RocksDB local state"]
            CKP["S3 checkpoints"]
        end
    end

    subgraph SINKS["Sinks & storage"]
        direction LR
        CH[("ClickHouse OLAP")]
        RD[("Redis counters")]
        ES[("Elasticsearch")]
        PGE[("PostgreSQL enriched")]
        S3P[("S3 Parquet archive")]
    end

    subgraph SERVE["Serving layer"]
        direction LR
        GRAF["Grafana dashboards"]
        SUP["Apache Superset"]
        FAPI["FastAPI metrics API"]
        ALRT["AlertManager"]
    end

    BE & MOB & IOT & CS --> TR
    TR --> SR
    TR --> JE --> JD --> TU
    TU --> JA & JM
    JA --> CH & RD
    JM --> ALRT
    JE --> PGE
    TD --> ES
    JOBS --> RDB --> CKP
    CH --> GRAF & SUP & FAPI
    RD --> FAPI

    style PROD   fill:#042f2e,stroke:#2dd4bf,color:#ccfbf1
    style KFK    fill:#450a0a,stroke:#f87171,color:#fee2e2
    style TOPICS fill:#7f1d1d,stroke:#fca5a5,color:#fee2e2
    style FLINK  fill:#1e1b4b,stroke:#818cf8,color:#e0e7ff
    style JOBS   fill:#312e81,stroke:#a5b4fc,color:#e0e7ff
    style STATE  fill:#312e81,stroke:#a5b4fc,color:#e0e7ff
    style SINKS  fill:#052e16,stroke:#4ade80,color:#dcfce7
    style SERVE  fill:#1c1917,stroke:#f59e0b,color:#fef3c7
```

---

### 5 · Full Tech Stack Map

```mermaid
mindmap
  root((Russell Viccaro\nBackend · Data))
    Backend Frameworks
      FastAPI
        Pydantic v2
        SQLAlchemy async
        Alembic migrations
        Uvicorn / Gunicorn
        slowapi rate limiting
      Django
        Django REST Framework
        SimpleJWT
        Celery + Beat
        django-tenants
        drf-spectacular
        django-storages
      Flask
        Flask-RESTful
        Lightweight tooling
    Data Engineering
      Orchestration
        Apache Airflow 2.x
        Prefect
        Dagster
      Batch
        PySpark
        dbt Core
        Pandas / Polars
        Delta Lake
      Streaming
        Apache Flink
        Kafka Streams
        Spark Structured Streaming
      Ingestion
        Apache Kafka
        Debezium CDC
        Airbyte
        AWS DMS
      Quality
        Great Expectations
        dbt Tests
        Soda Core
    Warehouses
      Snowflake
      Google BigQuery
      ClickHouse
      AWS Redshift
    Databases
      PostgreSQL
      MySQL
      Redis
      MongoDB
      Elasticsearch
      DynamoDB
    Languages
      Python 3.12
        asyncio
        mypy
        ruff / black
        pytest
      SQL
        PostgreSQL dialect
        BigQuery SQL
        Snowflake SQL
      Bash / Shell
      Go basics
    Cloud & Infra
      AWS
        ECS · EKS
        RDS · Aurora
        S3 · Glue · EMR
        Lambda · SQS · SNS
      GCP
        BigQuery
        Cloud Composer
        Dataflow
        Cloud Run
      Docker · Kubernetes
      Terraform · AWS CDK
      GitHub Actions · ArgoCD
    Observability
      Prometheus
      Grafana
      Loki
      Sentry
      OpenTelemetry
      DataDog APM
      Airflow UI · Flower
```

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
