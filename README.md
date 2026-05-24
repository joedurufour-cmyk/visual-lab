# Visual Lab — Fullstack

## Architecture

```
visual-lab/
├── frontend/          → Cloudflare Pages (static)
│   ├── index.html     → App adapted for cloud mode
│   ├── sw.js          → Service worker (PWA)
│   ├── manifest.json  → PWA manifest
│   └── css/           → Extracted styles (optional)
├── backend/           → Render.com (API)
│   ├── main.py        → FastAPI app
│   ├── .env.example   → Required env vars
│   └── requirements.txt
├── .gitignore
└── README.md
```

## Frontend (Cloudflare)

The frontend is a static PWA. Cloudflare Pages serves it globally.
- All UI, localStorage, and offline logic stays the same
- Added: "Cloud Mode" toggle that sends requests to the Render backend
- API key and backend URL are stored in localStorage (per-device)

## Backend (Render)

FastAPI server with 3 endpoints:
- `POST /api/enhance` — Take a rough intention, return refined Midjourney prompt
- `POST /api/translate` — Take Spanish/hybrid input, return structured English prompt
- `POST /api/variations` — Take base prompt + variable sets, return N generated prompts

The backend holds the LLM API key (Kimi/OpenRouter). Never exposed to frontend.

## LLM Connection

The backend calls Kimi API (Moonshot AI) using your API key. 
This is NOT calling OpenClaw directly — it's calling the Kimi LLM API.
You get an API key from platform.moonshot.cn.

## Deploy

1. Push this repo to GitHub
2. Connect Render to the `backend/` folder (or root with a `render.yaml`)
3. Connect Cloudflare Pages to the `frontend/` folder
4. Set env vars in Render dashboard: `KIMI_API_KEY`, `API_SECRET`
5. Put the Render URL + API_SECRET into the app's Settings tab

## Local Dev

```bash
cd backend
python3 -m venv venv && source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
# Edit .env with your keys
uvicorn main:app --reload --port 8000
```

Frontend dev: just open `frontend/index.html` in a browser or serve with `python3 -m http.server 8080`.
