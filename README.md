# Fib Signal AI

Private MVP web app for beginner-friendly Fibonacci price signals across stocks and crypto.

## What It Does

Enter a stock ticker like `AAPL` or a crypto symbol like `BTC`, save symbols to a watchlist, view a TradingView chart, and create email alerts.

The backend pulls recent prices, analyzes the latest price range, calculates Fibonacci-based price zones internally, and returns simple dollar-based signals:

- Current price
- Possible buy zone
- Stop loss
- Sell target 1
- Sell target 2
- Risk level
- Plain-English explanation
- TradingView chart
- Stock and crypto watchlists
- Email alerts

The user-facing dashboard intentionally avoids technical ratio labels.

## Project Structure

```text
fib-signal-ai/
  backend/
    app/
      main.py
      models.py
      services/
        db.py
        email_alerts.py
        fib.py
        market_data.py
        explanations.py
    sample_data/
      AAPL.json
      BTC.json
    requirements.txt
    .env.example
  frontend/
    src/
      App.jsx
      api.js
      main.jsx
      styles.css
    index.html
    package.json
    vite.config.js
  README.md
```

## Backend Setup

```bash
cd backend
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
copy .env.example .env
uvicorn app.main:app --reload --port 8000
```

On Windows, you can also run:

```bat
start-backend.bat
```

Set `OPENAI_API_KEY` in `backend/.env` to enable AI-generated explanations. Without a key, the app returns a built-in plain-English explanation.

SQLite is used automatically. By default, the database file is `backend/fib_signal_ai.db` when you start the server from the `backend` folder. You can override it with:

```env
SQLITE_DB_PATH=fib_signal_ai.db
```

To enable email alerts, set SMTP values in `backend/.env`:

```env
SMTP_HOST=smtp.example.com
SMTP_PORT=587
SMTP_USERNAME=you@example.com
SMTP_PASSWORD=your-password
ALERT_FROM_EMAIL=alerts@example.com
```

If SMTP is not configured, alerts can still be saved, but emails will not be sent.

## Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

On Windows, you can also run:

```bat
start-frontend.bat
```

The frontend runs on `http://localhost:5173` and talks to the backend at `http://localhost:8000`.

## API Example

```bash
curl "http://localhost:8000/api/signal?symbol=AAPL&asset_type=stock"
curl "http://localhost:8000/api/signal?symbol=BTC&asset_type=crypto"
```

Use sample data without network calls:

```bash
curl "http://localhost:8000/api/signal?symbol=AAPL&asset_type=stock&use_sample=true"
```

Watchlists:

```bash
curl "http://localhost:8000/api/watchlist"
curl -X POST "http://localhost:8000/api/watchlist" ^
  -H "Content-Type: application/json" ^
  -d "{\"symbol\":\"MSFT\",\"asset_type\":\"stock\",\"notes\":\"Main watchlist\"}"
```

Email alerts:

```bash
curl "http://localhost:8000/api/alerts"
curl -X POST "http://localhost:8000/api/alerts" ^
  -H "Content-Type: application/json" ^
  -d "{\"email\":\"you@example.com\",\"symbol\":\"AAPL\",\"asset_type\":\"stock\",\"event_type\":\"buy_zone\",\"enabled\":true}"
curl -X POST "http://localhost:8000/api/alerts/check?use_sample=true"
```

Alert event types are `all`, `buy_zone`, `target_1`, `target_2`, and `stop_loss`. Emails use beginner-friendly wording such as "price entered the possible buy zone" and "sell target 1 was reached."

## Notes

This MVP is educational software, not financial advice. The generated signals are simplified examples based on recent price action.
