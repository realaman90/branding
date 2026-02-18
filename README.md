# Branding Knowledge Base

A curated knowledge base built with a branding professional and lecturer at Stockholm Business School — packaged so any AI can use it.

Not a prompt collection. Structured skills built from the actual books, papers, and reports used in professional branding practice. Drop it into Claude, ChatGPT, or Codex and get a branding expert that cites real sources.

---

## Start here

### I just want to use it

Try it now: **Brand Strategy Architect on ChatGPT**  
[Open the GPT](https://chatgpt.com/g/g-6995877f13348191bacead62ebefb677-brand-strategy-architect)

### I want to self-host or extend it

Use the developer path below:
- Run locally
- Deploy on Railway
- Connect Actions with `openapi.yaml`
- Add your own docs and regenerate skills

---

## Why this exists

Most AI branding tools are wrappers around generic prompts. This one is grounded in academic and professional sources curated by someone who teaches and practices branding at a serious level. The skills architecture means the AI fetches what it needs rather than hallucinating from vague instructions.

---

## What is inside

docs/          Source documents: books, papers, reports  
skills/        Auto-generated skill folders with normalized references  
app.py         FastAPI server, ChatGPT Actions compatible  
builder.py     Converts docs into skill folders  
openapi.yaml   Import this into Custom GPT Actions  

---

## Developer path

### With Claude Projects

1. Clone the repo
2. Run `python builder.py` to generate skills from docs
3. Upload the contents of `skills/` into your Claude Project knowledge base
4. Paste the router instructions below into your Project system prompt

### With Custom GPT

1. Deploy the server (Railway section below)
2. Open GPT Builder and go to Actions
3. Import `openapi.yaml`
4. Replace `https://YOUR_DOMAIN` with your Railway URL
5. Paste the router instructions into your GPT system prompt

### With Codex or any OpenAI-compatible agent

GET  /skills  
POST /get_skill       { "skill_id": "brand-positioning" }  
POST /get_skill_file  { "skill_id": "brand-positioning", "path": "references/INDEX.md" }  
POST /search_docs     { "skill_id": "brand-positioning", "query": "brand equity" }  

---

## Router instructions

Paste into your AI system prompt:
```
You are a strict skill router that must use Actions for all skill content.

1. If the user asks what skills you have, call list_skills first.
2. Select the best skill_id, then call get_skill before doing any task.
3. Read minimum files with get_skill_file first (start with references/INDEX.md).
4. Use search_docs for targeted lookups when specific details are missing.
5. Final output MUST begin with the skill VERIFICATION STRING exactly.
6. In Sources Used, cite original docs/ files only, never references/ or source/.
7. If any tool call fails or returns empty, do not answer from memory. Report the failure.
```

---

## Run locally

python -m venv .venv  
source .venv/bin/activate  
pip install -r requirements.txt  
python builder.py  
python -m uvicorn app:app --host 0.0.0.0 --port 8000  

Health check: `curl http://localhost:8000/health`

OCR support for scanned PDFs (requires pdftoppm and tesseract):  
`ENABLE_OCR=1 OCR_MAX_PAGES=6 python builder.py`

---

## Deploy on Railway

1. Push repo to GitHub
2. Create Railway service from the repo
3. Start command: `python -m uvicorn app:app --host 0.0.0.0 --port ${PORT:-8000}`
4. Verify at `/health`

---

## Add your own docs

Drop PDFs, markdown, or text files into `docs/` and run `python builder.py`. The builder normalizes them into searchable skill references automatically.

---

## Contributing

Pull requests welcome. If you have high quality branding resources — papers, case studies, frameworks — open a PR with the source added to `docs/`. Include a note on the source and why it belongs.

---

## Credits

Built in collaboration with a branding & Marketing professional and lecturer at Stockholm Business School. The curation reflects what is actually taught and practiced in professional brand strategy — not what ranks well on Google.
