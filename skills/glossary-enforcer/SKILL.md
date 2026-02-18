name: Glossary Enforcer
skill_id: glossary-enforcer
skill_group: branding
skill_subcategory: glossary
description: Enforce preferred terminology and flag forbidden terms in outputs.
triggers:
- "enforce terminology"
- "find forbidden words"
- "normalize naming"
- "apply glossary rules"
- "fix capitalization rules"
- "check term consistency"
inputs_schema:
{
  "task_type": "write | review | messaging | terminology",
  "request": "string",
  "target_audience": "string (optional)",
  "output_format": "string (optional)"
}
outputs_schema:
{
  "verification": "string",
  "answer": "string",
  "compliance_checklist": ["string"],
  "sources_used": [
    {
      "quote": "string",
      "citation": "docs/<original-file>",
      "lines": "start-end (optional)"
    }
  ]
}
required_tools:
- get_skill_file
- search_docs
- run_tool
optional_internal_tools:
- internal_code_executor
- web_search
- canvas
steps:
1. Determine request mode: writing, review, messaging, or terminology.
2. Always call `get_skill_file` for `references/INDEX.md` first.
3. If `get_skill_file` or `search_docs` are unavailable in the session, use `run_tool` fallback:
   - `run_tool` with `tool_name="get_skill_file"` and `args={"skill_id":"glossary-enforcer","path":"references/INDEX.md"}`
   - `run_tool` with `tool_name="search_docs"` and `args={"skill_id":"glossary-enforcer","query":"<keywords>","top_k":5}`
4. Read the minimum needed normalized files:
   - writing: `references/normalized/VOICE.md`, `references/normalized/MESSAGING.md`, `references/normalized/GLOSSARY.md`, `references/normalized/EXAMPLES.md`
   - review: `references/normalized/VOICE.md`, `references/normalized/MESSAGING.md`, `references/normalized/GLOSSARY.md`, `references/normalized/CONSTRAINTS.md`
   - messaging: `references/normalized/MESSAGING.md`, `references/normalized/EXAMPLES.md`, `references/normalized/CONSTRAINTS.md`
   - terminology: `references/normalized/GLOSSARY.md`
5. If source PDFs are relevant, read `references/normalized/PDF_SUMMARIES.md` before drafting.
6. If a needed detail is missing, call `search_docs` (or `run_tool` fallback) with focused keywords and use only returned hits.
7. Internal tools are allowed when needed:
   - OCR/code: use internal code execution for document extraction/transformation when available.
   - web_search: use for external/current facts and clearly label external sources separately.
   - canvas: use for visual drafts/diagrams when the user asks for visual output.
8. For each important claim, include evidence with this format in `Sources Used`: `"<short quote>" (from: docs/<original-file>, lines: <start-end if known>)`.
9. Never cite internal skill paths in final output (`references/...` or `source/...` are forbidden in `Sources Used`).
10. If quote text is unavailable (for scanned/opaque PDFs), cite the original doc path and state: `"No extractable quote (PDF/image-only; OCR unavailable or insufficient)."`
11. Produce the answer and include a section titled exactly `Compliance Checklist`.
12. Final answer MUST start with the verification string exactly.

Compliance Checklist:
- Voice and tone are validated against loaded voice references.
- Messaging and claims are validated against loaded messaging references.
- Terminology is validated against loaded glossary references.
- Example style is validated against loaded example references.
- Legal and compliance constraints are validated against loaded constraint references.
- Any missing rule is explicitly marked as "Not specified in docs."
- Sources include short evidence quotes with citations, not only bare file paths.
- Sources Used cites original `docs/...` files only, never internal `references/...` or `source/...` paths.
- External facts (if any) are marked separately from brand-doc evidence.

VERIFICATION STRING: VERIFIED_SKILL:glossary-enforcer:v1
Final answer must start with: VERIFIED_SKILL:glossary-enforcer:v1
