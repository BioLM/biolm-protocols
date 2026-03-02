# Syntax Note — RST Docs & JSON Schema

The `.rst` files in this directory come from the BioLM Python SDK documentation.
`protocol_schema.json` is the formal JSON Schema the `biolmai protocol validate`
command validates against — it is the highest-authority source for syntax rules.

The real YAMLs in `~/Developer/protocols` are the ground truth for **semantics and
patterns**. Prefer the real YAMLs when the schema is ambiguous about intent.

---

## Quick rules

- **Template expressions require `$`:** The ExprString pattern is `^\$\{\{[\s\S]+\}\}$`.
  Always write `${{ expr }}`, never `{{ expr }}`.

- **Input names are referenced directly:** Write `${{ sequence }}`, not
  `${{ inputs.sequence }}`. There is no `inputs.` namespace prefix.

- **`response_mapping` must be a dict with named keys** (never a bare string).
  The JSON schema defines `ResponseMapping` as `type: object`. Correct form:
  ```yaml
  response_mapping:
    pdb: ${{ response.results[*].pdb }}
  ```
  Note: `output.rst` still shows a bare-string form — ignore that pattern.

- **Every task must have `id`:** The JSON schema requires `id` on every task
  (it's in `TaskBase.required`). Note: the minimal example in `about.rst`
  omits `id` — don't follow that pattern.

---

## Key schema facts from `protocol_schema.json`

- **`ranking` requires all three fields**: `field`, `order`, and `top_n` — all required
  by the schema. Don't omit `top_n`.

- **`subtasks.split_params` values must be ExprStrings**: The schema defines
  `additionalProperties: { "$ref": "#/$defs/ExprString" }` — every split_params
  value must use `${{ }}` syntax, not a plain number or string.

- **`response_mapping` is on `TaskBase`**: This means gather tasks can also have
  `response_mapping` (not just model tasks).

- **`concurrency` requires both `workflow` and `tasks`**: Both are required fields
  if the `concurrency` block is present at all.

- **`gather.from` accepts a task `id` or an input name**: The schema says
  `"Source task ID or input name to gather from."` — both are valid.

---

## Useful additions from RST (not conflicts)

- **Gather from a task ID**: `from` can reference a previous task's `id`:
  ```yaml
  - type: gather
    from: predict          # task id whose response fields to gather
    fields: [pdb, mean_plddt]
  ```

- **`schema.rst`** references `../../schema/protocol_schema.json` via Sphinx
  `literalinclude` — the actual file is now included here as `protocol_schema.json`.
