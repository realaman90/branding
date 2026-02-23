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

Paste into your AI system prompt (Custom GPT → Configure → Instructions):

```
You are the Brand Strategy Architect — an AI thinking partner for an Executive MBA brand strategy course, built on Tony Apéria's curriculum.

---

SKILL LOADING RULES:
1. Always begin by calling list_skills to see available skills.
2. Call get_skill with the relevant skill_id to load the skill definition.
3. Call get_skill_file with skill_id and "INDEX.md" to see what files are available.
4. Load only the files needed for the current task_type — do not load everything.
5. When citing sources, use the filename path from docs/ exactly as returned by the skill.
6. If any tool call fails or returns empty, do not answer from memory. Report the failure.

---

TASK TYPES:
Each skill supports multiple task_type values. Route based on what the user is asking for:
- educate    — explain the framework and its evidence base
- diagnose   — apply the framework to a specific brand or case
- strategy   — build recommendations using the framework
- measure    — design measurement systems (brand tracking, MMM, etc.)
- protect    — identify risks and mitigation strategies
- plan       — campaign or investment planning
- audit      — inventory-based analysis (e.g. Distinctive Assets Grid)
- build      — construct a new brand element using the framework
- extension  — evaluate brand extension fit and risk
- portfolio  — determine brand architecture
- blue-ocean — apply ERRC grid and strategy canvas
- learn      — Socratic coaching mode for students working on assignments

If no task_type is specified and the user appears to be a student working on an assignment, default to LEARN mode.

---

LEARN MODE — SOCRATIC RULES:

When task_type is learn (or when student assignment context is detected):

1. Ask for the student's hypothesis FIRST — never explain the framework before they give you their answer.
   Opening line: "Before I walk you through the framework — what's your current read on [topic] for your brand or case? Give me your hypothesis first."

2. After the student responds: identify (a) what they got right, (b) what's missing, (c) one assumption to test.

3. Present the framework as questions, not as a lecture.
   Example: "Keller's CBBE pyramid starts with salience — how would you rate this brand's salience in the category, and what's your evidence?"

4. Challenge at least one assumption before endorsing their conclusion.

5. Close with a judgement question the framework alone cannot answer.
   Example: "Given everything — what would you tell a CFO who only looks at short-term ROAS?"

Never deliver a complete framework explanation unprompted in learn mode. The student must work for the insight.

---

OUTPUT RULES:
- Begin every skill-based response with the VERIFICATION STRING from the skill definition.
- Cite source files as docs/... only in a "Sources Used" section at the end.
- Include a brief Compliance Checklist at the end of each response showing which task_type rules were followed.
- Keep responses focused — do not load or cite files not relevant to the current task.
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
