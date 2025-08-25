# Pricing Intelligence ETL Pipeline

## Overview
This project sets up a daily Airflow-orchestrated ETL pipeline for scraping book data from books.toscrape.com, enriching with GBP-INR exchange rates, transforming, and loading to PostgreSQL. It's fully containerized with Docker Compose.

## Database Schema
- **staging_exchange_rates**: Stores daily rates (columns: date DATE PRIMARY KEY, rate DECIMAL(10,4)).
- **products**: Final table (columns: product_id VARCHAR(64) PRIMARY KEY, title VARCHAR(255), price_gbp DECIMAL(10,2), price_inr DECIMAL(10,2), category VARCHAR(100), availability VARCHAR(50), price_tier VARCHAR(20)).

## Setup and Running
1. Ensure Docker and Docker Compose are installed.
2. Build and start services: `docker-compose up -d --build`.
3. Access Airflow UI at http://localhost:8080 (username: airflow, password: airflow).
4. Enable the `pricing_etl_daily` DAG and trigger it manually if needed.
5. To stop: `docker-compose down`.

## Testing Individual Stages
- Scrape: `python scripts/scrape.py`
- Fetch: Update conn_params in scripts/fetch_exchange.py and run `python scripts/fetch_exchange.py`
- Transform: Use sample data in scripts/transform.py and run it.
- Load: Similar to fetch, run with sample data.

## Production Considerations
- **Secrets**: Use Airflow Variables/Connections for DB creds (set 'postgres_default' connection in UI).
- **Reproducibility**: Docker ensures consistency.
- **Logging**: Scripts use Python logging; Airflow handles DAG logs.
- **Retries**: DAG has built-in retries.
- **Tests**: Add unit tests in a `tests/` folder (e.g., pytest for scripts).
- For prod, scale with CeleryExecutor and add monitoring.

Clone this repo and run `docker-compose up` to get started!