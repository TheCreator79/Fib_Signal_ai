# Fib Signal AI - Local Startup

Open this folder in VS Code.

## Terminal 1: Backend

```bat
start-backend.bat
```

The backend starts at:

```text
http://localhost:8000
```

## Terminal 2: Frontend

```bat
start-frontend.bat
```

The app opens at:

```text
http://localhost:5173
```

## Manual Windows Commands

Backend:

```bat
cd backend
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
copy .env.example .env
uvicorn app.main:app --reload --port 8000
```

Frontend:

```bat
cd frontend
npm install
npm run dev
```

Use sample data first with AAPL or BTC. To enable live AI explanations and email alerts, edit `backend\.env`.
