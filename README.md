# Homelab-NMS

A lightweight, on-premise observability and log processing pipeline designed for homelabs and SMEs. Built on an event-driven architecture using Kafka, ClickHouse, Spring Boot, and Grafana, it serves as an affordable, self-hosted alternative to heavy enterprise monitoring platforms.

*Note: This project is currently under active development.*

## 🚀 Architecture & Data Flow

The platform collects data from multiple observability sources, processes it through a robust Java-based pipeline, and stores it in a columnar database for high-performance time-series querying.

`[Agents]` -> `[Kafka: raw-*]` -> `[Spring Boot Pipeline]` -> `[Kafka: parsed-*]` -> `[ClickHouse]` -> `[Grafana]`

### Core Observability Sources
1. **Infrastructure Metrics:** Agents collect `cpu%`, `ram%`, `disk%`, and state data.
2. **Security & Log Engine:** Tailed `/var/log/auth.log` data parsed via Grok. Includes a stateful session tracker to detect brute-force SSH attacks in real-time.
3. **Network Availability:** SNMP and ICMP ping monitoring for device uptime and latency tracking.

## 🛠️ Tech Stack

*   **Ingestion & Message Broker:** Apache Kafka (KRaft mode)
*   **Processing Pipeline:** Java, Spring Boot, Grok
*   **Time-Series Storage:** ClickHouse (MergeTree Engine)
*   **Relational Metadata:** PostgreSQL
*   **Visualization:** Grafana (ClickHouse Datasource Plugin)
*   **Deployment:** Docker Compose (K8s-ready)

## ⚙️ Planned Features

*   **Grafana Templating:** Dynamic variables (`$host`, `$source`) to scale a single dashboard template across thousands of nodes.
*   **Stateful Security Alerts:** In-memory session tracking to detect 5+ failed login attempts from a single IP within a 60-second window.
*   **Data Retention Policies:** Hot/warm/cold storage policies handled directly via ClickHouse TTLs.
*   **Multi-Tenancy:** Future support for isolated organizational data (`org_id`).

## 🚦 Getting Started

The platform runs entirely in Docker. To spin up the core ingestion, storage, and visualization layer:

```bash
git clone [https://github.com/ashish-belel/homelab-nms.git](https://github.com/ashish-belel/homelab-nms.git)
cd homelab-nms
docker-compose up -d
```

Grafana: http://localhost:3000 (admin/admin)

Kafka Brokers: localhost:9092

ClickHouse Native: localhost:9000
