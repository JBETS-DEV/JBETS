# 🎯 JBETS Elite - Sports Betting Prediction Portal

### *"Watch passionately. Bet Wisely!!"*

[![Win Rate](https://img.shields.io/badge/win%20rate-87.7%25-gold)](docs/accuracy.md) [![ROI](https://img.shields.io/badge/ROI-+68.2%25-success)]() [![Predictions](https://img.shields.io/badge/total%20predictions-270-blue)]()

## 📊 Current Performance

| Metric | Value |
|--------|-------|
| **Overall Record** | 235-33-2 (87.7%) |
| **ROI** | +68.2% |
| **Bankroll** | $11,500 |
| **Current Streak** | 3W |
| **📊 Breakdown** | Team: 176-23 • Props: 47-7 • Parlays: 12-3 |

## 🚀 Features

- ✅ Automated Daily Predictions (6 AM ET)
- ✅ Real-time Results Tracking (15 min updates)
- ✅ Multi-Sport Coverage: NHL, NBA, MLB, Tennis, Soccer, UFC
- ✅ Player Props System (47-7 record, 87.0%)
- ✅ Parlay Builder with Optimized Legs
- ✅ Kelly Criterion Bankroll Management
- ✅ 3-Source Verification System
- ✅ Excel Bidirectional Sync
- ✅ 📱 Mobile-First Responsive Design (All sheets optimized)

## 🛠️ Tech Stack

**Backend:** FastAPI + PostgreSQL + Redis + Celery  
**Frontend:** Next.js 14 + Tailwind CSS + Socket.io  
**Hosting:** Vercel (frontend) + Railway (backend)

## 🏃 Quick Start

\```bash
# Clone and start with Docker
git clone https://github.com/yourusername/jbets-portal.git
cd jbets-portal
docker-compose up -d

# Visit http://localhost:3000 (frontend)
# API at http://localhost:8000 (backend)
\```

## ⚙️ Environment Variables

Create `.env` in backend/ and frontend/:

\```env
# Backend
DATABASE_URL=postgresql://jbets:jbets@localhost:5432/jbets
REDIS_URL=redis://localhost:6379
SECRET_KEY=your-secret-key
ODDS_API_KEY=your-odds-api-key

# Frontend (.env.local)
NEXT_PUBLIC_API_URL=http://localhost:8000
NEXT_PUBLIC_WS_URL=ws://localhost:8000/ws
\```

## 📡 Key API Endpoints

| Endpoint | Description |
|----------|-------------|
| GET /api/predictions/today | Today's picks |
| GET /api/stats/overview | Overall stats |
| GET /api/live/games | Live games |
| WS /ws/updates | Real-time updates |

## 📱 Mobile-First Design

All sheets and views are optimized for mobile viewing with:
- Responsive layouts that adapt to screen size
- Touch-friendly navigation
- Condensed data views for smaller screens
- Quick-tap access to key features

---

*Powered by JBETS Decision Intelligence — High Class Platinum Edition*

**✨ "Watch passionately. Bet Wisely!!" ✨**
Scroll down and click "Commit changes"
Add commit message: Initial README with signature and mobile directive
Click "Commit changes"
⚠️ Note: Remove the backslashes (\) before the triple backticks in the code blocks. They are only there to prevent formatting issues in these instructions.

Step 3: Create the Folder Structure
From your repository main page, click "Add file" → "Create new file"
Type: backend/requirements.txt
Paste this content:
fastapi==0.109.0
uvicorn[standard]==0.27.0
sqlalchemy==2.0.25
alembic==1.13.1
redis==5.0.1
celery==5.3.4
pandas==2.2.0
openpyxl==3.1.2
httpx==0.26.0
pydantic==2.5.3
python-dotenv==1.0.0
websockets==12.0
pytest==7.4.4
Click "Commit changes"
Step 4: Create Backend Dockerfile
Click "Add file" → "Create new file"
Type: backend/Dockerfile
Paste this content:
FROM python:3.11-slim

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . .

EXPOSE 8000
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
Click "Commit changes"
Step 5: Create Frontend Dockerfile
Click "Add file" → "Create new file"
Type: frontend/Dockerfile
Paste this content:
FROM node:18-alpine

WORKDIR /app

# Install dependencies
COPY package*.json ./
RUN npm ci

# Copy application code
COPY . .

# Build for production
RUN npm run build

EXPOSE 3000
CMD ["npm", "start"]
Click "Commit changes"
Step 6: Create Docker Compose File
Click "Add file" → "Create new file"
Type: docker-compose.yml
Paste this content:
version: '3.8'

services:
  postgres:
    image: postgres:15-alpine
    container_name: jbets-postgres
    environment:
      POSTGRES_USER: jbets
      POSTGRES_PASSWORD: jbets_secure_password
      POSTGRES_DB: jbets
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - '5432:5432'
    healthcheck:
      test: ['CMD-SHELL', 'pg_isready -U jbets']
      interval: 10s

  redis:
    image: redis:7-alpine
    container_name: jbets-redis
    ports:
      - '6379:6379'
    healthcheck:
      test: ['CMD', 'redis-cli', 'ping']

  backend:
    build: ./backend
    container_name: jbets-backend
    environment:
      DATABASE_URL: postgresql://jbets:jbets_secure_password@postgres:5432/jbets
      REDIS_URL: redis://redis:6379
      SECRET_KEY: ${SECRET_KEY:-change-me}
      ODDS_API_KEY: ${ODDS_API_KEY}
    ports:
      - '8000:8000'
    depends_on:
      - postgres
      - redis
    command: uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload

  celery-worker:
    build: ./backend
    container_name: jbets-celery-worker
    environment:
      DATABASE_URL: postgresql://jbets:jbets_secure_password@postgres:5432/jbets
      REDIS_URL: redis://redis:6379
    depends_on:
      - backend
    command: celery -A app.celery_app worker --loglevel=info

  frontend:
    build: ./frontend
    container_name: jbets-frontend
    environment:
      NEXT_PUBLIC_API_URL: http://localhost:8000
      NEXT_PUBLIC_WS_URL: ws://localhost:8000/ws
    ports:
      - '3000:3000'
    depends_on:
      - backend

volumes:
  postgres_data:
  redis_data:
Click "Commit changes"
Step 7: Create Seed Data File
Click "Add file" → "Create new file"
Type: scripts/seed_data.json
Paste this content:
{
  "predictions": [
    {
      "id": "PRED-001",
      "date": "2026-05-07",
      "sport": "NBA",
      "league": "Playoffs R2",
      "home": "Pistons",
      "away": "Cavaliers",
      "pick": "DET -5.5",
      "odds": -110,
      "confidence": 0.72,
      "result": "PENDING"
    },
    {
      "id": "PRED-003",
      "date": "2026-05-06",
      "sport": "NBA",
      "league": "Playoffs R2",
      "home": "Knicks",
      "away": "76ers",
      "pick": "NYK -5.5",
      "odds": -110,
      "confidence": 0.70,
      "result": "WIN",
      "score": "118-98"
    },
    {
      "id": "PRED-004",
      "date": "2026-05-06",
      "sport": "NBA",
      "league": "Playoffs R2",
      "home": "Spurs",
      "away": "Wolves",
      "pick": "SAS -7.5",
      "odds": -110,
      "confidence": 0.72,
      "result": "WIN",
      "score": "119-102"
    }
  ],
  "player_props": [
    {
      "id": "PROP-001",
      "player": "Shai Gilgeous-Alexander",
      "team": "OKC",
      "prop_type": "Points",
      "line": 30.5,
      "pick": "Over",
      "odds": -115,
      "confidence": 0.75
    },
    {
      "id": "PROP-002",
      "player": "Victor Wembanyama",
      "team": "SAS",
      "prop_type": "Points+Rebounds",
      "line": 32.5,
      "pick": "Over",
      "odds": -110
    }
  ],
  "sport_stats": [
    {"sport": "NHL", "wins": 38, "losses": 2, "win_rate": 0.95},
    {"sport": "NBA", "wins": 96, "losses": 13, "win_rate": 0.881},
    {"sport": "MLB", "wins": 5, "losses": 1, "win_rate": 0.833},
    {"sport": "Tennis", "wins": 4, "losses": 1, "win_rate": 0.80}
  ],
  "bankroll_history": [
    {"date": "2026-04-01", "balance": 5000},
    {"date": "2026-05-01", "balance": 9800},
    {"date": "2026-05-18", "balance": 11500}
  ]
}
Click "Commit changes"
Step 8: Create Environment Template
Click "Add file" → "Create new file"
Type: .env.example
Paste this content:
# Backend Environment Variables
DATABASE_URL=postgresql://jbets:jbets@localhost:5432/jbets
REDIS_URL=redis://localhost:6379
SECRET_KEY=your-secret-key-change-this
ODDS_API_KEY=your-odds-api-key

# Frontend Environment Variables (create as .env.local)
NEXT_PUBLIC_API_URL=http://localhost:8000
NEXT_PUBLIC_WS_URL=ws://localhost:8000/ws
