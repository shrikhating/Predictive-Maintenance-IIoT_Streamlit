# ⚡ PULSE 4.0 — Industrial Predictive Maintenance System

<div align="center">

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![MQTT](https://img.shields.io/badge/MQTT-660066?style=for-the-badge&logo=mqtt&logoColor=white)
![SQL Server](https://img.shields.io/badge/SQL%20Server-CC2927?style=for-the-badge&logo=microsoftsqlserver&logoColor=white)
![Scikit-Learn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)
![Node-RED](https://img.shields.io/badge/Node--RED-8F0000?style=for-the-badge&logo=node-red&logoColor=white)
![Ollama](https://img.shields.io/badge/Ollama-000000?style=for-the-badge&logo=ollama&logoColor=white)

**End-to-end IIoT Predictive Maintenance Platform**  
_Siemens S7 PLC → MQTT → SQL Server → ML → LLM → Power BI — 100% on-premises, zero cloud dependency_

![Status](https://img.shields.io/badge/Status-Production%20Ready-brightgreen?style=flat-square)
![Domain](https://img.shields.io/badge/Domain-Industry%204.0%20%7C%20IIoT-blue?style=flat-square)
![License](https://img.shields.io/badge/License-Personal%20Project-orange?style=flat-square)

</div>

---

## 📋 Overview

**PULSE 4.0** is a production-grade **Industry 4.0 predictive maintenance system** built entirely from scratch as a personal engineering project. It ingests real-time telemetry from a **Siemens SIMATIC S7-1200C PLC** over Ethernet using the Snap7 protocol, streams sensor data through an **MQTT messaging layer**, persists it in **SQL Server**, applies **Scikit-learn ML models** for anomaly detection and failure prediction, enriches results with a local **Ollama LLM**, and surfaces insights via two parallel interfaces:

- 📊 A **9-page Power BI dashboard** with custom "PULSE 4.0 Industrial Dark" theme  
- 🖥️ A **CustomTkinter desktop application** with Aurora Industrial Dark v2 UI

The entire platform runs on-premises — no cloud services, no SaaS subscriptions, no external API calls.

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    PULSE 4.0 — End-to-End Architecture                       │
└─────────────────────────────────────────────────────────────────────────────┘

  ┌───────────────────────────┐
  │  Siemens SIMATIC S7-1200C │   ← Industrial PLC: process sensors,
  │  (Ethernet / ISO-TCP:102) │     actuators, diagnostic registers
  └─────────────┬─────────────┘
                │  Snap7 Protocol (Python)
                ▼
  ┌─────────────────────────────┐
  │   Python Snap7 Client       │   ← Polls DB registers, I/O tags,
  │   (Data Acquisition Layer)  │     diagnostic data blocks at configurable
  └─────────────┬───────────────┘     intervals
                │  JSON Payload
                ▼
  ┌─────────────────────────────┐
  │   MQTT Publisher            │──────────► Mosquitto Broker
  │   (Paho-MQTT)               │              │
  └─────────────────────────────┘              │ Subscribe
                                               ▼
                                  ┌─────────────────────────┐
                                  │   Node-RED Flows         │
                                  │  ─ Engineering unit conv │
                                  │  ─ Tag mapping/routing   │
                                  │  ─ Conditional alerting  │
                                  └────────────┬────────────┘
                                               │
                                               ▼
                                  ┌─────────────────────────┐
                                  │   Microsoft SQL Server   │
                                  │  ─ Raw telemetry tables  │
                                  │  ─ ML prediction results │
                                  │  ─ Alert & event log     │
                                  └────────────┬────────────┘
                                               │
                   ┌───────────────────────────┼────────────────────────┐
                   ▼                           ▼                        ▼
    ┌──────────────────────┐    ┌──────────────────────┐  ┌────────────────────────┐
    │  Scikit-learn ML     │    │  Ollama LLM (Local)  │  │  Power BI Desktop      │
    │  ─ Anomaly Detection │    │  ─ NL diagnostics    │  │  ─ 9-Page Dashboard    │
    │  ─ RUL Estimation    │    │  ─ Maintenance advice│  │  ─ Industrial Dark Theme│
    │  ─ Fault Classifier  │    │  ─ Fully offline     │  │  ─ DirectQuery + Import │
    └──────────────────────┘    └──────────────────────┘  └────────────────────────┘
                   │
                   ▼
    ┌──────────────────────────────────┐
    │  CustomTkinter Desktop App       │
    │  ─ Aurora Industrial Dark v2 UI  │
    │  ─ Lazy tab rendering            │
    │  ─ Real-time KPI cards           │
    │  ─ Packaged: PyInstaller+PyArmor │
    └──────────────────────────────────┘
```

---

## 🛠️ Technology Stack

| Layer | Technology | Role |
|---|---|---|
| **Hardware** | Siemens SIMATIC S7-1200C PLC | Industrial process controller — primary data source |
| **PLC Connectivity** | Python `snap7` | ISO-TCP Ethernet communication; reads DB registers & I/O tags |
| **Message Broker** | Mosquitto (MQTT) | Real-time telemetry streaming and topic routing |
| **MQTT Client** | Paho-MQTT (Python) | Publisher (acquisition side) and subscriber (flow side) |
| **Flow Orchestration** | Node-RED | Tag mapping, unit conversion, alert condition evaluation |
| **Data Persistence** | Microsoft SQL Server | Primary time-series store for telemetry, ML results, events |
| **Machine Learning** | Scikit-learn | Anomaly detection, fault classification, RUL estimation |
| **Local LLM** | Ollama | AI-generated maintenance advisories — offline inference |
| **Desktop UI** | Python CustomTkinter | Aurora Industrial Dark v2 themed desktop application |
| **Business Intelligence** | Microsoft Power BI | 9-page PULSE 4.0 Industrial Dark dashboard |
| **Packaging** | PyInstaller + PyArmor | Single-file Windows executable with code obfuscation |

---

## ✨ Key Features

### 🔌 Real-Time PLC Integration
- Direct Ethernet connection to Siemens SIMATIC S7-1200C using `snap7` Python library
- Reads process DB registers, discrete I/O, and diagnostic memory areas
- Configurable polling interval to balance data granularity with network load

### 📡 MQTT Streaming Pipeline
- Structured JSON telemetry published to Mosquitto broker via Paho-MQTT
- Node-RED subscribes and applies engineering unit conversions, tag name mapping, and threshold-based alerting
- Decoupled architecture — any downstream consumer can subscribe to any topic

### 🤖 Scikit-learn ML Engine
- **Anomaly Detection** — flags deviations from normal operating baselines
- **Fault Classification** — categorizes fault types based on sensor signature patterns
- **Remaining Useful Life (RUL) Estimation** — predicts component degradation trajectory

### 🧠 Local Ollama LLM Layer
- Fully offline AI inference — no OpenAI or cloud API dependency
- Takes ML output as context and generates natural-language maintenance recommendations
- Zero data leaves the plant network

### 📊 9-Page Power BI Dashboard
- Custom **"PULSE 4.0 Industrial Dark"** theme — built from scratch for industrial aesthetics
- Mix of DirectQuery (live telemetry) and Import (historical/ML result) data modes
- Pages cover live KPIs, telemetry trends, anomaly scores, fault history, RUL, alerts, ML predictions, LLM advisories, and system health

### 🖥️ Aurora Industrial Dark v2 Desktop App
- Built with Python **CustomTkinter** — native-feeling desktop UI
- **Lazy tab rendering** — tabs initialize only when first accessed, dramatically reducing startup time and memory footprint
- Panels: live telemetry, ML predictions, alert console, historical trends, LLM advisory

### 📦 PyInstaller + PyArmor Deployment Pipeline
- Single `.exe` output via PyInstaller — no Python runtime needed on target machine
- Source code obfuscated with PyArmor for IP protection
- `build.bat` script automates the full packaging workflow

---

## 🔄 Complete Data Flow

```
Step 1 — ACQUISITION
  └─ Python Snap7 polls PLC every N ms
  └─ Reads: process values, status bits, diagnostic registers, counters

Step 2 — PUBLISHING
  └─ JSON payload structured with tag names, timestamp, engineering units
  └─ Published to Mosquitto under topic: pulse4/{plant}/{device}/{parameter}

Step 3 — FLOW PROCESSING (Node-RED)
  └─ Subscribes to MQTT topics
  └─ Applies engineering unit conversions (raw ADC → physical units)
  └─ Maps PLC tag addresses to human-readable signal names
  └─ Evaluates thresholds → routes alerts to SQL Server event log

Step 4 — PERSISTENCE (SQL Server)
  └─ Raw telemetry → time-stamped fact tables
  └─ Aggregated KPIs → computed views for BI consumption
  └─ Alerts → event log table with severity, source, timestamp

Step 5 — ML INFERENCE (Scikit-learn)
  └─ Pipelines consume persisted telemetry
  └─ Output: anomaly score (0-1), fault label, RUL estimate (hours)
  └─ Results written back to SQL Server prediction tables

Step 6 — LLM ENRICHMENT (Ollama)
  └─ ML results + recent telemetry context passed as prompt
  └─ Returns natural-language maintenance advisory
  └─ Advisory stored and surfaced via both UI interfaces

Step 7 — VISUALIZATION
  └─ Power BI: DirectQuery live data + Import for ML/historical layers
  └─ CustomTkinter App: polling loop with lazy-loaded tab components
```

---

## 📊 Power BI Dashboard — Page Inventory

| Page # | Title | Content |
|---|---|---|
| 1 | Executive Overview | Live KPI summary — OEE, availability, fault count, RUL critical alerts |
| 2 | Real-Time Telemetry | Live sensor feeds, process variable gauges |
| 3 | Anomaly Detection | ML anomaly score trends, threshold breach markers |
| 4 | Fault Classification | Fault type distribution, Pareto analysis |
| 5 | Remaining Useful Life | RUL estimates by component, degradation curves |
| 6 | Alert History | Timestamped event log with severity classification |
| 7 | Predictive Insights | ML model confidence, prediction accuracy tracking |
| 8 | Maintenance Advisory | LLM-generated recommendations panel |
| 9 | System Health | Pipeline connectivity status, data freshness indicators |

---

## 📁 Project Structure

```
pulse4.0/
├── plc/
│   └── snap7_client.py           # PLC data acquisition — Snap7 ISO-TCP reader
├── mqtt/
│   └── publisher.py              # Paho-MQTT telemetry publisher
├── node_red/
│   └── flows.json                # Exported Node-RED flow definitions
├── database/
│   └── schema.sql                # SQL Server schema — tables, views, indexes
├── ml/
│   ├── anomaly_detection.py      # Isolation Forest / One-Class SVM pipeline
│   ├── fault_classifier.py       # Supervised fault classification pipeline
│   └── rul_estimator.py          # Regression-based RUL estimation
├── llm/
│   └── ollama_advisor.py         # Ollama integration — prompt builder + response handler
├── app/
│   ├── main.py                   # CustomTkinter entry point
│   ├── theme/                    # Aurora Industrial Dark v2 color tokens
│   └── tabs/                     # Lazy-loaded tab components
├── powerbi/
│   └── PULSE4_Dashboard.pbix     # Power BI report file
├── config/
│   └── settings.yaml             # All configuration (separate from code)
└── deploy/
    └── build.bat                 # PyInstaller + PyArmor packaging script
```

---

## ⚙️ Configuration (`config/settings.yaml`)

```yaml
plc:
  ip: "192.168.x.x"
  rack: 0
  slot: 1
  poll_interval_ms: 1000

mqtt:
  broker: "localhost"
  port: 1883
  topic_prefix: "pulse4/"

database:
  server: "localhost"
  database: "PULSE4_DB"
  trusted_connection: true

ollama:
  model: "llama3.1:8b"
  endpoint: "http://localhost:11434"
  max_tokens: 512
```

> **Note:** CLI arguments override all config values at runtime.

---

## 🚀 Getting Started

### Prerequisites

- Python 3.10+
- Siemens S7-1200C PLC with Ethernet port (ISO-TCP port 102 accessible)
- Mosquitto MQTT Broker
- Node-RED
- Microsoft SQL Server
- Ollama with `llama3.1:8b` pulled (`ollama pull llama3.1:8b`)
- Power BI Desktop (for the dashboard)

### Installation

```bash
git clone https://github.com/shrikhating/pulse4.0.git
cd pulse4.0
pip install -r requirements.txt
```

### Running the Pipeline

```bash
# Initialize database schema
sqlcmd -S localhost -i database/schema.sql

# Start PLC acquisition
python plc/snap7_client.py --config config/settings.yaml

# Start desktop application
python app/main.py

# Build distributable executable
deploy/build.bat
```

---

## 🏭 Domain Context

Built for the **manufacturing / Industry 4.0** domain, PULSE 4.0 solves the real operational challenge of converting raw PLC registers into actionable maintenance intelligence without cloud connectivity — a critical requirement for air-gapped or bandwidth-constrained industrial environments.

---

## 👤 Author

**Shrikant Khating**  
Senior BI Developer | IIoT Engineer | AI/ML Practitioner  
📧 shri.khating@gmail.com  
🔗 [LinkedIn](https://linkedin.com/in/shrikant-khating) · [GitHub](https://github.com/shrikhating)

> *Built as a solo personal innovation lab project — no team, no budget, production-grade engineering.*
