# jobflow-backend

> **Note:** This repository contains the standalone backend component of [JobFlow](https://github.com/Prabhuteja799/jobflow). If you want the full system including the dashboard frontend and documentation, see the main repo.

---

## What's here

The FastAPI backend server and n8n automation workflow that power the JobFlow job intelligence pipeline.

```
jobflow-backend/
├── server.py               ← FastAPI entry point (POST /generate-resume)
├── optimizer.py            ← GPT resume rewriter (ATS-optimized)
├── jd_analysis.py          ← Job description analyzer
├── latex_pdf_generator.py  ← LaTeX PDF builder
├── pdf_client.py           ← PDF microservice client
├── drive.py                ← Google Drive upload
├── sheets.py               ← Google Sheets read/write
├── email_sender.py         ← Gmail notification sender
├── seed_gist.py            ← Syncs Sheets data → GitHub Gist
├── config.py               ← Env var loader
├── n8n-workflow.json       ← Full n8n automation (import into n8n)
├── prompts/                ← GPT system + user prompt templates
├── requirements.txt
└── .env.example
```

---

## Quick start

```bash
pip install -r requirements.txt
cp .env.example .env   # fill in your keys
PATH="$PATH:/Library/TeX/texbin" python server.py
# → http://localhost:8000
```

**Full setup instructions:** see [jobflow/README.md](https://github.com/Prabhuteja799/jobflow#setup)

---

## What the pipeline does

Every weekday night, the n8n workflow:
1. Scrapes Java job listings across FL, GA, NY via JSearch API
2. Filters by experience, title seniority, clearance requirements
3. Scores each job against your resume using GPT (0–100 fit score)
4. Routes results to Google Sheets (StrongMatch / MayBe / Weak / No)
5. Sends an email notification when done

On demand, the FastAPI server:
1. Receives a job ID from the dashboard
2. Analyzes the job description with GPT
3. Rewrites your resume to mirror JD keywords (ATS-optimized)
4. Compiles a clean PDF with LaTeX
5. Uploads to Google Drive and writes the link back to Sheets

---

## Tech stack

FastAPI · OpenAI GPT-4o · LaTeX (`pdflatex`) · Google Drive API · Google Sheets API · Gmail API · n8n · Python 3.11
