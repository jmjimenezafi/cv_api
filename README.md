# SemanticRecruiter

SemanticRecruiter is a CV Intelligence API with a React UI.

Current architecture is single-service:
- cv_api: FastAPI backend (OpenAI extraction + OpenAI embeddings + Qdrant storage/search)
- cv_ui: React app (Vite), deployed separately on Vercel

## Project structure

- cv_api/
  - main.py
  - requirements.txt
  - Dockerfile
- cv_ui/
  - src/
  - package.json
  - vercel.json
- render.yaml
- .env.example

## How it works

1. Upload a CV PDF to POST /candidates/ingest.
2. API extracts text from PDF.
3. OpenAI structures candidate data into JSON.
4. OpenAI generates embedding for candidate summary.
5. API stores vector + payload in Qdrant.
6. API supports CRUD, metadata filtering, and semantic or hybrid matching.

## API endpoints

- GET /
- POST /candidates/ingest
- GET /candidates
- GET /candidates/{id}
- PUT /candidates/{id}
- DELETE /candidates/{id}
- GET /candidates/search
- POST /candidates/match

## Environment variables

Use .env.example as template.

Required:
- OPENAI_API_KEY
- QDRANT_URL
- QDRANT_API_KEY

Common optional values:
- OPENAI_MODEL (default: gpt-4o-mini)
- OPENAI_EMBEDDING_MODEL (default: text-embedding-3-small)
- QDRANT_COLLECTION (default: candidates)
- CORS_ORIGINS (comma-separated list)

## Local backend run

From semantic-recruiter/cv_api:

1. Install dependencies
   pip install -r requirements.txt
2. Run API
   python -m uvicorn main:app --reload --port 8000

Open docs at:
http://localhost:8000/docs

## Local frontend run

From semantic-recruiter/cv_ui:

1. Install dependencies
   npm install
2. Create .env file with:
   VITE_API_BASE_URL=http://localhost:8000
3. Run app
   npm run dev

## Deploy backend on Render (Docker)

This repository includes render.yaml configured for a single web service.

1. Push semantic-recruiter to GitHub.
2. In Render, create Blueprint from repository.
3. Render will read render.yaml and create semantic-cv-api.
4. Add secret env vars in Render dashboard:
   - OPENAI_API_KEY
   - QDRANT_URL
   - QDRANT_API_KEY
   - Optional: OPENAI_MODEL, OPENAI_EMBEDDING_MODEL, QDRANT_COLLECTION, CORS_ORIGINS

## Deploy frontend on Vercel

1. Import semantic-recruiter/cv_ui in Vercel.
2. Add env var:
   VITE_API_BASE_URL=https://<your-render-service>.onrender.com
3. Deploy.

## Notes

- .env files are ignored by git via .gitignore.
- If you exposed API keys, rotate them.
- The embedding_service folder can remain in the repository for reference, but it is not required for deployment.
