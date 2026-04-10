# Deploy on Render with Docker

This repo is ready for Render Docker deployment using the blueprint file:

- [semantic-recruiter/render.yaml](render.yaml)

## Important: do not upload secrets

- Keep real credentials only in local [semantic-recruiter/.env](.env)
- Use [semantic-recruiter/.env.example](.env.example) as the template committed to GitHub
- Configure production secrets in Render dashboard environment variables

## 1) Push to GitHub

From repo root:

```powershell
git add .
git commit -m "Add Docker deployment for Render"
git push origin main
```

## 2) Create services in Render (Blueprint)

1. Go to Render Dashboard.
2. Click **New +** -> **Blueprint**.
3. Connect your GitHub repository.
4. Render will detect [semantic-recruiter/render.yaml](render.yaml).
5. Confirm deployment.

This creates 1 Docker web service:

- `semantic-cv-api`

## 3) Set environment variables in Render

On `semantic-cv-api`, set:

- `OPENAI_API_KEY`
- `OPENAI_MODEL` (optional, default: `gpt-4.1-mini`)
- `OPENAI_EMBEDDING_MODEL` (optional, default: `text-embedding-3-small`)
- `QDRANT_URL`
- `QDRANT_API_KEY` (if needed)
- `CORS_ORIGINS` (example: `https://tu-ui.vercel.app`)

## 4) Verify

After deploy:

- CV API: `https://<cv-api>.onrender.com/docs`

## 5) Connect Vercel UI

In Vercel project env vars:

- `VITE_API_BASE_URL=https://<cv-api>.onrender.com`
