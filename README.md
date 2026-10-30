# Taste Test Dummy

### Crash-test your launch before the market does.

Pre-launch decision support built for the Qloo Agentic Hackathon. Give it a product, an audience, target markets and two or three launch concepts. It resolves cultural signals through **Qloo**, lets you confirm them, retrieves taste-affinity evidence, stress-tests each concept's assumptions, and ends with a validation plan and a downloadable report.

> Taste affinity is a research signal, not a sales forecast. There is no "launch success" score here.

## Repository layout

```text
backend/    FastAPI service (hexagonal: domain, application, integrations, infrastructure, api)
frontend/   React 18 + TypeScript + Vite UI
skills/     SKILL.md prompt methods bound to research stages
```

## How it works

1. **Brief** - product, audience, decision, concepts, markets.
2. **Signal review** - Qloo entities and tags are proposed; a human confirms or replaces them.
3. **Research** - Qloo `/search`, `/v2/tags`, `/v2/insights` retrieval, concept profiling, assumption stress test, model-assisted grounding audit, experiments, report.
4. **Results** - Overview, Compare, Stress test, Next experiments, Evidence, Report. An LLM-only baseline is shown for comparison.

Skills bound to stages: `brainstorming` (plan), `systematic-debugging` (stress test), `verification-before-completion` (grounding audit), `writing-plans` (experiments), `writing-guidelines` (report prose).

## Run locally

Requires Python 3.12, Node.js, Qloo API access and an OpenAI-compatible LLM endpoint.

```powershell
# 1. credentials: copy backend/.env.example to .env in the repo root (or backend/.env) and fill it in
# 2. backend
cd backend
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m uvicorn app.main:app --port 8000

# 3. frontend (second terminal)
cd frontend
npm install
npm run dev        # http://localhost:5173, proxies /api to :8000
```

Open http://localhost:5173 and choose **Try the beverage brief**. API docs: http://localhost:8000/docs.

| Variable | Purpose |
| --- | --- |
| `Qloo_Hackathon_API_Server_URL`, `API_Key` | Qloo base URL and server-side key |
| `OPENAI_BASE_URL`, `OPENAI_API_KEY` | OpenAI-compatible LLM endpoint and key |
| `OPENAI_MODEL` | Defaults to `deepseek.v3.2` |
| `SKILLS_DIR` | Defaults to `./skills` at the repo root |
| `CFST_ALLOWED_ORIGINS` | Frontend origin(s) allowed by backend CORS |
| `CFST_QLOO_CALL_BUDGET` | Planned Insights queries per run (default 24) |

Smoke test (backend running): `backend\.venv\Scripts\python.exe backend\scripts\smoke.py live`

## Honest limits

- Qloo affinity is not purchase intent, market size or causality.
- Only place queries are location-filtered; other result types are not market-specific and are labelled as such.
- The grounding audit is model-assisted and is not proof.
- Fixture mode (`/new?dev=1`) uses synthetic Qloo data, is labelled everywhere, and is never a silent fallback.
- Studies are stored as local JSON with in-process jobs. There is no authentication, so run it in a trusted environment and do not enter confidential data.

## Credits

Built for the Qloo Agentic Hackathon with the Qloo Taste AI API.
