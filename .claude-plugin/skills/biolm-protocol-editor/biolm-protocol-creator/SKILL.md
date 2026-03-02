---
name: biolm-protocol-creator
description: >
  Creates new BioLM Protocol YAML files and saves them to ~/Developer/protocols.
  Use this skill whenever the user wants to write, create, draft, or build a BioLM
  Protocol YAML — including requests like "make a protocol that folds sequences",
  "write a YAML for HyperMPNN generation", "create a BioLM pipeline for X", or
  "add a new protocol to the repo". Also trigger when the user describes a multi-step
  or batch inference workflow on BioLM.ai and asks for it to be codified.
  If the user mentions ~/Developer/protocols or the BioLM protocols repo, always use
  this skill. Prefer this skill over biolmai-sdk whenever the deliverable is a YAML file.
---

# BioLM Protocol Creator

You help users write new BioLM Protocol YAML files. These are declarative pipeline
definitions that chain BioLM model calls with batching, ranking, deduplication, and
user-controlled inputs — without writing Python. You save the finished file to the
user's local protocols repo at `~/Developer/protocols`.

**Read `references/schema.md` before writing any YAML.** It contains the full field
reference, all template expression patterns, and common mistakes to avoid.

19 real-world example protocols are in `references/examples/`. Always consult the
closest example when writing a new protocol.

**Critical:** Every Algorithm + Action combination on BioLM has its own unique
request keys, response fields, batch limits, and params. Never assume one
algorithm is representative of another. Always look up each Algorithm + Action
individually using the live API endpoints described in the **Discovering unknown
algorithms** section — even for algorithms that appear in the static catalog or
example protocols, since APIs can be updated.

---

## Workflow

Follow these steps for every new protocol request:

### 1. Clarify the request

Before writing anything, make sure you know:
- **Which model(s)?** If unclear, suggest candidates from the task description and ask.
  Check `references/algorithm_catalog.md` for known slugs and their available
  actions. If the user wants help choosing a model, consult the Quick Reference
  table first, then use the discovery methods in **Discovering unknown
  algorithms** if more detail is needed. For any model — known or new — check
  the Endpoint Detail table for item keys and response fields before writing
  the YAML, and use live discovery when the static data is insufficient.
- **Single item or batch?** Will the user submit one sequence/structure, or a list?
- **Key output fields?** What does the user want ranked or returned (e.g., pLDDT, perplexity, CIF)?
- **User-tunable params?** Are there knobs the user needs (temperature, num_sequences, pH)?

If the request is clear enough to proceed, do so — don't over-ask.

### 2. Pick the right pattern

| Use case | Pattern | Example file |
|---|---|---|
| Single input → single result | **Single-item** | `esmfold_predict_single.yaml` |
| List of sequences → results for each | **Gather + foreach** | `esm2_predict_from_sequences.yaml` |
| Generate N sequences from one prompt | **Subtasks fan-out** | `zymctrl_generate_single.yaml` |
| PDB input + generation | **PDB + subtasks** | `hypermpnn_generate_single.yaml` |
| Co-fold sequence + ligand | **Molecules list** | `boltz2_cofolding_pulldown.yaml` |
| Generate → score (multi-task) | **Chained pipeline** | `updated-hypermpnn-with-chain-c86619_v6.yaml` |

See `references/schema.md` §Patterns and `references/examples/` for concrete YAML.

### 3. Verify response field paths

Before writing `response_mapping`, confirm the actual response field names.
Use the following sources in order:

1. **Endpoint Detail table** — `references/algorithm_catalog.md` §Endpoint
   Detail lists response keys for 39 algorithms (63 endpoints). Start here.

2. **Live discovery** (Claude Code only) — see **Discovering unknown
   algorithms** below for `curl` commands to fetch the request schema and
   model docs with example responses.

3. **WebSearch fallback** (Cowork/Chat) — search for
   `"<model_name> BioLM API"` to find indexed model docs with response examples.

The response shape from predict/encode uses `response.results[*].field`.
For generation tasks use `response.results[0][*].field`. But **always verify**
— every Algorithm + Action has its own unique response structure.

### 4. Write the YAML

Follow the naming convention and directory structure below. Start from the closest
existing protocol as a template when one exists. Always include `example_inputs` with
realistic values — this is what the BioLM UI uses to pre-fill the form.

### 5. Validate and save

```bash
# Validate (requires biolmai installed and auth)
biolmai protocol validate <path-to-yaml>

# If biolmai isn't available, do a YAML syntax check
python3 -c "import yaml; yaml.safe_load(open('<path>').read()); print('YAML syntax OK')"
```

Save to `~/Developer/protocols/single_model/<filename>_v1.yaml` for single-model protocols.
Tell the user the full path and suggest `git add` + `git commit` since the repo tracks these.

---

## Naming convention

```
{primary-model-slug}-{action}[-qualifier]_v{N}.yaml
```

| Field | Description |
|---|---|
| `primary-model-slug` | The BioLM model slug (e.g., `esmfold`, `boltz2`, `hyper-mpnn`) |
| `action` | `predict`, `generate`, `encode`, `score`, `search` |
| `qualifier` | Optional: input type or output type for disambiguation (e.g., `sequence-cif`, `from-sequences`) |
| `_v{N}` | Version suffix, start at `v1` |

**Examples from the existing repo:**
- `esmfold-predict-sequence-pdb_v1.yaml`
- `boltz2-predict-sequence-cif_v1.yaml`
- `hypermpnn-generate_v1.yaml`
- `esm2stabp-predict-from-sequences_v1.yaml`

---

## Directory structure

```
~/Developer/protocols/
└── single_model/      ← All protocols that wrap a single BioLM model
    └── <name>_v{N}.yaml
```

Use `single_model/` for all protocols that call exactly one model slug. If you're
creating a multi-model chained protocol (e.g., generate → score), consider
creating a `multi_model/` directory and noting this to the user.

---

## Task routing

Every task that calls a BioLM model uses `slug` + `action`:

```yaml
- id: esmfold_predict
  slug: esmfold
  action: predict
```

The `slug` is the **model_slug** (also called **algorithm_slug**) — the same
identifier used in API URLs like `/api/v3/<slug>/<action>/` and in the schema
endpoint.

The `action` must be one of: `predict`, `encode`, `generate`, `score`, `search`.

**Always use `slug` + `action`.** Some older example files may use an
`app`/`class`/`method` triple — that is an internal representation and should
never appear in new protocols.

---

## Discovering unknown algorithms

**Every Algorithm + Action combination is unique.** Never assume one algorithm's
request keys, response fields, batch limits, or params apply to another — even
models in the same family (e.g., `esm2-8m` vs `esm2-650m` may differ). Always
look up each Algorithm + Action individually before writing a protocol.

The discovery strategy depends on your runtime environment and what's available.
Always start with the static catalog, then use live methods when needed.

### Step 0: Check the static catalog (always do this first)

`references/algorithm_catalog.md` has two tables:

- **Quick Reference** (76 algorithms, Feb 2026) — slug, name, available
  actions, tags. Use this to confirm a slug exists and what actions it supports.
- **Endpoint Detail** (39 algorithms / 63 endpoints, Sept 2025) — item keys,
  notable params, response keys, and response paths. Use this to build
  `request_body` and `response_mapping`.

If the model AND action appear in the Endpoint Detail table, you have enough
info to write the protocol. If the model is in Quick Reference but not in
Endpoint Detail, or if you need to verify that the static data is current,
proceed to live discovery.

### Step 1: Live discovery — Claude Code (full network access)

Claude Code runs on the user's machine and can make direct HTTP requests.
Use these when the static catalog is insufficient.

**Request schema** — authoritative source for item keys and params:
```bash
curl -s -H 'Content-Type: application/json' \
  https://biolm.ai/api/v3/schema/<slug>/<action>/
```

This returns the **canonical request body schema only** (not response schema).
It tells you exactly which keys go in `items` and which in `params`, plus
types, defaults, min/max values, and maxItems.

**Model docs via API** — includes example responses with all field names:
```bash
curl -s -H 'Content-Type: application/json' \
  https://biolm.ai/api/ui/community-api-models/<slug>/
```
The `docs_html` field contains working example requests AND response bodies.

**Full model list** — all available algorithms and capabilities:
```bash
curl -s -H 'Content-Type: application/json' \
  https://biolm.ai/api/ui/community-api-models/
```
Returns `model_slug`, `model_name`, `description`, `tags`, and boolean
capability flags (`predictor`, `encoder`, `generator`) for every algorithm.

**Model docs page** — `https://biolm.ai/models/<slug>/`
**Postman collection** — `https://api.biolm.ai/`

### Step 2: Live discovery — Cowork and Chat (web search only)

Cowork and Chat **cannot** make direct HTTP requests to biolm.ai. The
`curl` commands above will not work. Use **WebSearch** instead:

1. Search for `"<model_name> BioLM API"` or `"<slug> BioLM"`.
2. Search results often include useful snippet-level details about input
   parameters, output fields, and usage examples from indexed BioLM pages.
3. If WebSearch doesn't return enough detail for `response_mapping`, note
   the assumption in a YAML comment and suggest the user verify against the
   live schema API or model docs page.

### Interpreting the schema → building `request_body`

(Applies when you have access to the JSON schema from Step 1.)

The top-level JSON has `properties.items` and optionally `properties.params`.

- **`items`** → maps directly to `request_body.items` in the protocol YAML.
  Look at `$defs/<Model>RequestItem.properties` for the per-item field keys.
  - `required` fields MUST appear in `items` entries.
  - Fields with `default: null` or `anyOf: [{type: X}, {type: null}]` are optional —
    omit them from the YAML and the runtime removes the empty values.
  - `maxItems` on the `items` array tells you the **batch size limit** for the API.
    If `maxItems: 1`, use `into: 1` in a gather or a single-item pattern.

- **`params`** → maps to `request_body.params` in the protocol YAML.
  Look at `$defs/<Model>RequestParams.properties` for param keys.
  - Params with defaults can be omitted — only expose what the user should tune.
  - If `params` is listed in top-level `required`, include it even if empty: `params: {}`.
  - Enums in the schema → use `select` or `multiselect` input types with matching `choices`.

**Example: schema → YAML mapping**

Given a schema with:
```json
{
  "items": [{"$ref": "...RequestItem"}],  // RequestItem has: sequence (required, str, maxLength 2048)
  "params": {"$ref": "...RequestParams"}  // RequestParams has: temperature (number, default 0.1)
}
```

The protocol YAML `request_body` becomes:
```yaml
request_body:
  params:
    temperature: ${{ temperature }}
  items:
    - sequence: ${{ sequence }}
```

### Response path rules of thumb (verify per-algorithm)

- Predict/encode → `response.results[*].field`
- Generate → `response.results[0][*].field`

Cross-reference the Endpoint Detail table for the exact path of each algorithm.
- Nested objects → `response.results[*].parent.field`

---

## Input types reference

| `type` | Description | Key properties |
|---|---|---|
| `text` | Free-text string | `min_length`, `max_length` |
| `str` | Simple string (same as text) | `min_length`, `max_length` |
| `float` | Decimal number | `min`, `max`, `step` |
| `integer` | Whole number | `min`, `max`, `step` |
| `boolean` | True/false toggle | — |
| `select` | Single choice dropdown | `choices: [...]` |
| `multiselect` | Multiple choice | `choices: [...]`, `initial: []` |
| `list_of_str` | List of strings | `max_length` (max 100 000 chars total input) |
| `list_of_int` | List of integers | `max_length` |
| `list_of_float` | List of floats | `max_length` |
| `pdb_text` | PDB-format text | `max_length` (default/max 2 500 000) |
| `cif_text` | CIF-format text | `max_length` (default/max 2 500 000) |
| `file` | File upload | — |

**Important rules:**
- `initial` values are always strings even for floats/integers: `initial: '7.0'` not `initial: 7.0`
  (exception: booleans can be actual `true`/`false`)
- Use `required: true` OR `optional: true` (never both)
- `advanced: true` hides the input by default in the UI
- Every input needs `label` and `help_text`
- **Reserved input names — never use these:** `protocol_yaml`, `protocol_slug`,
  `protocol_def_id`, `app_username`, `dev`, `json`, `input_json`, `protocol_id`
- `name` at the top level (the protocol display name) must be ≤ 120 characters

---

## Key YAML skeleton

Every protocol follows this structure (all optional fields marked):

```yaml
name: Human-readable protocol name
description: One sentence describing what it does.

example_inputs:             # Required: realistic values for UI pre-fill
  sequence: MKFLKFSLLTAVLLSVVFAFSSCGDDDD

inputs:
  sequence:
    type: text
    label: Protein sequence
    required: true
    help_text: ...
    initial: ''
    min_length: 1
    max_length: 1024

concurrency:                # Optional: parallelism controls (requires BOTH fields)
  workflow: 1
  tasks: 8

progress:
  total_expected: 1         # Integer or ${{ expr }}

ranking:                    # All three fields required
  field: mean_plddt         # Must match a response_mapping key
  order: descending         # ascending or descending
  top_n: 10

writing:                    # Optional
  deduplicate: true
  max_dedupe_size: 500      # Optional: limit on dedup set size

tasks:
  - id: task_id             # Snake_case, unique, REQUIRED on every task
    slug: esmfold           # Model slug (see Task routing)
    action: predict
    request_body:
      items:
        - sequence: ${{ sequence }}
      params: {}            # Model-specific params
    response_mapping:       # Must be a dict with named keys (never a bare string)
      pdb: ${{ response.results[*].pdb }}
      mean_plddt: ${{ response.results[*].mean_plddt }}
    fail_on_error: true
```

---

## Validation constraints

These limits are enforced at upload time. Protocols that violate them will be rejected.

### Top-level field limits

| Field | Min | Max | Default | Notes |
|---|---|---|---|---|
| `ranking.top_n` | 1 | 10 | 10 | — |
| `concurrency.workflow` | 1 | 2 | 1 | Always use `1` for now |
| `concurrency.tasks` | 1 | 16 | 8 | Per-task semaphore; typically 12–16 |
| `writing.max_dedupe_size` | 0 | 1000 | 1000 | In MB; only meaningful with `deduplicate: true` |

### Allowed task actions

The `action` field must be one of:

`predict` · `encode` · `generate` · `score` · `search`

### Required fields per task

Every **non-gather** task must have:
1. `id` — unique snake_case identifier
2. `request_body.items` — the input items list
3. At least one key in `response_mapping`

Gather tasks (`type: gather`) require `id`, `type: gather`, `from`, and `fields`.
`into` is optional (defaults to batching all items into one chunk if omitted).

### Allowed top-level keys

These keys are accepted at the root of a protocol YAML:

`name` · `description` · `example_inputs` · `inputs` · `tasks` · `progress` · `ranking` · `concurrency` · `writing`

The JSON schema also accepts `schema_version` (use `1` for now) and
`protocol_version` (optional version string for tracking revisions). Both are
stripped before execution and have no runtime effect. An `about` metadata block
and `outputs` block also exist in the schema but are not currently used —
**do not include `outputs` in new protocols** (it is a placeholder for future
MLflow logging that is not yet implemented). Any key not listed above causes
a validation error.

### Allowed task-level keys

Each task object accepts only:

`id` · `type` · `depends_on` · `request_body` · `foreach` · `skip_if` · `fail_on_error` ·
`slug` · `action` · `response_mapping` ·
`from` · `field` · `fields` · `into` · `skip_if_empty` · `subtasks`

Unknown keys on a task cause a validation error.

### UI-only fields

`description` and `example_inputs` are stripped before the protocol runs.
They exist solely for the UI display and form pre-fill — include them for
usability but know they never reach the execution engine.

---

## YAML key semantics (runtime behavior)

These describe what each key actually does when the protocol executes.

### `fail_on_error` (task-level, boolean, default `true`)

Controls error propagation:

- **`true`** (default): If the task (or any of its subtasks) fails, the runner
  raises immediately. Downstream tasks are skipped. The workflow status becomes
  `"failure"`.
- **`false`**: The task's failure is recorded but execution continues. Failed
  items become **error placeholders** in the result array — downstream tasks
  receive them and can skip or handle them. Use for non-critical scoring or
  annotation tasks.

Each task can have its own setting. In a generate → score chain, set the
generation task to `true` (it's critical) and scoring tasks to `false`
(partial scoring is still useful).

### `skip_if` (task-level, template expression)

Evaluates a `${{ expr }}` before the task runs. If the expression is truthy,
the task is marked "skipped" and never executes. The expression has access to
all input parameters and all prior task results.

```yaml
skip_if: "${{ n_samples > 100 }}"          # Skip if too many samples
skip_if: "${{ gather_id.empty }}"           # Skip if gather produced nothing
```

### `skip_if_empty` (task-level, boolean)

When `true`, the task is automatically skipped if its source data is empty.
This applies to:
- **Gather tasks**: Skips if the source task/input has no items or the
  requested fields can't be extracted.
- **Foreach tasks**: Skips if the iterable is empty.

A skipped gather still emits internal finalization signals so downstream
tasks are properly notified.

### `writing.deduplicate` (top-level, boolean or template, default `true`)

When `true`, the writer tracks record hashes (JSON key-order-independent) and
suppresses exact duplicates. The hash window is bounded by `max_dedupe_size`.
Supports template expressions: `deduplicate: ${{ some_param }}`.

### `ranking` (top-level config)

Maintains a heap of the top N records by a designated field:

- `field` — must match a key produced by `response_mapping`
- `order` — `"ascending"` (lowest first) or `"descending"` (highest first, default)
- `top_n` — max records retained (1–10)

Records missing the ranking field are assigned the worst possible value
(−∞ for descending, +∞ for ascending) so they always rank last.

### `concurrency` (top-level config)

- `workflow` — number of parallel instances of this protocol (always `1` for now)
- `tasks` — per-task semaphore limit for concurrent subtask/foreach execution
  (default 8; typically set to 12–16 in production)

### `progress.total_expected` (top-level, integer or template)

Expected final record count. Used for progress percentage:
`progress = records_written / total_expected`. Supports templates like
`${{ n_samples }}` or `${{ number_of_batches * batch_size }}`.

### `foreach` (task-level, template expression)

Iterates the task over each element (or batch) from a prior result:

```yaml
foreach: "${{ gather_id.results }}"
```

Creates one subtask per item/batch. Inside the task's `request_body`, `${{ item }}`
refers to the current iteration element. Foreach tasks execute reactively —
as a gather emits batches, downstream foreach tasks start immediately rather
than waiting for the gather to fully complete.

### `subtasks` (task-level config)

Splits a single task into parallel subtasks with different parameters:

- `count` — number of subtasks (template supported: `${{ expr }}`)
- `split_params` — dict of parameter overrides per subtask (all values must be
  `${{ expr }}` template strings). Inside these expressions, `subtask.index`
  (0-based) and `subtasks.count` are available.

Results from all subtasks are aggregated (items arrays concatenated).
Each subtask maintains a correlation offset for record alignment.

### `depends_on` (task-level, list of task IDs)

Hard dependency: the task waits until all listed tasks complete before running.
If any dependency fails, the dependent task is skipped with reason
`"upstream_failed"`. Tasks are executed in topological order based on
these dependencies.

### `gather` task type

A gather task collects items from a source and batches them:

- `from` — source task ID **or** input parameter name (both are valid)
- `fields` — list of field names to extract from each item in the source
- `into` — batch size (integer or template); each batch becomes one item
  in `results`
- `skip_if_empty` — skip if source is empty

When gathering from an **input parameter** (a `list_of_str` input, not a
task), the gather wraps each string into a dict using the first field name.
E.g., `from: sequences` + `fields: [sequence]` transforms `["SEQ1", "SEQ2"]`
into `[{"sequence": "SEQ1"}, {"sequence": "SEQ2"}]`.

### `response_mapping` (task-level, dict)

Maps API response fields to named output keys. Must be a dict with named
keys (never a bare string). Values are `${{ response.path }}` expressions.
The `[*]` operator flattens arrays at each level.

---

## Patterns

### Pattern 1: Single-item predict/encode

The simplest pattern. One input → one API call → results.

```yaml
# See: esmfold_predict_single.yaml, soluprot_predict_single.yaml, diamond_predict_single.yaml
tasks:
  - id: esmfold_predict
    slug: esmfold
    action: predict
    request_body:
      items:
        - sequence: ${{ sequence }}
    response_mapping:
      pdb: ${{ response.results[*].pdb }}
      mean_plddt: ${{ response.results[*].mean_plddt }}
    fail_on_error: true
```

Note: Some example files use an older `app`/`class`/`method` convention —
always use `slug`/`action` as shown above for new protocols.

### Pattern 2: Gather + foreach batch predict

Takes a list of sequences, chunks them into batches, then processes each batch.

```yaml
# See: esm2_predict_from_sequences.yaml, esm2stabp_predict_from_sequences.yaml
tasks:
  - id: sequences_batch
    type: gather
    from: sequences           # Reference to a list_of_str input
    fields: [sequence]        # Wraps each string as {"sequence": "..."}
    into: ${{ batch_size }}   # Dynamic batch size from input (or literal: into: 1)
    skip_if_empty: true

  - id: esm2_predict
    slug: esm2-8m
    action: predict
    depends_on: [sequences_batch]
    foreach: ${{ sequences_batch.results }}
    skip_if_empty: true
    request_body:
      items: ${{ item }}      # The whole batch chunk as items list
    response_mapping:
      log_prob: ${{ response.results[*].log_prob }}
    fail_on_error: true
```

**`into: 1`** means per-item processing (batch size of 1). Use this when the model only
accepts one item at a time (e.g., Boltz2 structure prediction).

**`into: ${{ batch_size }}`** lets the user control batch size via an input parameter.

### Pattern 3: Gather + foreach with per-item field access

When gather chunks have named fields and you need to access them individually in the
request body (not pass the whole batch):

```yaml
# See: boltz2_predict_from_sequences.yaml, boltz2_cofolding_pulldown.yaml
tasks:
  - id: sequences_batch
    type: gather
    from: sequences
    fields: [sequence]
    into: 1
    skip_if_empty: true

  - id: boltz2_predict
    slug: boltz2
    action: predict
    depends_on: [sequences_batch]
    foreach: ${{ sequences_batch.results }}
    skip_if_empty: true
    request_body:
      params:
        recycling_steps: ${{ recycling_steps }}
      items:
        - molecules:
            - id: "A"
              type: ${{ molecule_type }}
              sequence: ${{ item.sequence }}   # Access named field from foreach item
    response_mapping:
      cif: ${{ response.results[*].cif }}
      confidence_score: ${{ response.results[*].confidence.confidence_score }}
    fail_on_error: true
```

### Pattern 4: Subtasks fan-out (generation)

Splits a large generation request across parallel subtasks.

```yaml
# See: zymctrl_generate_single.yaml, dsm_generate_single.yaml
tasks:
  - id: zymctrl_generate
    slug: zymctrl
    action: generate
    subtasks:
      count: ${{ (num_samples + 19) // 20 }}              # Ceiling division
      split_params:
        num_samples: ${{ min(20, max(0, num_samples - subtask.index * 20)) }}
    request_body:
      params:
        num_samples: ${{ num_samples }}
        max_length: ${{ max_length }}
        temperature: ${{ temperature }}
      items:
        - ec_number: ${{ ec_number }}
    response_mapping:
      sequence: ${{ response.results[0][*].sequence }}     # Generation: [0][*]
      perplexity: ${{ response.results[0][*].perplexity }}
    fail_on_error: true
```

**Empty `split_params: {}`** — creates N identical subtasks (one per sequence):
```yaml
# See: evo2_generate_single.yaml
subtasks:
  count: ${{ num_sequences }}
  split_params: {}
```

### Pattern 5: PDB + subtasks (structure-conditioned generation)

```yaml
# See: hypermpnn_generate_single.yaml
inputs:
  pdb_str:
    type: pdb_text
    label: Protein PDB
    required: true
    max_length: 2500000
tasks:
  - id: hypermpnn_generate
    slug: hyper-mpnn
    action: generate
    subtasks:
      count: ${{ max(1, (number_of_batches + 11) // 12) }}
      split_params:
        number_of_batches: ${{ min(12, number_of_batches) }}
    request_body:
      params:
        temperature: ${{ temperature }}
        batch_size: ${{ min(batch_size, 500) }}   # Cap in request body
      items:
        - pdb: ${{ pdb_str }}
    response_mapping:
      sequence: ${{ response.results[*].sequence }}
      overall_confidence: ${{ response.results[*].overall_confidence }}
    fail_on_error: true
```

### Pattern 6: Co-fold (molecules list with optional ligand)

```yaml
# See: boltz2_cofolding_pulldown.yaml
inputs:
  cofold_smiles:
    type: str
    label: Co-fold ligand (SMILES)
    optional: true
    initial: ""
  cofold_molecule_type:
    type: select
    label: Co-fold molecule type
    optional: true
    initial: ""
    choices: ["", "ligand"]

tasks:
  - id: boltz2_predict
    # ...
    request_body:
      items:
        - molecules:
            - id: "A"
              type: ${{ molecule_type }}
              sequence: ${{ item.sequence }}
            - id: ${{ cofold_chain_id }}
              type: ${{ cofold_molecule_type }}
              smiles: ${{ cofold_smiles }}
```

When `cofold_smiles` is empty, the second molecule entry with empty values is
**implicitly removed** from the request body by the runtime.

### Pattern 7: Multi-task pipeline (generate → gather → score)

Chain tasks where the output of one feeds the next. Gather can reference a **task ID**
(not just an input name) via `from:`.

```yaml
# See: updated-hypermpnn-with-chain-c86619_v6.yaml
tasks:
  - id: hyper_mpnn
    slug: hyper-mpnn
    action: generate
    # ... generates sequences
    response_mapping:
      sequence: ${{ response.results[*].sequence }}
      overall_confidence: ${{ response.results[*].overall_confidence }}
    fail_on_error: true

  - id: sequences_batches_esmc
    type: gather
    from: hyper_mpnn             # Gather from TASK OUTPUT, not input
    fields: [sequence]
    depends_on: [hyper_mpnn]
    into: ${{ esmc_batch_size }}
    skip_if_empty: true

  - id: esmc_scoring
    slug: esmc-300m
    action: score
    depends_on: [sequences_batches_esmc]
    foreach: ${{ sequences_batches_esmc.results }}
    skip_if_empty: true
    request_body:
      items: ${{ item }}
    response_mapping:
      esmc_300m_lp: ${{ response.results[*].log_prob }}
    fail_on_error: false         # Non-critical scoring: false
```

Key points for multi-task:
- `from: <task_id>` gathers from a previous task's output
- `depends_on` ensures execution order
- Use `fail_on_error: false` for non-critical annotation/scoring tasks

---

## Template expression quick reference

| Pattern | When to use |
|---|---|
| `${{ sequence }}` | Direct input value |
| `${{ response.results[*].field }}` | Extract field from predict/encode results |
| `${{ response.results[0][*].field }}` | Extract field from **generation** results |
| `${{ response.results[*].features.field }}` | Nested field extraction |
| `${{ response.results[*].confidence.field }}` | Nested confidence metrics (Boltz2) |
| `${{ response.results[0].sequences[*].field }}` | DIAMOND-style nested sequences |
| `${{ len(sequences) }}` | Count of a list_of_str input |
| `${{ max(1, (n + 11) // 12) }}` | Ceiling division (subtask count calc) |
| `${{ min(12, n) }}` | Cap a value |
| `${{ min(batch_size, 500) }}` | Cap in request body params |
| `${{ (num_samples + 19) // 20 }}` | Ceiling division shorthand |
| `${{ min(20, max(0, num_samples - subtask.index * 20)) }}` | Per-subtask remainder calc |
| `${{ item }}` | Whole batch chunk in foreach context |
| `${{ item.sequence }}` | Named field from foreach iteration |
| `${{ number_of_batches * batch_size }}` | Math expression in progress |

**Syntax rule:** Always use `${{ }}` (with dollar sign). Never `{{ }}`.

---

## Response mapping path patterns

| Model type | Path pattern | Example |
|---|---|---|
| Predict (flat) | `response.results[*].field` | `response.results[*].mean_plddt` |
| Predict (nested) | `response.results[*].parent.field` | `response.results[*].confidence.confidence_score` |
| Generate | `response.results[0][*].field` | `response.results[0][*].sequence` |
| DIAMOND | `response.results[0].sequences[*].field` | `response.results[0].sequences[*].score` |
| Peptides | `response.results[*].features.field` | `response.results[*].features.molecular_weight` |
| AlphaFold2 | `response.results[*].pdbs[0]` | Indexed sub-array |
| Chai1 | `response.results[0][0].cif` | Double-indexed |

---

## Existing protocols for reference

These are already in `~/Developer/protocols/single_model/`. Use them as starting
points — copy the closest one and modify rather than writing from scratch:

| File | Pattern | Notes |
|---|---|---|
| `esmfold-predict-sequence-pdb_v1.yaml` | Single-item predict | Simplest template |
| `boltz2-predict-sequence-cif_v1.yaml` | Single-item predict | Molecules format |
| `biolmsol-predict_v1.yaml` | Single-item predict | Multiple params (ph, boolean) |
| `esm2stabp-predict-from-sequences_v1.yaml` | Gather + foreach batch | Batch predict pattern |
| `evo2-predict-log-prob-from-sequences_v1.yaml` | Gather + foreach batch | Batch predict, genomics |
| `diamond-predict_v1.yaml` | Single-item predict | Lookup-style output |
| `zymctrl-generate_v1.yaml` | Subtasks generation | Generation + ranking |
| `progen2-oas-generate_v1.yaml` | Subtasks generation | Antibody generation |
| `hypermpnn-generate_v1.yaml` | PDB + subtasks | PDB input, structure-conditioned |
| `igg-antigen-structure-prediction.yaml` | Single-item predict | Multi-chain molecules format |
| `temberture-regression_v1.yaml` | Gather + foreach batch | Regression output |

**19 additional examples** are in `references/examples/` covering all patterns above.

---

## Additional references

- `references/schema.md` — Full protocol YAML authoring reference (595 lines)
- `references/algorithm_catalog.md` — Quick reference (76 algorithms, Feb 2026) + endpoint detail (39 algorithms, Sept 2025) + discovery instructions
- `references/examples/` — 19 real-world protocol YAML files
- `references/sdk-docs/SYNTAX_NOTE.md` — Conflicts between RST docs and real YAMLs
- `references/sdk-docs/protocol_schema.json` — Formal JSON Schema for validation
- `references/sdk-docs/` — Python SDK RST docs (about, inputs, tasks, execution, output, schema)

## Live API endpoints (always use `Content-Type: application/json`)

| Purpose | URL | Returns |
|---|---|---|
| List all available algorithms | `GET https://biolm.ai/api/ui/community-api-models/` | Slugs, descriptions, tags, available actions, Postman links |
| Algorithm detail + full docs HTML | `GET https://biolm.ai/api/ui/community-api-models/<slug>/` | `docs_html` with example requests/responses, pricing |
| Request body schema (Pydantic) | `GET https://biolm.ai/api/v3/schema/<slug>/<action>/` | Item keys, param keys, types, defaults, limits (request only) |
| Model docs page (human-readable) | `https://biolm.ai/models/<slug>/` | Full documentation, examples, use cases |

**Read `references/sdk-docs/SYNTAX_NOTE.md`** for quick syntax rules (e.g.,
`${{ }}` is required, no `inputs.` prefix, `response_mapping` must be a dict).
The real YAMLs in `~/Developer/protocols` and `references/examples/` are always
the ground truth for patterns and semantics.
