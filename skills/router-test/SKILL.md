skill_id: router-test
description: Router verification skill that requires two tool calls in sequence.
triggers:
- "router test"
- "tool routing test"
- "verify tool calls"
required_tools:
- now
- calculator
steps:
1. Read optional `tz` and optional `expression`; default to `UTC` and `1+1` when missing.
2. Call `run_tool` with `tool_name="now"` first.
3. Call `run_tool` with `tool_name="calculator"` second.
4. Return both outputs in one response so call ordering is visible.
input_schema:
{
  "tz": "string (optional, IANA timezone)",
  "expression": "string (optional)"
}
output_schema:
{
  "iso": "string",
  "tz": "string",
  "value": "number",
  "expression": "string"
}
VERIFICATION STRING: "VERIFIED_SKILL: router-test:v1"
