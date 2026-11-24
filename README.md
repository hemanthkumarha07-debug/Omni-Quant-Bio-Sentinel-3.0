# 🛡️ Omni-Quant-Bio-Sentinel 3.0
### *An AI-Driven Biometric-Locked Multi-Market Intelligence Ecosystem*

> Final Year Project — Built with Django, Local LLaMA 3.2 RAG, OpenCV/dlib Biometrics & Real-Time Quantitative Analytics.

---

## 📌 Overview

**Omni-Quant-Bio-Sentinel (OQBS) 3.0** is a full-stack AI-powered quantitative trading intelligence platform that fuses **facial liveness biometric authentication** with a **local RAG (Retrieval-Augmented Generation)** engine to deliver multi-market insights across Stocks, Forex, Crypto, Commodities, Options, and Bonds — all without sending your data to any cloud AI.

The system is designed around three core philosophies:
- 🔐 **Security First** — No one enters without passing a liveness biometric challenge
- 🧠 **AI with Integrity** — LLaMA 3.2 interprets; Python calculates. Never the other way around.
- ⚡ **Speed & Determinism** — Technical indicators are computed locally, fast, and reproducibly

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                   OQBS 3.0 Platform                     │
├──────────────┬──────────────────────────┬───────────────┤
│  Bio-Sentinel│     Omni-Quant Core      │  Markets Hub  │
│  (sentinel/) │       (core/)            │  (markets/)   │
│              │                          │               │
│ OpenCV/dlib  │  LLaMA 3.2 (Ollama)     │ Angel One API │
│ 68 Landmarks │  ChromaDB RAG            │ SmartAPI      │
│ Head-Turn    │  Market Engine           │ Options Greeks│
│ Liveness     │  Stock Ranker (Top 10)   │ B-S Model     │
│ Challenge    │  Multi-Asset AI Router   │               │
└──────────────┴──────────────────────────┴───────────────┘
```

---

## ✨ Key Features

### 🔐 Bio-Sentinel — Biometric Access Control
- Real-time **face detection** using `dlib` with 68-point facial landmark model
- **Head-turn liveness challenge** (not a static photo — must turn left or right)
- Anti-spoofing via minimum face-size ratio and stable-frame gating
- 25-second challenge timeout with automatic session reset

### 🧠 Omni-Quant AI Engine
- **Local RAG pipeline** — LLaMA 3.2 via Ollama + ChromaDB + HuggingFace Embeddings
- Knowledge base pre-loaded with 10+ trading classics (Murphy, Al Brooks, Mark Douglas, etc.)
- Specialist AI agents for each market segment with dedicated rule sets:
  | Segment | AI Focus |
  |---------|----------|
  | Stocks | Momentum, volume gaps, technical breakouts |
  | Forex | Macro-liquidity, base currency strength |
  | Crypto | Extreme volatility, on-chain sentiment |
  | Commodities | Supply-demand shifts, futures pricing |
  | Options | Implied volatility, premium decay |
  | Bonds | Yield curve, central bank rate expectations |

### 📊 Quantitative Market Engine
- Python-computed technical indicators (LLM **never** calculates these):
  - RSI (Wilder-style exponential smoothing)
  - EMA 20 / EMA 50
  - MACD
  - Volume Analysis
  - Support & Resistance levels
  - Trend detection
  - Composite **Quant Score**
- **Top 10 Stock Ranker** — multi-threaded scanning of Indian equities universe (NSE)
- Concurrent `ThreadPoolExecutor` for fast parallel data fetching via `yfinance`

### 📈 Markets Hub
- **Options Greeks** via Black-Scholes model: Delta, Gamma, Theta
- **Angel One SmartAPI** integration with TOTP-based authentication
- Multi-asset symbol maps: Stocks, Forex pairs (INR-based), Crypto, Commodities, Bonds
- TradingView chart integration

### 🖥️ Command Center
- In-browser terminal interface for direct system interaction
- Real-time AI sync via JavaScript (`ai_sync.js`, `options_sync.js`)
- Dashboard with live market data feeds

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Backend Framework | Django 6.1.1 |
| AI Inference | Ollama + LLaMA 3.2 |
| Vector Database | ChromaDB |
| Embeddings | `sentence-transformers/all-MiniLM-L6-v2` |
| Biometrics | OpenCV + dlib + 68-point landmark model |
| Market Data | yfinance, Angel One SmartAPI |
| Options Pricing | Black-Scholes (SciPy, NumPy) |
| Auth | pyotp (TOTP), Django Auth |
| Containerization | Docker |
| Language | Python 3.14.x |

---

## ⚙️ Installation & Setup

### Prerequisites

| Requirement | Notes |
|-------------|-------|
| Python 3.14.x | Tested local version |
| [Ollama](https://ollama.com/download) | Local LLM runtime |
| `llama3.2` model | Pull via Ollama |
| `shape_predictor_68_face_landmarks.dat` | dlib landmark model (place in project root) |
| Angel One SmartAPI account | For live market data |

### 1. Clone the Repository
```bash
git clone https://github.com/hemanthkumarha07-debug/Omni-Quant-Bio-Sentinel-3.0.git
cd Omni-Quant-Bio-Sentinel-3.0
```

### 2. Install Ollama & Pull LLaMA
```bash
# Download from https://ollama.com/download
ollama pull llama3.2
```

### 3. Install Python Dependencies
```bash
pip install -r requirements.txt
```

### 4. Place the dlib Landmark Model
Download `shape_predictor_68_face_landmarks.dat` and place it in the **project root** (same level as `manage.py`).
> You can get it from: http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2

### 5. Configure API Credentials
Edit `core/views.py` and set your Angel One SmartAPI credentials:
```python
SMART_API_KEY     = "YOUR_SMART_API_KEY"
SMART_CLIENT_ID   = "YOUR_SMART_CLIENT_ID"
SMART_PASSWORD    = "YOUR_SMART_PASSWORD"
SMART_TOTP_SECRET = "YOUR_SMART_TOTP_SECRET"
```
> ⚠️ **Never commit real credentials to GitHub.** Use environment variables in production.

### 6. Run Database Migrations
```bash
python manage.py migrate
```

### 7. Train the RAG Knowledge Base
```bash
python engine/trainer.py
```

### 8. Launch the Platform
```bash
python manage.py runserver
```
Access at: **http://127.0.0.1:8000**

---

## 🐳 Docker Deployment

```bash
docker build -t oqbs3 .
docker run -p 8000:8000 oqbs3
```
> Ensure Ollama is running and accessible from within the container. Mount the `.dat` file and configure credentials via environment variables.

---

## 📂 Project Structure

```
Omni-Quant-Bio-Sentinel-3.0/
│
├── config/              # Django project settings, URLs, WSGI/ASGI
├── core/                # Main intelligence engine
│   ├── ai_agents.py     # OmniQuantSpecialist — multi-market AI agents
│   ├── ai_pipeline.py   # RAG pipeline integration
│   ├── ai_router.py     # LLaMA inference router (thread-safe, cached)
│   ├── market_engine.py # Technical indicator engine (RSI, MACD, EMA...)
│   ├── stock_ranker.py  # Top 10 Indian stocks ranker (concurrent)
│   ├── stock_universe.py# NSE stock universe
│   ├── news_api.py      # News data integration
│   ├── chart_api.py     # Chart data APIs
│   └── views.py         # Core views and symbol maps
│
├── sentinel/            # Biometric authentication module
│   ├── biometrics.py    # BioSentinelEngine — liveness challenge
│   └── views.py         # Auth views
│
├── markets/             # Markets hub
│   ├── options_engine.py# Black-Scholes Options Greeks
│   └── services/
│       └── angel_auth.py# Angel One SmartAPI authentication
│
├── command_center/      # In-browser terminal
│   └── templates/       # Terminal UI
│
├── engine/
│   ├── trainer.py       # RAG knowledge base trainer
│   └── knowledge/       # Trading book PDFs for RAG ingestion
│
├── static/js/           # Frontend JS (AI sync, dashboard, options, sentinel)
├── chroma_db_backup/    # ChromaDB vector store backup
├── Dockerfile
├── requirements.txt
└── manage.py
```

---

## 📚 RAG Knowledge Base

The AI engine is grounded in the following trading literature:

| Book | Author |
|------|--------|
| Technical Analysis of the Financial Markets | John J. Murphy |
| Intermarket Analysis | John Murphy |
| Encyclopedia of Chart Patterns | Thomas Bulkowski |
| Trading in the Zone | Mark Douglas |
| Reading Price Charts Bar by Bar | Al Brooks |
| Currency Trading & Intermarket Analysis | Ashraf Laïdi |
| Option Volatility and Pricing | Sheldon Natenberg |
| Hot Commodities | Jim Rogers |
| The Bitcoin Standard | Saifedean Ammous |
| Building Winning Algorithmic Trading Systems | Kevin Davey |

---

## ⚠️ Disclaimer

> This platform is a **final year academic project** built for educational and research purposes only. It is **not** financial advice. Do not use this system to make real investment decisions without proper due diligence. The developer is not responsible for any financial losses.

---

## 👨‍💻 Author

**Hemanth Kumar H A**
- GitHub: [@hemanthkumarha07-debug](https://github.com/hemanthkumarha07-debug)
- Project: Omni-Quant-Bio-Sentinel 3.0 — Final Year Project

---

## 📄 License

This project is for academic and educational use. All rights reserved © 2026 Hemanth Kumar H A.

---

<div align="center">
  <strong>Built with 🧠 AI · 🔐 Biometrics · 📊 Quantitative Finance</strong>
</div>
