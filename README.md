# CommitCraft Backend

FastAPI backend for CommitCraft — AI-powered commit message and PR description generator.

## Tech Stack

- **Framework**: FastAPI
- **Package Manager**: uv
- **LLM**: Nemotron 120B via OpenRouter (free tier)
- **Hosting**: Vercel (serverless)

## Project Structure
```
commitcraft/
├── api/
│   └── main.py              # FastAPI app entry + routes
├── core/
│   ├── __init__.py
│   └── config.py            # Settings, env vars
├── services/
│   ├── __init__.py
│   └── llm_service.py       # OpenRouter integration
├── schemas/
│   ├── __init__.py
│   └── requests.py          # Pydantic models
├── prompts/
│   ├── __init__.py
│   └── templates.py         # System prompts
├── .env                     # Local env vars (gitignored)
├── .env.example             # Env template
├── .gitignore
├── pyproject.toml           # uv dependencies
├── requirements.txt         # Vercel deployment
├── vercel.json              # Vercel config
└── README.md
```

## Setup

### Prerequisites

- Python 3.11+
- uv package manager (https://github.com/astral-sh/uv)
- OpenRouter API key (https://openrouter.ai/keys)

### Installation
```bash
git clone https://github.com/yourusername/commitcraft-backend.git
cd commitcraft-backend

uv venv
uv pip install -r requirements.txt

cp .env.example .env
# Add your OPENROUTER_API_KEY to .env
```

### Run Locally
```bash
# Activate venv
# Windows
.venv\Scripts\activate
# Unix/Mac
source .venv/bin/activate

# Run server
uvicorn api.main:app --reload
```

Server runs at http://localhost:8000

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | / | Health check |
| GET | /health | Health status |
| POST | /generate/commit | Generate commit message |
| POST | /generate/pr | Generate PR description |

### Generate Commit Message
```bash
curl -X POST http://localhost:8000/generate/commit \
  -H "Content-Type: application/json" \
  -d '{"description": "Added JWT authentication to user login"}'
```

Response:
```json
{
  "content": "feat(auth): add JWT authentication to user login"
}
```

### Generate PR Description
```bash
curl -X POST http://localhost:8000/generate/pr \
  -H "Content-Type: application/json" \
  -d '{
    "description": "Payment gateway integration with Stripe",
    "diff": "optional git diff here..."
  }'
```

## Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| OPENROUTER_API_KEY | OpenRouter API key | Yes |

## Deployment (Vercel)
```bash
npm i -g vercel
vercel
vercel env add OPENROUTER_API_KEY
vercel --prod
```

## License

MIT
