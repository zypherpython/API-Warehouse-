# Crypto Data Platform

An end-to-end data engineering pipeline that pulls live cryptocurrency data from a public API, orchestrates the workflow with Apache Airflow, stores it in PostgreSQL, generates analytics, and uses an LLM to produce plain-English market summaries — all visualized in a Streamlit dashboard.

---

## What this project does

Airflow runs on a schedule, hits a crypto API, cleans the response, and loads it into PostgreSQL. From there, SQL queries compute metrics like daily trends, monthly highs/lows, and price spikes. Those metrics get passed to an LLM which writes a short human-readable summary. A Streamlit app ties it all together.

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
    ├──▶ Analytics layer     ← SQL: trends, highs/lows, spike detection
    │         │
    │         ▼
    │    AI insight layer    ← LLM summarizes computed metrics
    │         │
    └─────────┴──▶ Streamlit dashboard
```

---

## Tech stack

| Layer | Tool |
|---|---|
| Orchestration | Apache Airflow |
| Ingestion | Python + Requests |
| Transformation | Pandas |
| Warehouse | PostgreSQL |
| Analytics | SQL |
| AI insights | LLM API |
| Dashboard | Streamlit |

---

## Database schema

```sql
CREATE TABLE crypto_prices (
    id          SERIAL PRIMARY KEY,
    symbol      TEXT NOT NULL,
    price_usd   FLOAT,
    market_cap  FLOAT,
    volume_24h  FLOAT,
    timestamp   TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## Analytics

SQL queries run against the warehouse to produce:

- Daily price tracking
- Monthly high/low calculations
- Percentage change over time
- Rule-based spike detection

```python
# Spike detection logic
if price_change_1h > 5%:
    spike_detected = True
```

---

## AI insight layer

Computed metrics are passed to an LLM which returns a plain-English summary. No predictions, no automated decisions — just readable context on top of the numbers.

Example output:
> BTC is trading above its 7-day average with elevated volume, suggesting short-term buying pressure.

---

## Dashboard features

- Live prices for BTC, ETH, SOL
- Historical trend charts
- Monthly high/low visualization
- Spike detection highlights
- AI-generated insight panel
- Filter by coin and date range

---

## Getting started

**Prerequisites:** Python 3.8+, PostgreSQL running locally, Airflow installed

```bash
git clone https://github.com/your-username/crypto-data-platform.git
cd crypto-data-platform
pip install -r requirements.txt
```

Set your API keys and DB connection string in `.env`, then:

```bash
airflow standalone       # start Airflow
streamlit run app.py     # start the dashboard
```

---

## What I built this to learn

- End-to-end ETL pipeline design
- Airflow DAG structure — tasks, dependencies, scheduling, retries
- Warehouse schema design for time-series data
- SQL-based analytics on real market data
- Using an LLM as an interpretation layer on computed metrics
- Connecting a live dashboard to a backend database

---

## What's next

- Docker so setup is one command
- Deploy on AWS (EC2 + RDS)
- Swap batch ingestion for Kafka streaming
- Add dbt for warehouse transformations
- Alerting via Telegram or email on spike detection
- Expand to multi-source financial data
