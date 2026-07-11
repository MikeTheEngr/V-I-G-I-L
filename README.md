<div align="center">
 
---


██╗   ██╗  ║██╗   ██████╗    ║██╗  ║██╗     
██║   ██║  ║██║  ║██╔════╝   ║██║  ║██║     
██║   ██║  ║██║  ║██║  ███╗  ║██║  ║██║     
╚██╗ ██╔╝  ║██║  ║██║   ██║  ║██║  ║██║     
 ╚████╔╝   ║██║  ╚ ██████╔╝  ║██║  ║███████╗
  ╚═══╝    ╚══╝    ╚══════╝  ╚══╝  ╚═══════╝

---
</div>


### Predictive Intelligence for Industrial Infrastructure


![Vigil Dashboard](https://raw.githubusercontent.com/MikeTheEngr/V-I-G-I-L/main/vigil.gif)

> Always watching. Always ready.
---

## What is Vigil?

Vigil is an AI-powered industrial monitoring platform that continuously watches over critical equipment and pipeline infrastructure, detecting anomalies, predicting failures before they happen, and alerting operators in real time.

Built to demonstrate production-grade ML engineering, full-stack development, and industrial AI system design.

---

## Core Intelligence

### Predictive Maintenance Engine
Analyses equipment sensor data (temperature, vibration, RPM, pressure) and forecasts **Remaining Useful Life (RUL)** using a **GradientBoosting regression model** trained on the NASA CMAPSS turbofan engine degradation dataset (20,631 readings across 100 real engines).

- MAE: **5.62 cycles** on held-out validation set
- Outputs: health score (0–100%), RUL in days, risk level (Nominal / Watch / Warning / Critical)
- Auto-generates alerts when equipment crosses critical thresholds

### Pipeline Leak Detection
Monitors pipeline sensor streams (pressure differential, flow rate, acoustic emission, vibration) for the physical signature of a leak using a **supervised + unsupervised ensemble**:

- **RandomForestClassifier** — catches known leak patterns
- **IsolationForest** — catches novel/unknown anomalies
- Combined weighted anomaly score (0.0–1.0)
- ROC-AUC: **1.0** on held-out validation set

---

## Live Demo

🔗 **[V I G I L](https://vigil-alpha-lovat.vercel.app/)**

---

## Stack

| Layer | Technology |
|---|---|
| **Frontend** | Next.js 16, TypeScript, Tailwind CSS v4, Recharts |
| **Backend** | FastAPI, SQLAlchemy, APScheduler |
| **Database** | PostgreSQL (Neon — managed serverless) |
| **ML** | scikit-learn — GradientBoosting, RandomForest, IsolationForest |
| **Hosting** | Vercel (frontend) + Render (backend) + Neon (database) |

---

## System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    VIGIL DASHBOARD                      │
│              Next.js + TypeScript (Vercel)              │
└────────────────────────┬────────────────────────────────┘
                         │ REST API
┌────────────────────────▼────────────────────────────────┐
│                   FASTAPI BACKEND                       │
│                   Python (Render)                       │
│                                                         │
│  ┌─────────────────┐    ┌──────────────────────────┐   │
│  │ Predictive      │    │ Leak Detection           │   │
│  │ Maintenance     │    │ RandomForest +           │   │
│  │ GradientBoosting│    │ IsolationForest ensemble │   │
│  └────────┬────────┘    └────────────┬─────────────┘   │
└───────────┼─────────────────────────┼─────────────────┘
            │                         │
┌───────────▼─────────────────────────▼─────────────────┐
│                    POSTGRESQL                          │
│              (Neon — serverless Postgres)              │
│  equipment · pipelines · sensors · sensor_readings    │
│  predictions · alerts · users                         │
└────────────────────────────────────────────────────────┘
```

---

## Key Features

- **Live dashboard** — system status, equipment health, pipeline status, active alerts
- **Health score trends** — 30-day history charts per equipment
- **Real-time alert management** — acknowledge, resolve, filter by severity
- **Automated prediction cycle** — ML models score all equipment every 15 minutes
- **Sensor data ingestion** — bulk ingest endpoint for IoT / simulator data
- **Responsive design** — works on desktop and mobile

---

## Dataset

The predictive maintenance model was trained on the **NASA CMAPSS (Commercial Modular Aero-Propulsion System Simulation)** dataset — a widely used benchmark for remaining useful life prediction in turbofan engines.

The leak detection model uses physics-informed synthetic data encoding the real signatures of pipeline leaks: downstream pressure drop, flow loss, and acoustic emission spike.

---

## Local Setup

```bash
# Clone the code repo
git clone https://github.com/MikeTheEngr/VIGIL.git
cd VIGIL

# Backend
cd backend
python -m venv venv
venv\Scripts\activate  # Windows
pip install -r requirements.txt

# Add your .env file
cp .env.example .env
# Edit .env with your database credentials

# Train the ML models
cd ..
python ml/models/maintenance/train.py
python ml/models/leak_detection/train.py

# Start backend
cd backend
uvicorn app.main:app --reload

# Frontend (new terminal)
cd frontend
npm install
npm run dev
```

---

## About

Built by **MikeTheEngr** ([@MikeTheEngrr](https://x.com/MikeTheEngrr)) — AI Engineer & Full Stack Developer.

---

*VIGIL — Industrial AI Monitoring · SN-VGL-001 · OPTICAL SENSOR UNIT*
