skill_id: hello-skill
description: Deterministic greeting flow that returns hello plus current time in a timezone.
triggers:
- "say hello"
- "greet me"
- "hello with time"
required_tools:
- now
steps:
1. Read optional `name` and optional `tz`; default `tz` to `UTC` when missing.
2. Call `run_tool` with `tool_name="now"` and `args={"tz":"<tz>"}`.
3. Return `Hello, <name or user>` and include the timestamp from the tool.
input_schema:
{
  "name": "string (optional)",
  "tz": "string (optional, IANA timezone)"
}
output_schema:
{
  "message": "string",
  "iso": "string",
  "tz": "string"
}
VERIFICATION STRING: "VERIFIED_SKILL: hello-skill:v1"
