# Crypto Data Platform

An end-to-end data engineering pipeline that pulls live cryptocurrency data from a public API, orchestrates the workflow with Apache Airflow, stores it in PostgreSQL, and surfaces insights through a Streamlit dashboard.

---

## What this project does

Every N minutes, Airflow triggers a DAG that hits a crypto API, cleans the response, and loads it into a PostgreSQL warehouse. A Streamlit app reads from that warehouse and renders live price tracking, historical trends, and comparison charts — no manual steps required once it's running.

---

## Architecture

```
Crypto API
    │
    ▼
Apache Airflow          ← schedules, retries, monitors
    │
    ▼
PostgreSQL              ← stores all historical price data
    │
    ├──▶ SQL queries     ← ad-hoc analysis
    └──▶ Streamlit app   ← interactive dashboard
```

---

## Tech stack

| Layer | Tool |
|---|---|
| Orchestration | Apache Airflow |
| Data ingestion | Python + Requests |
| Transformation | Pandas |
| Warehouse | PostgreSQL |
| Dashboard | Streamlit |
| Language | Python, SQL |

---

## Database schema

```sql
CREATE TABLE crypto_prices (
    id          SERIAL PRIMARY KEY,
    symbol      TEXT,
    price_usd   FLOAT,
    market_cap  FLOAT,
    volume_24h  FLOAT,
    timestamp   TIMESTAMP
);
```

---

## Getting started

**Prerequisites:** Python 3.8+, PostgreSQL running locally, Airflow installed

```bash
git clone https://github.com/your-username/crypto-data-platform.git
cd crypto-data-platform
pip install -r requirements.txt
```

**Start Airflow**
```bash
airflow standalone
```

**Run the dashboard**
```bash
streamlit run app.py
```

Make sure PostgreSQL is running and your connection string is set in `.env` before starting either service.

---

## Dashboard features

- Live prices for BTC, ETH, SOL
- Price trends over time
- Average price comparison across coins
- Filter by coin and date range
- One-click refresh

---

## What I built this to learn

- Designing and running ETL pipelines from scratch
- Airflow DAG structure — tasks, dependencies, scheduling, retries
- Warehouse schema design for time-series market data
- Connecting a live dashboard to a backend database
- Handling real API responses — rate limits, nulls, type mismatches

---

## What's next

- Add Docker so setup is one command
- Deploy on AWS (EC2 + RDS)
- Swap batch ingestion for Kafka streaming
- Add dbt for warehouse transformations
- Expand beyond crypto to multi-source financial data
