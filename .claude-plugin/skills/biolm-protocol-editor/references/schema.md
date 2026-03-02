# BioLM Protocol YAML — Authoring Reference

Protocols are YAML pipeline definitions that let you chain BioLM model calls, fan out over
large inputs, batch, deduplicate, and rank results — all without writing Python. They run on
the BioLM platform and are submitted as `.yaml` files.

---

## Top-Level Structure

```yaml
name: My Protocol               # required — identifier string
description: What this does     # recommended
schema_version: 1               # optional, default 1
protocol_version: "1.0"         # optional

about:                          # optional metadata block
  title: Full title
  description: Longer description
  authors:
    - name: Jane Smith
      affiliation: Acme Bio
      email: jane@acme.com
      orcid: 0000-0000-0000-0000
  keywords: [protein, folding]
  doi: 10.1234/example
  links:
    paper: https://example.com/paper

example_inputs:                 # optional — shown in UI as defaults
  sequence: MKFLKFSLLTAVLLSVVFAFSSCGDDDD
  temperature: 0.8

inputs:                         # required — defines the protocol's parameters
  sequence:
    type: text
    label: Protein sequence
    required: true
    min_length: 1
    max_length: 2048

progress:                       # optional — for UI progress bar
  total_expected: ${{ len(sequences) }}

ranking:                        # optional — real-time top-N result tracking
  field: score
  order: descending             # or ascending
  top_n: 10

writing:                        # optional — deduplication settings
  deduplicate: ${{ deduplication }}
  max_dedupe_size: 100000

concurrency:                    # optional — parallelism control
  workflow: 1                   # parallel workflow runs
  tasks: 8                      # parallel task slots

outputs:                        # optional — MLflow logging rules
  - where: ${{ some_field > 0.5 }}
    order_by:
      - field: score
        order: desc
    limit: 200
    run:
      name: ${{ run_name }}
    log:
      params:
        temperature: ${{ temperature }}
      metrics:
        mean_score: ${{ mean_score }}
      aggregates:
        - field: score
          ops: [count, mean, max, p90]
      artifacts:
        - type: fasta
          name: top_sequences

tasks:                          # required — list of ApiTask or GatherTask
  - id: my_task
    slug: esmfold
    action: predict
    request_body:
      items:
        - sequence: ${{ sequence }}
    response_mapping:
      pdb: ${{ response.results[*].pdb }}
      mean_plddt: ${{ response.results[*].mean_plddt }}
```

---

## Input Types (`InputSpec`)

| type | Description | Extra fields |
|---|---|---|
| `text` | Plain string | `min_length`, `max_length` |
| `float` | Floating point | `min`, `max`, `step` |
| `integer` | Integer | `min`, `max` |
| `boolean` | True/false toggle | — |
| `select` | Dropdown (one of) | `choices: [a, b, c]` |
| `multiselect` | Dropdown (many) | `choices: [a, b, c]` |
| `list_of_str` | List of strings (batch input) | `max_length` (total items) |
| `pdb_text` | PDB file content as raw text | `max_length` (chars) |

Common optional fields on any InputSpec:
- `label` — display name in UI
- `help_text` — tooltip/hint
- `required: true` / `optional: true`
- `initial` — default value (always a string even for numbers: `initial: '7.0'`)
- `advanced: true` — hides in UI by default

---

## Template Expressions — `${{ ... }}`

All values in a Protocol that can vary are written as `${{ expression }}`. Expressions are
evaluated as Python-like code with access to inputs, task results, and context variables.

### Accessing inputs

```yaml
${{ sequence }}           # direct reference to an input named "sequence"
${{ ph }}                 # float input
${{ deduplication }}      # boolean input
${{ len(sequences) }}     # length of a list_of_str input
```

### Arithmetic

```yaml
${{ number_of_batches * batch_size }}
${{ (num_samples + 19) // 20 }}      # ceiling division
${{ min(12, number_of_batches) }}
${{ max(1, (n + 11) // 12) }}
```

### Accessing response results

```yaml
# Single result, specific index
${{ response.results[0].field }}

# All results (wildcard)
${{ response.results[*].field }}

# Nested — generation models often return results[0][*]
${{ response.results[0][*].sequence }}
${{ response.results[0][*].ll_mean }}
```

### Foreach / gather context

```yaml
foreach: ${{ sequences_batch.results }}   # iterate over batches from gather task
items: ${{ item }}                         # current foreach item (the batch)
```

### Subtask context

```yaml
subtasks:
  count: ${{ (num_designs + 2) // 3 }}
  split_params:
    num_samples: ${{ min(3, max(0, num_designs - subtask.index * 3)) }}
    # subtask.index is 0-based: 0, 1, 2, ...
```

### Jinja-style pipe filters (for list transformation)

```yaml
# Transform list_of_str ["seq1", "seq2"] → [{"sequence": "seq1"}, {"sequence": "seq2"}]
items: ${{ sequences | map('dict', 'sequence') }}
```

---

## Task Types

### ApiTask — model call

```yaml
- id: my_task_id              # required — unique within the protocol
  slug: esmfold               # model slug (string or expression)
  action: predict             # predict | encode | generate | similarity | predict_log_prob | etc.

  # Task control
  depends_on:                 # list of task IDs this task waits for
    - gather_task_id
  foreach: ${{ gather_task.results }}   # iterate: one API call per item in the list
  skip_if: ${{ some_condition }}        # skip if expression is truthy
  skip_if_empty: true                   # skip if foreach source is empty

  fail_on_error: true         # default true; set false to continue despite errors

  # Fan-out: splits one task into N parallel subtasks
  subtasks:
    count: ${{ (n + 19) // 20 }}
    split_params:
      num_samples: ${{ min(20, num_samples - subtask.index * 20) }}

  request_body:               # required
    items:                    # list, object, or expression
      - sequence: ${{ sequence }}
    params:                   # optional — model-specific parameters
      temperature: ${{ temperature }}
      top_p: ${{ top_p }}

  response_mapping:           # optional — extract fields from the response for ranking/writing
    my_field: ${{ response.results[0].raw_field }}
    all_scores: ${{ response.results[*].score }}
```

**`slug` + `action`** is the standard combination. Alternatively you can use `class` + `app`
+ `method` for internal routing, but this is uncommon.

### GatherTask — batch a list input

Gather splits a `list_of_str` input into fixed-size chunks so the next task can process them
one batch at a time via `foreach`.

```yaml
- id: sequences_batch         # referenced by downstream tasks' depends_on + foreach
  type: gather                # required — marks this as a gather task
  from: sequences             # name of a list_of_str input (or another task ID)
  fields:
    - sequence                # field name to assign each element
  into: ${{ batch_size }}     # items per chunk (integer or expression)
  skip_if_empty: true
```

After this gather task, downstream tasks use:
```yaml
  depends_on: [sequences_batch]
  foreach: ${{ sequences_batch.results }}
  request_body:
    items: ${{ item }}        # "item" is the current chunk (list of dicts)
```

---

## Common Protocol Patterns

### Pattern 1: Single input → single result

```yaml
name: ESMFold Predict
inputs:
  sequence:
    type: text
    required: true
    min_length: 1
    max_length: 768
progress:
  total_expected: 1
ranking:
  field: mean_plddt
  order: descending
  top_n: 10
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

### Pattern 2: Batch list input via gather

Use when the user provides a `list_of_str`. The gather task chunks it; the model task
iterates over chunks via `foreach`.

```yaml
name: ESM2StabP Batch Predict
inputs:
  sequences:
    type: list_of_str
    required: true
  batch_size:
    type: integer
    optional: true
    initial: '8'
    min: 1
    max: 8
  deduplication:
    type: boolean
    optional: true
    initial: false
progress:
  total_expected: ${{ len(sequences) }}
writing:
  deduplicate: ${{ deduplication }}
ranking:
  field: melting_temperature
  order: descending
  top_n: 10
tasks:
  - id: sequences_batch
    type: gather
    from: sequences
    fields:
      - sequence
    into: ${{ batch_size }}
    skip_if_empty: true
  - id: esm2stabp_predict
    slug: esm2stabp
    action: predict
    depends_on:
      - sequences_batch
    foreach: ${{ sequences_batch.results }}
    skip_if_empty: true
    request_body:
      items: ${{ item }}
    response_mapping:
      melting_temperature: ${{ response.results[*].melting_temperature }}
      is_thermophilic: ${{ response.results[*].is_thermophilic }}
```

### Pattern 3: Fan-out generation via subtasks

Use when you want to generate many sequences in parallel. Subtasks split the total request
count across N parallel calls.

```yaml
name: ZymCTRL Generate
inputs:
  ec_number:
    type: text
    required: true
  num_samples:
    type: integer
    optional: true
    initial: '5'
    min: 1
    max: 50000
  ...
concurrency:
  workflow: 1
  tasks: 10
progress:
  total_expected: ${{ num_samples }}
ranking:
  field: perplexity
  order: ascending
  top_n: 10
tasks:
  - id: zymctrl_generate
    slug: zymctrl
    action: generate
    subtasks:
      count: ${{ (num_samples + 19) // 20 }}      # ceil(num_samples / 20)
      split_params:
        num_samples: ${{ min(20, max(0, num_samples - subtask.index * 20)) }}
    request_body:
      params:
        num_samples: ${{ num_samples }}
        temperature: ${{ temperature }}
      items:
        - ec_number: ${{ ec_number }}
    response_mapping:
      sequence: ${{ response.results[0][*].sequence }}
      perplexity: ${{ response.results[0][*].perplexity }}
```

### Pattern 4: PDB input

```yaml
inputs:
  pdb_str:
    type: pdb_text
    required: true
    max_length: 2500000
tasks:
  - id: mpnn_generate
    slug: hyper-mpnn
    action: generate
    request_body:
      items:
        - pdb: ${{ pdb_str }}
      params:
        temperature: ${{ temperature }}
```

### Pattern 5: Multi-field input item (antibody / complex)

```yaml
inputs:
  H:
    type: text
    required: true
    max_length: 210
  L:
    type: text
    required: true
    max_length: 210
  antigen_pdb:
    type: pdb_text
    optional: true
tasks:
  - id: immunefold_complex
    slug: immunefold-antibody
    action: predict
    request_body:
      items:
        - H: ${{ H }}
          L: ${{ L }}
          pdb: ${{ antigen_pdb }}
    response_mapping:
      pdb: ${{ response.results[*].pdb }}
      full_plddt: ${{ response.results[*].full_plddt }}
```

### Pattern 6: Minimal protocol (no optional sections)

The only required keys are `name`, `inputs`, and `tasks`:

```yaml
name: TemBERTure Regression
inputs:
  sequences:
    type: list_of_str
    required: true
tasks:
  - id: temberture_regression
    slug: temberture-regression
    action: predict
    request_body:
      items: ${{ sequences | map('dict', 'sequence') }}
    response_mapping:
      temperature: ${{ response.results[*].temperature }}
    fail_on_error: false
```

---

## `response_mapping` Patterns

`response_mapping` extracts fields from the API response and makes them available for
`ranking`, `writing`, `outputs`, and downstream task access.

```yaml
response_mapping:
  # Simple path extraction
  solubility_score: ${{ response.results[0].solubility_score }}

  # Wildcard — all results
  mean_plddt: ${{ response.results[*].mean_plddt }}

  # Nested array (generation)
  sequence: ${{ response.results[0][*].sequence }}

  # Object form — with explode and prefix
  confidence:
    path: ${{ response.results[*].confidence }}
    explode: true
    prefix: conf_
```

---

## `outputs` — MLflow Logging (NOT YET IMPLEMENTED)

**⚠ The `outputs` block is a placeholder for future MLflow logging and is NOT
currently implemented. Do not include `outputs` in new protocols.**

The schema below is reserved for future use:

```yaml
outputs:
  - where: ${{ score > 0.5 }}           # filter expression (optional)
    order_by:
      - field: score
        order: desc
    limit: 200                           # max rows to log
    run:
      name: ${{ run_name }}             # MLflow run name
    log:
      params:
        temperature: ${{ temperature }}
      metrics:
        mean_score: ${{ mean_score }}
      tags:
        protocol: my_protocol
      aggregates:
        - field: score
          ops: [count, mean, max, p50, p90, p95, p99, std]
      artifacts:
        - type: fasta        # seqparse | pdb | fasta | table | msa | plot | json | text
          name: top_hits
          content: ${{ result_fasta }}
```

---

## Running Protocols

### CLI

```bash
# Run a protocol YAML — pass inputs as key=value or JSON file
biolmai protocol run esmfold-predict_v1.yaml --inputs sequence=MKTAYIAKQR...
biolmai protocol run hypermpnn-generate_v1.yaml --inputs-file inputs.json

# List available protocols on the platform
biolmai protocol list

# Validate a protocol YAML without running it
biolmai protocol validate my_protocol.yaml
```

### Python SDK

```python
from biolmai.protocol import Protocol

# Load and run
proto = Protocol.from_file("esmfold-predict_v1.yaml")
results = proto.run(inputs={"sequence": "MKTAYIAKQR..."})

# Or run from dict
proto = Protocol.from_dict({
    "name": "Quick ESMFold",
    "inputs": {"sequence": {"type": "text", "required": True}},
    "tasks": [{"id": "t1", "slug": "esmfold", "action": "predict",
               "request_body": {"items": [{"sequence": "${{ sequence }}"}]}}]
})
results = proto.run(inputs={"sequence": "MKTAYIAKQR..."})
```

---

## Validation Checklist

Before submitting a protocol:

- [ ] `name` is set
- [ ] All `inputs` have `type` and either `required: true` or `optional: true`
- [ ] Every `${{ expr }}` refers to a defined input name, `response`, `item`, or `subtask`
- [ ] Every task has a unique `id`
- [ ] `GatherTask` (type: gather): has `from`, `fields`, is referenced by a downstream task's `depends_on` + `foreach`
- [ ] `ApiTask` with `subtasks`: `split_params` values use `subtask.index` correctly; ensure last subtask doesn't request 0 via `max(0, ...)`
- [ ] `response_mapping` paths match the actual API response structure for that model (verify via `biolmai.ai/api/ui/community-api-models/<slug>/`)
- [ ] `ranking.field` matches a key in `response_mapping`
- [ ] `progress.total_expected` is an integer or resolves to one
- [ ] `concurrency` has both `workflow` and `tasks` if used
- [ ] Optional inputs have `initial` values so the UI can prefill them

---

## Expression Quick Reference

| Pattern | Example |
|---|---|
| Input reference | `${{ sequence }}` |
| Input length | `${{ len(sequences) }}` |
| Arithmetic | `${{ (n + 19) // 20 }}` |
| Min/max | `${{ min(20, n) }}` |
| Response single | `${{ response.results[0].score }}` |
| Response all | `${{ response.results[*].score }}` |
| Generation nested | `${{ response.results[0][*].sequence }}` |
| Foreach current item | `${{ item }}` |
| Subtask index | `${{ subtask.index }}` |
| List → list of dicts | `${{ seqs \| map('dict', 'sequence') }}` |
| Conditional subtask count | `${{ max(0, n - subtask.index * 20) }}` |

---

## Schema Summary (required keys)

```
Protocol:
  name           (string)         required
  inputs         (map of InputSpec) required
  tasks          (list of Task)   required

InputSpec:
  type           (string)         required

ApiTask:
  id             (string)         required
  slug           (string)         required  ─┐ one of
  action         (string)         required   │ these
  request_body:                              ┘
    items        (array/object/expr) required
    params       (object)         optional

GatherTask:
  id             (string)         required
  type: gather                    required
  from           (string)         required
  fields         (list)           required
```

See `references/examples/` for 19 real protocol YAML files covering all patterns above.
