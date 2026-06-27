# ⚡ PULSE 4.0 — Industrial IIoT Monitoring Dashboard

<div align="center">

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)
![Plotly](https://img.shields.io/badge/Plotly-3F4F75?style=for-the-badge&logo=plotly&logoColor=white)
![SQL Server](https://img.shields.io/badge/SQL%20Server-CC2927?style=for-the-badge&logo=microsoftsqlserver&logoColor=white)
![Scikit-Learn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)

**End-to-end IIoT Real-Time Monitoring & Predictive Maintenance Web Dashboard**  
_SQL Server → Python → Streamlit — live auto-refresh, 9-tab industrial UI, historic data mode_

![Status](https://img.shields.io/badge/Status-Production%20Ready-brightgreen?style=flat-square)
![Domain](https://img.shields.io/badge/Domain-Industry%204.0%20%7C%20IIoT-blue?style=flat-square)
![License](https://img.shields.io/badge/License-Personal%20Project-orange?style=flat-square)

</div>

---

## 📋 Overview

**PULSE 4.0** is a production-grade **Industry 4.0 IIoT monitoring dashboard** built entirely from scratch as a personal engineering project. It reads machine telemetry from a **SQL Server** backend, computes OEE, predictive maintenance, energy, ROI and anomaly KPIs via a **Python calculations engine**, and surfaces everything through a **Streamlit web application** featuring a bespoke "Ultra Premium Dark Industrial" UI theme built with custom CSS, JetBrains Mono typography, and Plotly charts.

The dashboard auto-refreshes live sensor data every 5 seconds using Streamlit's fragment API — without re-rendering the page structure — and fully supports **historic data mode** for any past date range with a purple-themed mode indicator and paused live refresh.

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    PULSE 4.0 — Application Architecture                      │
└─────────────────────────────────────────────────────────────────────────────┘

  ┌───────────────────────────┐
  │   Industrial Data Source  │   ← Machine sensors, PLC registers,
  │   (SQL Server Database)   │     telemetry tables, ML prediction results
  └─────────────┬─────────────┘
                │  pyodbc / SQLAlchemy
                ▼
  ┌─────────────────────────────┐
  │   db.py                     │   ← fetch_raw_data(), load_config()
  │   (Data Access Layer)       │     Reads config.json, queries SQL Server,
  └─────────────┬───────────────┘     returns cleaned DataFrame
                │  pandas DataFrame
                ▼
  ┌─────────────────────────────┐
  │   calculations.py           │   ← compute_all(), color_flag(),
  │   (KPI Engine)              │     style_dataframe()
  └─────────────┬───────────────┘     OEE, MTBF, RUL, ROI, energy KPIs
                │  kpis dict
                ▼
  ┌──────────────────────────────────────────────────────┐
  │   app4.py  —  Streamlit Web Application              │
  │                                                      │
  │   ┌──────────────┐   ┌──────────────────────────┐   │
  │   │  Sidebar     │   │  9-Tab Dashboard          │   │
  │   │  Filters     │   │  ─ ⬡ Command Center       │   │
  │   │  ─ Plant     │   │  ─ ⚙ Operations           │   │
  │   │  ─ Line      │   │  ─ 🔧 Maintenance          │   │
  │   │  ─ Machine   │   │  ─ 📡 Technician           │   │
  │   │  ─ Status    │   │  ─ ₹  ROI & Finance        │   │
  │   │  ─ Severity  │   │  ─ ⚡ Energy & ESG         │   │
  │   │  ─ Risk      │   │  ─ 🤖 AI Insights          │   │
  │   │  ─ Date Range│   │  ─ 📋 Shift Report         │   │
  │   └──────────────┘   │  ─ 📄 Machine Detail       │   │
  │                       └──────────────────────────┘   │
  │                                                      │
  │   @st._fragment(run_every=5s) — flicker-free live    │
  │   refresh via st.empty() slot pattern                │
  └──────────────────────────────────────────────────────┘
                │
                ▼  Browser
  ┌─────────────────────────────┐
  │   End User                  │   ← Any browser, LAN or internet
  │   (Web Dashboard)           │     with optional auth/reverse proxy
  └─────────────────────────────┘
```

---

## 🛠️ Technology Stack

| Layer | Technology | Role |
|---|---|---|
| **Web Framework** | Streamlit | Single-page web app, tab layout, sidebar, fragment live refresh |
| **Data Visualisation** | Plotly (Express + Graph Objects) | Bar, line, area, scatter, pie, waterfall, box charts |
| **Data Processing** | Pandas | DataFrame filtering, aggregation, KPI computation |
| **Data Access** | `db.py` (custom) | SQL Server connection, raw data fetch, config loader |
| **KPI Engine** | `calculations.py` (custom) | OEE, MTBF, RUL, ROI, energy efficiency, anomaly, risk scoring |
| **Configuration** | `config.json` | App settings, color tokens, refresh interval, cost parameters |
| **Database** | Microsoft SQL Server | Primary telemetry store — machines, sensors, alarms, predictions |
| **UI Theme** | Custom CSS + Google Fonts | Ultra Premium Dark Industrial — Inter + JetBrains Mono |
| **Charting** | Plotly | Industrial-themed dark charts with threshold reference lines |

---

## ✨ Key Features

### ⚡ Flicker-Free Live Refresh
- Uses Streamlit's `@st._fragment(run_every=N)` to update data every 5 seconds
- Tab structure and layout rendered **once** outside the fragment — never rebuilt
- Each content section uses `st.empty()` slot pattern — only data updates, no DOM tear-down
- MutationObserver JS injected to immediately cancel any Streamlit opacity-dim on refresh
- All `transition` and `animation` properties suppressed on structural elements

### 🕐 Historic Data Mode
- Date range picker defaults to **today** (live view) — fully editable back to earliest DB record
- When a past date range is selected, a **purple-themed historic mode banner** appears across all tabs
- Header switches from `⚡ LIVE` to `🕐 HISTORIC` with the selected date range displayed
- Live auto-refresh is **paused** in historic mode — data stays frozen to the selected range
- All KPIs, charts, tables and alerts reflect the filtered historic date range

### 🎛️ Sidebar Filters — All Applied Globally
- **Plant** → **Line** (cascading) → **Machine ID** → **Status** → **Alarm Severity** → **Risk Level** → **Date Range**
- All filters are applied inside the fragment before any KPI computation
- Filter state read via `st.session_state` so sidebar changes instantly propagate to the live fragment

### ⬡ Command Center (Tab 1)
- 12 KPI cards across two rows: OEE, Availability, Risk, Units, Downtime, Failure Risk, Running/Stopped, Fleet Online %, Reject Rate, Anomalies, MTBF
- OEE % by Plant (horizontal bar), Fleet Status donut, High Alarms by Plant (bar)
- Full OEE trend line chart across all plants with 85% target reference line
- Alert banners for high-severity machines and critical failure probability

### ⚙ Operations (Tab 2)
- Production Rate, High Alarms, Downtime, Good Units %, Power Factor KPIs
- Downtime Pareto chart (bar + cumulative % line on dual axis)
- Production Rate by Plant (box plot), OEE % by Machine, Alarm Distribution (stacked bar)

### 🔧 Maintenance (Tab 3)
- 8 KPI cards: Maintenance Needed, Critical Risk, Anomalies, MTBF, Failure Risk %, Avg RUL, Predicted Failures 24h, Anomaly Rate %
- Failure Probability % by Machine (horizontal bar with 70% critical line)
- RUL Prediction by Machine (horizontal bar with 500-cycle minimum line)
- Vibration vs Bearing Temp scatter — bubble sized by Failure %, coloured by Risk Level
- Full Risk Priority Table with colour-coded styling

### 📡 Technician (Tab 4)
- 8 live sensor KPIs: Max Temperature, Max Vibration, Max RPM, Max Bearing Temp, Max Power, Avg Voltage, Machines At Risk, Latest Timestamp
- Temperature trend area chart (80°C limit), Vibration trend area chart (4.5 mm/s limit)
- Full Live Sensor Readings table with all sensor columns

### ₹ ROI & Finance (Tab 5)
- Downtime Saved ₹, Energy Saved ₹, Reject Cost ₹, Total Savings ₹, Payback Months, ROI Multiplier, Net Benefit ₹, Total Energy kWh
- Waterfall chart: savings breakdown by category
- Savings by Plant (horizontal bar), Energy Efficiency % by Plant, Reject Cost by Plant

### ⚡ Energy & ESG (Tab 6)
- Total Energy kWh, CO₂ Equivalent, Energy Cost ₹, Avg Efficiency %, Avg Power Factor
- Energy by Plant (horizontal bar), Efficiency Trend (area chart)
- Power Factor by Machine, CO₂ by Plant

### 🤖 AI Insights (Tab 7)
- Fleet Health Score — large numeric display with colour-coded status (EXCELLENT / GOOD / FAIR / CRITICAL)
- AI Alerts, Predicted Failures, Anomaly Rate %, Critical Machines KPIs
- Failure Risk % by Machine (bar), Risk Level Distribution (donut pie)
- AI Risk Summary Table

### 📋 Shift Report (Tab 8)
- Shift OEE %, Units Produced, Shift Downtime, Shift Alarms KPIs
- OEE % by Shift and Plant (grouped bar), Units Produced by Machine (bar)
- Shift Detail table — all machines

### 📄 Machine Detail (Tab 9)
- Full 36-column sensor data table for selected machine/filter
- Context banner: machine, plant, as-of timestamp
- CSV export (filtered view) + All Records CSV export

---

## 📁 Project Structure

```
pulse4.0/
├── app4.py               # Main Streamlit application — UI, tabs, fragment refresh
├── db.py                 # Data access layer — SQL Server connection, fetch_raw_data()
├── calculations.py       # KPI engine — compute_all(), color_flag(), style_dataframe()
├── config.json           # App configuration — DB connection, refresh interval, cost params
└── requirements.txt      # Python dependencies
```

---

## ⚙️ Configuration (`config.json`)

```json
{
  "app": {
    "refresh_interval_ms": 5000,
    "cost_per_minute_inr": 500,
    "product_value_inr": 250,
    "implementation_cost_inr": 500000
  },
  "colors": {
    "red": "#e63946",
    "amber": "#ff9f1c",
    "green": "#06d6a0",
    "blue": "#00b4d8"
  },
  "database": {
    "server": "localhost",
    "database": "PULSE4_DB",
    "trusted_connection": true
  }
}
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.10+
- Microsoft SQL Server (local or network)
- ODBC Driver 17 or 18 for SQL Server

### Installation

```bash
git clone https://github.com/shrikhating/pulse4.0.git
cd pulse4.0
pip install -r requirements.txt
```

### Requirements

```
streamlit
pandas
plotly
pyodbc
sqlalchemy
```

### Running the Dashboard

```bash
streamlit run app4.py
```

The dashboard will open at `http://localhost:8501` by default.

### Running on a Custom Port

```bash
streamlit run app4.py --server.port 8080
```

---

## 🔐 Exposing to the Internet — Securely

> **Never expose a raw `streamlit run` process directly to the internet.**  
> Always place it behind a reverse proxy with TLS and authentication.

### Recommended Production Setup

```
Internet → Cloudflare / DNS → Nginx (TLS + Auth) → localhost:8501 (Streamlit)
```

**Step 1 — Add Streamlit authentication** (simplest layer):

Use [`streamlit-authenticator`](https://github.com/mkhorasani/Streamlit-Authenticator) to add login before the dashboard loads.

```bash
pip install streamlit-authenticator
```

**Step 2 — Run behind Nginx with TLS**:

```nginx
server {
    listen 443 ssl;
    server_name pulse.yourdomain.com;

    ssl_certificate     /etc/letsencrypt/live/pulse.yourdomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/pulse.yourdomain.com/privkey.pem;

    location / {
        proxy_pass         http://127.0.0.1:8501;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection "upgrade";
        proxy_set_header   Host $host;
        proxy_read_timeout 86400;
    }
}
```

Get a free TLS certificate via [Let's Encrypt](https://letsencrypt.org/):
```bash
certbot --nginx -d pulse.yourdomain.com
```

**Step 3 — Firewall** (allow only ports 80, 443):

```bash
ufw allow 80/tcp
ufw allow 443/tcp
ufw deny 8501   # block direct Streamlit port from internet
ufw enable
```

**Step 4 — Optional: Cloudflare Tunnel** (zero open inbound ports):

```bash
cloudflared tunnel --url http://localhost:8501
```
No port forwarding needed — Cloudflare handles TLS and DDoS protection.

**Step 5 — Keep Streamlit running**:

```bash
# systemd service (Linux)
[Unit]
Description=PULSE 4.0 Dashboard
After=network.target

[Service]
User=pulse
WorkingDirectory=/opt/pulse4.0
ExecStart=/usr/bin/streamlit run app4.py --server.port 8501 --server.headless true
Restart=always

[Install]
WantedBy=multi-user.target
```

```bash
systemctl enable pulse4 && systemctl start pulse4
```

**Security checklist:**

- [ ] HTTPS only — no HTTP access to the dashboard
- [ ] Login wall via `streamlit-authenticator` before any data is visible
- [ ] SQL Server accessible only from `localhost` — no public DB port
- [ ] Firewall blocks port 8501 from external access
- [ ] Streamlit config: `server.headless = true`, `server.enableCORS = false`
- [ ] Rotate credentials; use environment variables, not hardcoded strings in `config.json`

---

## 🏭 Domain Context

Built for the **manufacturing / Industry 4.0** domain, PULSE 4.0 converts raw machine telemetry stored in SQL Server into actionable operational intelligence — OEE tracking, predictive maintenance scheduling, energy ESG reporting, and ROI quantification — surfaced through a browser-based dashboard accessible from any device on the plant network or securely over the internet.

---

## 👤 Author

**Shrikant Khating**  
Senior BI Developer | IIoT Engineer | AI/ML Practitioner  
📧 shri.khating@gmail.com  
🔗 [LinkedIn](https://linkedin.com/in/shrikant-khating) · [GitHub](https://github.com/shrikhating)

> *Built as a solo personal innovation lab project — no team, no budget, production-grade engineering.*
