<div align="center">

# Russell Viccaro
### Backend Engineer · Data Engineer
`FastAPI` · `Django` · `ETL Pipelines` · `Python`

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat&logo=fastapi&logoColor=white)
![Django](https://img.shields.io/badge/Django-092E20?style=flat&logo=django&logoColor=white)

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

A production-grade FastAPI service with async request handling, JWT auth, Redis caching, background task processing, and PostgreSQL persistence.

```mermaid
graph TB
    subgraph CLIENT["🌐 Client Layer"]
        WEB[Web App / SPA]
        MOB[Mobile Client]
        EXT[External API Consumer]
    end

    subgraph GATEWAY["⚡ API Gateway & Edge"]
        direction LR
        CDN[CloudFront CDN]
        LB[AWS ALB\nLoad Balancer]
        WAF[WAF Rules\nRate Limiting]
    end

    subgraph FASTAPI["🚀 FastAPI Application — Uvicorn / Gunicorn Workers"]
        direction TB
        ROUTER[APIRouter\nversioned endpoints]

        subgraph MW["Middleware Stack"]
            direction LR
            CORS[CORS\nMiddleware]
            AUTH_MW[JWT Auth\nMiddleware]
            LOG_MW[Structured\nLogging]
            TRACE[OpenTelemetry\nTracing]
        end

        subgraph HANDLERS["Request Handlers"]
            direction LR
            V1["/api/v1/\nusers"]
            V2["/api/v1/\norders"]
            V3["/api/v1/\nproducts"]
            WS["/ws/\nWebSocket"]
        end

        subgraph SERVICES["Service Layer"]
            direction LR
            AUTH_SVC[Auth\nService]
            BIZ_SVC[Business\nLogic Service]
            NOTIF_SVC[Notification\nService]
        end

        subgraph DEPS["Dependency Injection"]
            direction LR
            DB_DEP[DB Session\nProvider]
            CACHE_DEP[Cache\nProvider]
            USER_DEP[Current User\nProvider]
        end
    end

    subgraph TASKS["⚙️ Background Workers — Celery + Redis"]
        direction LR
        WORKER1[Email\nWorker]
        WORKER2[Report Gen\nWorker]
        WORKER3[Webhook\nDispatcher]
        BEAT[Celery\nBeat Scheduler]
    end

    subgraph DATA["🗄️ Data & Cache Layer"]
        direction TB
        PG_WRITE[(PostgreSQL\nWrite Replica)]
        PG_READ[(PostgreSQL\nRead Replica)]
        REDIS_CACHE[(Redis\nSession & Cache)]
        REDIS_Q[(Redis\nTask Queue)]
        S3[S3\nObject Store]
    end

    subgraph OBS["📊 Observability"]
        direction LR
        PROM[Prometheus\nMetrics]
        GRAF[Grafana\nDashboards]
        SENTRY[Sentry\nError Tracking]
        LOKI[Loki\nLog Aggregation]
    end

    WEB & MOB & EXT --> CDN --> WAF --> LB
    LB --> ROUTER
    ROUTER --> MW --> HANDLERS
    HANDLERS --> SERVICES
    SERVICES --> DEPS
    DEPS --> DB_DEP & CACHE_DEP & USER_DEP
    DB_DEP --> PG_WRITE & PG_READ
    CACHE_DEP --> REDIS_CACHE
    SERVICES --> REDIS_Q
    REDIS_Q --> WORKER1 & WORKER2 & WORKER3
    BEAT --> REDIS_Q
    WORKER2 --> S3
    HANDLERS --> PROM
    FASTAPI --> LOKI --> GRAF
    FASTAPI --> SENTRY
    PROM --> GRAF

    style CLIENT fill:#1a1a2e,stroke:#4a9eff,color:#e0e0e0
    style GATEWAY fill:#16213e,stroke:#4a9eff,color:#e0e0e0
    style FASTAPI fill:#0f3460,stroke:#4a9eff,color:#e0e0e0
    style TASKS fill:#1a2744,stroke:#7c83fd,color:#e0e0e0
    style DATA fill:#1a2a1a,stroke:#52c788,color:#e0e0e0
    style OBS fill:#2a1a2a,stroke:#c084fc,color:#e0e0e0
```

---

### 2 · Django REST Framework — Multi-Tenant SaaS Backend

A Django-based SaaS backend with tenant isolation, role-based access control, async task queuing, and a pluggable app architecture.

```mermaid
graph TB
    subgraph INGRESS["🔀 Ingress"]
        direction LR
        NGINX[Nginx\nReverse Proxy]
        STATIC[WhiteNoise\nStatic Files]
    end

    subgraph DJANGO["🦄 Django Application — Gunicorn WSGI"]
        direction TB

        subgraph MIDDLEWARE["Django Middleware Chain"]
            direction LR
            SEC[SecurityMiddleware]
            SESS[SessionMiddleware]
            TENANT_MW[TenantMiddleware\nschema routing]
            PERM[PermissionMiddleware\nRBAC checks]
        end

        subgraph APPS["Installed Apps"]
            direction TB
            subgraph CORE_APPS["Core Apps"]
                direction LR
                USERS_APP[users\nCustom User Model]
                TENANT_APP[tenants\nMulti-tenancy]
                PERMS_APP[permissions\nRBAC + Groups]
            end
            subgraph BIZ_APPS["Business Apps"]
                direction LR
                BILLING[billing\nStripe Integration]
                ORDERS[orders\nOrder Lifecycle]
                PRODUCTS[products\nCatalog + Inventory]
                REPORTS[reports\nAggregations]
            end
            subgraph API_APPS["API Layer"]
                direction LR
                DRF[djangorestframework\nSerializers + Views]
                SWAGGER[drf-spectacular\nOpenAPI Schema]
                SIMPLEJWT[SimpleJWT\nAuth Tokens]
            end
        end

        subgraph ORM["Django ORM Layer"]
            direction LR
            MANAGERS[Custom\nManagers]
            SIGNALS[Django\nSignals]
            Q_OBJ[Q Objects\nComplex Queries]
        end

        subgraph ADMIN["Django Admin"]
            direction LR
            ADMIN_SITE[Admin Site\nCustom Actions]
            IMPORT_EXPORT[Import/Export\nCSV/XLSX]
        end
    end

    subgraph CELERY_STACK["⚙️ Async Task System"]
        direction TB
        subgraph QUEUES["Celery Queues"]
            direction LR
            Q_DEFAULT[default\nqueue]
            Q_HIGH[high-priority\nqueue]
            Q_EMAIL[email\nqueue]
            Q_REPORT[reports\nqueue]
        end
        FLOWER[Flower\nTask Monitor]
        CELERY_BEAT[Celery Beat\nCron Scheduler]
    end

    subgraph STORAGE["🗄️ Persistence Layer"]
        direction TB
        subgraph DB_CLUSTER["PostgreSQL Cluster — Tenant Schemas"]
            direction LR
            PUBLIC_SCHEMA[(public\nshared schema)]
            T1_SCHEMA[(tenant_acme\nschema)]
            T2_SCHEMA[(tenant_globex\nschema)]
        end
        PGBOUNCER[PgBouncer\nConnection Pool]
        REDIS[(Redis\nCache + Sessions)]
        MINIO[(MinIO / S3\nMedia & Documents)]
    end

    subgraph EXTERNAL["🌍 External Integrations"]
        direction LR
        STRIPE[Stripe\nPayments]
        SENDGRID[SendGrid\nEmail]
        TWILIO[Twilio\nSMS]
        WEBHOOK_OUT[Outbound\nWebhooks]
    end

    NGINX --> STATIC
    NGINX --> MIDDLEWARE
    MIDDLEWARE --> SEC --> SESS --> TENANT_MW --> PERM
    PERM --> APPS
    APPS --> ORM --> PGBOUNCER --> DB_CLUSTER
    APPS --> REDIS
    APPS --> MINIO
    BILLING --> STRIPE
    BIZ_APPS --> QUEUES
    CELERY_BEAT --> QUEUES
    QUEUES --> FLOWER
    Q_EMAIL --> SENDGRID
    Q_EMAIL --> TWILIO
    Q_REPORT --> MINIO
    ORDERS --> WEBHOOK_OUT

    style INGRESS fill:#1a1a1a,stroke:#ff6b6b,color:#e0e0e0
    style DJANGO fill:#0d2137,stroke:#44b3c2,color:#e0e0e0
    style CELERY_STACK fill:#1f1a2e,stroke:#9b59b6,color:#e0e0e0
    style STORAGE fill:#1a2e1a,stroke:#2ecc71,color:#e0e0e0
    style EXTERNAL fill:#2e1a1a,stroke:#e67e22,color:#e0e0e0
```

---

### 3 · ETL / ELT Data Pipeline Architecture

An end-to-end data pipeline covering ingestion from multiple sources, transformation layers, quality checks, and serving to downstream consumers.

```mermaid
graph LR
    subgraph SOURCES["📥 Data Sources"]
        direction TB
        PG_SRC[(PostgreSQL\nTransactional DB)]
        MYSQL_SRC[(MySQL\nLegacy System)]
        REST_SRC[REST APIs\nThird-party SaaS]
        EVENTS[Kafka Topics\nEvent Streams]
        FILES[S3 / SFTP\nCSV · JSON · Parquet]
        WEBHOOKS[Inbound\nWebhooks]
    end

    subgraph INGEST["⬇️ Ingestion Layer"]
        direction TB
        CDC[Debezium CDC\nChange Data Capture]
        KAFKA[Apache Kafka\nEvent Bus]
        AIRBYTE[Airbyte\nBatch Connectors]
        CUSTOM[Custom Python\nIngestion Scripts]
    end

    subgraph LANDING["🪣 Landing Zone — Raw Storage"]
        direction LR
        RAW_S3[S3 Raw Bucket\nJSON / Avro / CSV]
        DELTA_RAW[Delta Lake\nRaw Tables]
    end

    subgraph ORCHESTRATION["🎯 Orchestration — Apache Airflow"]
        direction TB
        subgraph DAGS["DAG Definitions"]
            direction LR
            DAG_ING[ingestion_dag\ndaily / hourly]
            DAG_TRANS[transformation_dag\npost-ingestion trigger]
            DAG_QC[quality_check_dag\nsla-enforced]
            DAG_EXPORT[export_dag\nscheduled delivery]
        end
        AIRFLOW_SCHEDULER[Airflow Scheduler]
        AIRFLOW_WORKERS[Airflow Workers\nKubernetesPodOperator]
    end

    subgraph TRANSFORM["🔄 Transformation Layer"]
        direction TB
        subgraph SPARK_CLUSTER["Apache Spark Cluster — EMR / Dataproc"]
            direction LR
            SPARK_STREAM[Spark Streaming\nreal-time transforms]
            SPARK_BATCH[Spark Batch\nbulk processing]
            SPARK_ML[Spark MLlib\nfeature engineering]
        end
        subgraph DBT_LAYER["dbt — SQL Transformations"]
            direction LR
            DBT_STAGE[Staging\nModels]
            DBT_INT[Intermediate\nModels]
            DBT_MART[Data Mart\nModels]
        end
        PANDAS[Pandas / Polars\nlight transforms]
    end

    subgraph QUALITY["✅ Data Quality — Great Expectations"]
        direction LR
        SCHEMA_CHK[Schema\nValidation]
        NULL_CHK[Null /\nCompleteness]
        RANGE_CHK[Range &\nUniqueness]
        FRESHNESS[Data\nFreshness SLA]
        ALERTS[PagerDuty\nAlerts]
    end

    subgraph WAREHOUSE["🏛️ Data Warehouse / Lakehouse"]
        direction TB
        SNOWFLAKE[(Snowflake\nCloud Warehouse)]
        subgraph LAYERS["Warehouse Layers"]
            direction LR
            RAW_LAYER[RAW\nschema]
            STAGING_LAYER[STAGING\nschema]
            CORE_LAYER[CORE\nschema]
            MART_LAYER[MART\nschema]
        end
    end

    subgraph SERVING["📤 Serving & Consumption"]
        direction TB
        BI[BI Tools\nTableau · Looker]
        JUPYTER[Jupyter\nData Science]
        REVERSE_ETL[Reverse ETL\nHightouch · Census]
        API_SERVE[FastAPI\nAnalytics API]
        FEATURE_STORE[Feature Store\nML Serving]
    end

    subgraph META["📋 Metadata & Lineage"]
        direction LR
        DATAHUB[DataHub\nData Catalog]
        LINEAGE[dbt Lineage\nDAG Docs]
        COST[Cost Monitor\nQuery Spend]
    end

    PG_SRC & MYSQL_SRC --> CDC --> KAFKA
    REST_SRC & WEBHOOKS --> CUSTOM --> KAFKA
    FILES --> AIRBYTE --> LANDING
    EVENTS --> KAFKA
    KAFKA --> LANDING
    LANDING --> AIRFLOW_SCHEDULER
    AIRFLOW_SCHEDULER --> DAGS
    DAGS --> AIRFLOW_WORKERS
    AIRFLOW_WORKERS --> SPARK_CLUSTER
    AIRFLOW_WORKERS --> DBT_LAYER
    SPARK_STREAM --> QUALITY
    SPARK_BATCH --> QUALITY
    DBT_LAYER --> QUALITY
    PANDAS --> QUALITY
    QUALITY --> ALERTS
    QUALITY --> SNOWFLAKE
    SNOWFLAKE --> LAYERS
    MART_LAYER --> BI & JUPYTER & REVERSE_ETL & API_SERVE & FEATURE_STORE
    SNOWFLAKE --> DATAHUB
    DBT_LAYER --> LINEAGE
    SNOWFLAKE --> COST

    style SOURCES fill:#1a1a2e,stroke:#4a9eff,color:#e0e0e0
    style INGEST fill:#16213e,stroke:#4a9eff,color:#e0e0e0
    style LANDING fill:#1e2a1e,stroke:#52c788,color:#e0e0e0
    style ORCHESTRATION fill:#2e1a1e,stroke:#ff6b9d,color:#e0e0e0
    style TRANSFORM fill:#1e1e2e,stroke:#7c83fd,color:#e0e0e0
    style QUALITY fill:#2a1a1a,stroke:#ff8c42,color:#e0e0e0
    style WAREHOUSE fill:#1a2a2a,stroke:#44b3c2,color:#e0e0e0
    style SERVING fill:#2a2a1a,stroke:#f9ca24,color:#e0e0e0
    style META fill:#1a1a1a,stroke:#888,color:#e0e0e0
```

---

### 4 · Streaming Event Pipeline — Kafka + Flink + ClickHouse

A real-time streaming architecture for high-throughput event processing, aggregation, and analytics delivery with sub-second latency.

```mermaid
graph TB
    subgraph PRODUCERS["📡 Event Producers"]
        direction LR
        APP1[Backend\nMicroservices]
        APP2[Mobile\nSDK Events]
        APP3[IoT / Edge\nDevices]
        APP4[Clickstream\nTracking]
    end

    subgraph KAFKA_CLUSTER["🔀 Apache Kafka Cluster — 6 Brokers"]
        direction TB
        subgraph TOPICS["Topics"]
            direction LR
            T_RAW[raw.events\n24-partition]
            T_USER[user.actions\n12-partition]
            T_ERRORS[dlq.errors\nDead Letter Queue]
        end
        ZK[ZooKeeper /\nKRaft Consensus]
        SR[Schema Registry\nAvro · Protobuf]
    end

    subgraph FLINK["⚡ Apache Flink — Stream Processing"]
        direction TB
        subgraph JOBS["Flink Jobs"]
            direction LR
            JOB_ENRICH[Enrichment Job\nuser + product lookup]
            JOB_DEDUP[Deduplication Job\nbloom filter · 1h window]
            JOB_AGG[Aggregation Job\ntumbling window · 60s]
            JOB_DETECT[Anomaly Detection\nML model inference]
        end
        subgraph STATE["State Management"]
            direction LR
            ROCKSDB[RocksDB\nLocal State]
            CHECKPOINTS[S3 Checkpoints\nfault tolerance]
        end
        FLINK_UI[Flink\nDashboard UI]
    end

    subgraph SINKS["🗄️ Sinks & Storage"]
        direction TB
        CLICKHOUSE[(ClickHouse\nAnalytics OLAP)]
        REDIS_RT[(Redis\nReal-time Counters)]
        ES[(Elasticsearch\nSearch + Logs)]
        S3_SINK[S3 Parquet\nLong-term Archive]
        PG_ENRICH[(PostgreSQL\nEnriched Records)]
    end

    subgraph SERVE["📊 Serving Layer"]
        direction TB
        GRAFANA[Grafana\nReal-time Dashboards]
        SUPERSET[Apache Superset\nAd-hoc Analytics]
        FASTAPI_RT[FastAPI\nReal-time Metrics API]
        ALERT_MGR[AlertManager\nThreshold Alerts]
    end

    APP1 & APP2 & APP3 & APP4 --> T_RAW
    T_RAW --> SR
    T_RAW --> JOB_ENRICH
    JOB_ENRICH --> JOB_DEDUP
    JOB_DEDUP --> T_USER
    T_USER --> JOB_AGG
    T_USER --> JOB_DETECT
    JOB_AGG --> CLICKHOUSE & REDIS_RT
    JOB_DETECT --> ALERT_MGR
    JOB_ENRICH --> PG_ENRICH
    T_ERRORS --> ES
    JOBS --> ROCKSDB --> CHECKPOINTS
    CLICKHOUSE --> GRAFANA & SUPERSET & FASTAPI_RT
    REDIS_RT --> FASTAPI_RT
    ZK --> KAFKA_CLUSTER

    style PRODUCERS fill:#1a2e1a,stroke:#52c788,color:#e0e0e0
    style KAFKA_CLUSTER fill:#2e1a1a,stroke:#e74c3c,color:#e0e0e0
    style FLINK fill:#1a1a2e,stroke:#7c83fd,color:#e0e0e0
    style SINKS fill:#1e2a2a,stroke:#44b3c2,color:#e0e0e0
    style SERVE fill:#2e2a1a,stroke:#f9ca24,color:#e0e0e0
```

---

### 5 · Tech Stack Overview

```mermaid
mindmap
  root((Russell Viccaro\nBackend · Data))
    Backend Frameworks
      FastAPI
        Pydantic v2
        SQLAlchemy async
        Alembic migrations
        Uvicorn / Gunicorn
      Django
        Django REST Framework
        SimpleJWT
        Celery + Beat
        Django ORM
        django-tenants
    Data Engineering
      Pipelines
        Apache Airflow
        Apache Spark
        Apache Flink
        Kafka + Debezium
        dbt Core
        Airbyte
      Warehouses
        Snowflake
        BigQuery
        ClickHouse
        Delta Lake
      Quality
        Great Expectations
        dbt Tests
        Soda Core
    Languages
      Python 3.12
      SQL
      Bash / Shell
      Go basics
      TypeScript basics
    Databases
      PostgreSQL
      MySQL
      Redis
      MongoDB
      Elasticsearch
      ClickHouse
    Cloud & Infra
      AWS
        EC2 · ECS · EKS
        RDS · Aurora
        S3 · Glue · EMR
        Lambda · SQS · SNS
      GCP
        BigQuery
        Dataflow
        Cloud Composer
      Docker
      Kubernetes
      Terraform
      GitHub Actions
    Observability
      Prometheus
      Grafana
      Loki
      Sentry
      OpenTelemetry
      DataDog
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
| **FastAPI** | Async REST APIs, microservices, ML serving endpoints | Uvicorn, Gunicorn, SQLAlchemy async, Alembic, Pydantic v2, python-jose, passlib |
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
