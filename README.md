# 🔍 Distributed Root Cause Investigator

**AI-Powered Real-Time Root Cause Analysis for Microservice Architectures**

An intelligent observability platform that combines deep learning, causal inference, and graph-based analysis to automatically detect anomalies, trace failure propagation, and pinpoint root causes across distributed systems — reducing Mean Time To Resolution (MTTR) from hours to seconds.

![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-ee4c2c?logo=pytorch&logoColor=white)
![React](https://img.shields.io/badge/React-18+-61dafb?logo=react&logoColor=black)
![Flask](https://img.shields.io/badge/Flask-3.0+-000?logo=flask&logoColor=white)
![D3.js](https://img.shields.io/badge/D3.js-7+-f9a03c?logo=d3.js&logoColor=white)

---

## 🛑 The Problem

Modern distributed architectures (microservices) are incredibly complex. When a single node fails or experiences degraded performance (e.g., a memory leak in an inventory database), the error propagates through the network, triggering a cascade of alerts across dozens of dependent services. 

Traditional monitoring tools only alert you *that* something is wrong, leaving SREs and developers to manually sift through thousands of logs and dashboards to piece together the temporal sequence of events and find the actual root cause. This manual process takes hours, leading to high MTTR and significant business impact.

## 💡 The Solution

The **Distributed Root Cause Investigator** transforms observability from reactive alerting to proactive intelligence. It analyzes telemetry (metrics and logs) across an 11-node microservice topology and tells you **why** a failure happened.

The platform automatically:
1. **Detects anomalies** in real-time using an LSTM Autoencoder trained on normal operating patterns.
2. **Classifies log severity** using a custom Transformer encoder with multi-head self-attention.
3. **Traces failure chains** through the dependency graph using PageRank-weighted propagation analysis.
4. **Infers causal relationships** via cross-correlation lag analysis between service metrics.
5. **Predicts upcoming failures** using linear regression trend extrapolation on metric trajectories.
6. **Matches known patterns** from a failure knowledge base using TF-IDF similarity scoring.
7. **Scores confidence & business impact** of every root cause hypothesis with a multi-signal fusion engine.

---

## 🏗️ System Architecture

```text
┌─────────────────────────────────────────────────────────┐
│                    React + D3.js Frontend               │
│   Service Topology · Anomaly Timeline · Metric Charts   │
│   Root Cause Panel · Causal Graph · Predictive Engine   │
├─────────────────────────────────────────────────────────┤
│                     Flask REST API                      │
├──────────┬──────────┬──────────┬────────────────────────┤
│ Transfrmr│  LSTM    │  Graph   │   Intelligence         │
│ Log      │ Anomaly  │ Analyzer │   Engine               │
│ Embedder │ Detector │(PageRank │ • Causal Inference     │
│ (4-layer,│ (Encoder-│  + BFS)  │ • Predictive Failure   │
│  8-head) │ Decoder) │          │ • Pattern Memory       │
│          │          │          │ • Confidence Scoring   │
├──────────┴──────────┴──────────┴────────────────────────┤
│           Synthetic Telemetry Data Generator            │
│     4 Failure Scenarios · 11 Services · 600s Windows    │
└─────────────────────────────────────────────────────────┘
```

---

## 🤖 AI/ML Models (Built From Scratch in PyTorch)

No heavy external ML dependencies like HuggingFace are used. All models are implemented natively in PyTorch:

| Model | Architecture | Purpose |
|-------|-------------|---------|
| **Log Embedder** | 4-layer Transformer Encoder, 8 attention heads, 128-dim embeddings | Pre-trained with Masked Language Modeling (MLM), fine-tuned for log severity classification |
| **Anomaly Detector** | 2-layer LSTM Autoencoder (64→32 bottleneck) | Learns normal metric patterns; flags high reconstruction error as anomalous (μ + 3σ threshold) |
| **Statistical Ensemble** | Z-Score + EWMA + IQR (weighted fusion) | Complements the LSTM with classical statistical methods for robust detection |
| **Causal Inference** | Cross-correlation with lag analysis | Determines temporal causal relationships between service metric time-series |
| **Predictive Engine** | Linear trend extrapolation with threshold projection | Forecasts when metrics will breach critical thresholds (5-minute horizon) |
| **Pattern Memory** | TF-IDF vectorization + cosine similarity | Matches current failure signatures against a curated knowledge base of known incidents |
| **Confidence Scorer** | Multi-signal weighted fusion (signal + causal + evidence + pattern) | Produces calibrated confidence scores and business impact assessments for root cause hypotheses |

---

## 🔥 Pre-built Failure Scenarios

The system includes 4 pre-built chaos engineering scenarios:

| Scenario | Root Cause | Propagation Chain |
|----------|-----------|-------------------|
| **Memory Leak** | inventory-service | → inventory-db → order-service → notification-service → api-gateway |
| **DB Exhaustion** | user-db | → auth-service → api-gateway |
| **Network Partition**| payment-gateway | → payment-service → order-service |
| **CPU Spike** | search-service | → cache-redis → api-gateway |

---

## ⚙️ Setup Instructions

### Prerequisites
- Python 3.10+
- Node.js 18+

### 1. Start the Backend API & Train Models
Open a terminal in the `backend` directory. We will use the `demo` mode which automatically generates synthetic data, trains the PyTorch models from scratch, and starts the API server.
```bash
cd backend
pip install -r requirements.txt
python run.py --mode demo
```
Wait for the data generation and model training to complete. The API will be available at `http://localhost:5000`. Keep this terminal open.

*Note: If you have already trained the models, you can just start the server with `python run.py --mode serve`.*

### 2. Start the React Frontend Dashboard
Open a new terminal in the `frontend` directory:
```bash
cd frontend
npm install
npm run dev
```
The observability dashboard will be available at `http://localhost:5173` (or as indicated by Vite/React).

---

## 🚀 Usage Guide

Once the system is running, you can access the dashboard to view the generated data and root cause analysis.

### Simulating a Failure

You can trigger a scenario data generation via the API (the `demo` command defaults to `memory_leak`):

Open a new terminal and run:
```bash
cd backend
python run.py --mode generate --scenario cpu_spike
```

**Available Scenarios:** `memory_leak`, `db_connection_exhaustion`, `network_partition`, `cpu_spike`.

### The Analysis Pipeline
1. The **Synthetic Generator** generates normal traffic followed by the scenario's specific failure injection.
2. The **Anomaly Service** flags the LSTM/statistical deviations in the metrics.
3. The **Log Processor** classifies the burst of error logs.
4. The **Graph Analyzer** runs PageRank to trace the propagating anomalies back to their origin.
5. The **Intelligence Engine** validates the causal chain and publishes the final RCA result.

### Viewing the Results
Access the React dashboard at `http://localhost:5173`. 
The dashboard will display:
- **System Overview** — Real-time health status of all 11 microservices.
- **Service Topology** — Interactive D3.js force-directed graph showing service dependencies.
- **Anomaly Timeline** — Temporal heatmap showing anomaly intensity.
- **Root Cause Panel** — AI-generated failure chain with confidence scores and remediation.
- **Causal Inference Graph** — Visual representation of causal relationships.
- **Predictive Failure Engine** — Early warnings for services.

---

## 🗂️ Project Structure

```
├── backend/
│   ├── api/server.py              # Flask REST API (14 endpoints)
│   ├── models/
│   │   ├── log_embedder.py        # Transformer encoder (from scratch)
│   │   ├── anomaly_detector.py    # LSTM Autoencoder (from scratch)
│   │   ├── causal_inference.py    # Cross-correlation causal engine
│   │   ├── predictive_engine.py   # Trend-based failure predictor
│   │   ├── pattern_memory.py      # TF-IDF pattern matcher
│   │   └── confidence_scorer.py   # Multi-signal confidence fusion
│   ├── data/
│   │   ├── synthetic_generator.py # Telemetry data generator
│   │   └── scenarios.py           # Failure scenario definitions
│   ├── graph/analyzer.py          # NetworkX graph analysis (PageRank, BFS)
│   ├── config.py                  # All hyperparameters
│   └── run.py                     # CLI entry point
├── frontend/
│   ├── src/
│   │   ├── App.jsx                # Main dashboard layout
│   │   └── components/            # React components (10+)
│   └── index.html                 # Tailwind CSS configuration
└── knowledge_base/                # Curated failure pattern library
```

---
