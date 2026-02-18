skill_id: notes
description: Save notes and list notes using tool calls only.
triggers:
- "save note"
- "remember this"
- "list notes"
required_tools:
- save_note
- list_notes
steps:
1. If `action` is `save`, call `run_tool` with `tool_name="save_note"` and pass `title` and `body`.
2. If `action` is `list`, call `run_tool` with `tool_name="list_notes"` and empty args.
3. Return the tool output exactly and include note ids when provided.
input_schema:
{
  "action": "save | list",
  "title": "string (required for save)",
  "body": "string (required for save)"
}
output_schema:
{
  "message": "string",
  "id": "number (for save)",
  "notes": "array (for list)"
}
VERIFICATION STRING: "VERIFIED_SKILL: notes:v1"
