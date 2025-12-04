---

# **LedgerFlow — Real-Time Banking Data Platform**

*LedgerFlow is an end-to-end real-time financial data pipeline that captures banking transactions using CDC, streams them through Kafka, processes them in Spark Structured Streaming, and stores them in Delta Lake for analytics.*

This project is built incrementally — starting minimal and gradually growing into a full enterprise-grade platform.

---

# 📑 **Index**

1. [Overview](#overview)
2. [Architecture (Minimal Version)](#architecture-minimal-version)
3. [Project Goals](#project-goals)
4. [Folder Structure](#folder-structure)
5. [Technologies Used](#technologies-used)
6. [Database Schema](#database-schema)
7. [How to Run (Minimal Setup)](#how-to-run-minimal-setup)
8. [Next Milestones](#next-milestones)
9. [Contributions & Progress](#contributions--progress)

---

# ⭐ **Overview**

**LedgerFlow** is an end-to-end real-time financial data pipeline that captures banking transactions using **CDC**, streams them through **Kafka**, processes them in **Spark Structured Streaming**, and stores them in **Delta Lake** for analytics.

This project is built **incrementally** — starting minimal and gradually growing into a full enterprise-grade financial data platform.

---

# 🚀 **Architecture (Minimal Version)**

```
          MySQL (bank)
        ─────────────────
        customers
        accounts
        transactions
        ─────────────────
                  │
                  ▼
          Debezium CDC
                  │
                  ▼
              Kafka
        ─────────────────
        bank.customers
        bank.accounts
        bank.transactions
        ─────────────────
                  │
                  ▼
   Spark Structured Streaming
   - parse CDC envelope
   - convert to clean schema
   - microbatch upserts
                  │
                  ▼
             Delta Lake
      bronze → silver tables
```

---

# 🎯 **Project Goals**

This project evolves in phases:

### **Phase 0 — Minimal Working Pipeline (DAY 1–2)**

* MySQL + banking schema
* Debezium CDC → Kafka
* Spark SS → Delta Lake (bronze/silver)

### **Phase 1 — Pipeline Hardening**

* UPSERT with Delta MERGE
* Schema evolution
* Deduplication
* Replay capability

### **Phase 2 — Enrichment & Fraud Signals**

* Merchant / device enrichment
* Basic fraud rules
* Event-time aggregations

### **Phase 3 — Analytics Layer**

* dbt modeling (bronze → silver → gold)
* Facts & dimensions
* Business metrics:

  * LTV
  * Merchant performance
  * Failure rates
  * Daily volumes

### **Phase 4 — Orchestration & Monitoring**

* Airflow DAGs
* Great Expectations
* Grafana dashboards

### **Phase 5 — Enterprise Features**

* Backfill & replay framework
* ML feature generation
* Data quality SLOs

---

# 📁 **Folder Structure**

```
ledgerflow/
│
├── infra/
│   └── docker-compose.yml
│
├── cdc/
│   ├── source_mysql/
│   │   └── init.sql
│   └── debezium/
│       └── connector.json
│
├── streaming/
│   └── spark_app/
│       ├── main.py
│       └── requirements.txt
│
├── delta/
│   ├── bronze/
│   └── silver/
│
└── README.md
```

---

# ⚙️ **Technologies Used**

### **Data Sources**

* MySQL (transactional banking data)
* Debezium CDC

### **Streaming Layer**

* Apache Kafka
* Spark Structured Streaming (PySpark)

### **Storage**

* Delta Lake (Bronze/Silver)

### **Orchestration (later phases)**

* Apache Airflow

### **Modeling (later phases)**

* dbt

### **Monitoring (later phases)**

* Grafana + Prometheus
* Great Expectations (DQ)

---

# 🗄️ **Database Schema (Minimal)**

### **customers**

* customer_id
* first_name
* last_name
* email
* created_at

### **accounts**

* account_id
* customer_id
* account_type
* balance
* status
* created_at

### **transactions**

* transaction_id
* account_id
* customer_id
* merchant_id
* amount
* currency
* status
* event_ts
* created_at

---

# 🧪 **How to Run (Minimal Setup)**

### **1. Start infra (Kafka + MySQL + Debezium)**

```
cd infra
docker compose up -d
```

### **2. Register Debezium connector**

```
curl -X POST -H "Content-Type: application/json" \
--data @../cdc/debezium/connector.json \
http://localhost:8083/connectors
```

### **3. Verify CDC events**

Open:

👉 [http://localhost:8080](http://localhost:8080) (Kafka UI)

Check topics:

* `bank_server.bank.customers`
* `bank_server.bank.accounts`
* `bank_server.bank.transactions`

### **4. Run Spark Structured Streaming**

```
spark-submit streaming/spark_app/main.py
```

---

# 📈 **Next Milestones**

* Implement Bronze → Silver MERGE
* Add type casting & cleaning
* Add fraud signal detections
* Add dbt silver/gold models
* Add Airflow DAG
* Add dashboards
* Add DQ with Great Expectations

---

# 🤝 **Contributions & Progress**

This project is intentionally built **from minimal → production-level** to simulate how real fintech companies build streaming data platforms.

Improvement will happen in daily commits.

---

