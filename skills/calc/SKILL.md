skill_id: calc
description: Deterministic arithmetic evaluator for one expression.
triggers:
- "calculate"
- "evaluate expression"
- "math result"
required_tools:
- calculator
steps:
1. Read `expression` from the user request exactly as written.
2. Call `run_tool` with `tool_name="calculator"` and `args={"expression":"<expression>"}`.
3. Return the numeric result from the tool without changing the value.
input_schema:
{
  "expression": "string"
}
output_schema:
{
  "result": "number",
  "expression": "string"
}
VERIFICATION STRING: "VERIFIED_SKILL: calc:v1"
