# Setup Guide

## Prerequisites
- **Node.js** 18+ (for frontend)
- **Python** 3.10+ (for backend)
- **Git**

---

## 1. Clone & Configure

```bash
git clone https://github.com/ISTE-VESIT-ORG/SR-21-Invictus.git
cd SR-21-Invictus
cp .env.example .env
```

Edit `.env` and fill in your keys (see sections below), then copy it to both apps:

```bash
cp .env backend/.env
cp .env frontend/.env
```

---

## 2. Supabase Setup

1. Go to [supabase.com](https://supabase.com) → Create a new project
2. Once created, go to **Settings → API** and copy:
   - `Project URL` → `SUPABASE_URL`
   - `anon public` key → `SUPABASE_ANON_KEY`
   - `service_role` key → `SUPABASE_SERVICE_KEY`
3. Go to **SQL Editor** → paste contents of [`supabase/schema.sql`](./supabase/schema.sql) → Run
4. Enable **Row Level Security** (already included in the schema)

---

## 3. Firebase Setup

1. Go to [console.firebase.google.com](https://console.firebase.google.com) → Create a new project
2. Enable **Authentication** → Sign-in method:
   - ✅ Email/Password
   - ✅ Google
3. Go to **Project Settings → General** → scroll to "Your apps" → Add a **Web app**
4. Copy the config object → fill into `.env`:
   - `VITE_FIREBASE_API_KEY`
   - `VITE_FIREBASE_AUTH_DOMAIN`
   - `VITE_FIREBASE_PROJECT_ID`
   - `VITE_FIREBASE_APP_ID`
5. Go to **Project Settings → Service accounts** → Generate new private key → save as `backend/firebase-service-account.json`

---

## 4. Backend

```bash
cd backend
python -m venv venv

# Windows
venv\Scripts\activate
# Mac/Linux
source venv/bin/activate

pip install -r requirements.txt
uvicorn main:app --reload --port 8000
```

Backend runs at `http://localhost:8000`. API docs at `http://localhost:8000/docs`.

---

## 5. Frontend

```bash
cd frontend
npm install
npm run dev
```

Frontend runs at `http://localhost:5173`.

---

## 6. Verify

1. Open `http://localhost:5173`
2. Register a new account
3. Create a ledger → add some rows
4. Check the dashboard

---

## Environment Variables Reference

| Variable | Where | Description |
|----------|-------|-------------|
| `SUPABASE_URL` | Backend | Supabase project URL |
| `SUPABASE_ANON_KEY` | Backend | Supabase anon/public key |
| `SUPABASE_SERVICE_KEY` | Backend | Supabase service role key |
| `FIREBASE_PROJECT_ID` | Backend | Firebase project ID |
| `VITE_FIREBASE_API_KEY` | Frontend | Firebase web API key |
| `VITE_FIREBASE_AUTH_DOMAIN` | Frontend | Firebase auth domain |
| `VITE_FIREBASE_PROJECT_ID` | Frontend | Firebase project ID |
| `VITE_FIREBASE_APP_ID` | Frontend | Firebase app ID |
| `VITE_API_BASE_URL` | Frontend | Backend URL (default: http://localhost:8000) |
