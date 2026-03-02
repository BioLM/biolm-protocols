# BioLM Algorithm Catalog

Two sections below: a **quick-reference table** (Feb 2026 snapshot, 76 algorithms)
and an **endpoint-detail table** (Sept 2025 OpenAPI snapshot, 39 algorithms with
item keys, params, and response keys).

Both are static snapshots and will become outdated as BioLM adds new models and
endpoints. See the **Discovery** section at the bottom for how to get the latest
information at runtime.

---

## Quick reference (Feb 2026)

| Slug | Name | Actions | Tags |
|---|---|---|---|
| `ablang2` | AbLang-2 | predict, encode, generate | antibody, prediction, generation, embedding, mlm |
| `abodybuilder2` | ABodyBuilder2 | predict | antibody, structure, prediction, folding |
| `abodybuilder3-language` | ABodyBuilder3 Language | predict | antibody, structure, prediction, folding |
| `abodybuilder3-plddt` | ABodyBuilder3 pLDDT | predict | antibody, structure, prediction, folding |
| `alphafold2` | AlphaFold2 | predict, encode | protein, structure, folding, alphafold2, msa |
| `antifold` | AntiFold | predict, encode, generate | protein, antibody, structure, generation |
| `biolm-ablef` | BioLM-AbLEF | predict, encode | antibody, prediction, embedding |
| `biolmsol` | BioLMSol | predict | prediction |
| `biolmtox2` | BioLMTox-2 | predict, encode | prediction, embedding, biosecurity, toxins |
| `biotite` | Biotite PDB RMSD | predict | pdb |
| `boltz2` | Boltz-2 | predict | protein, structure, dna, prediction |
| `chai1` | Chai-1 | predict | protein, structure, dna, prediction |
| `clean` | CLEAN | predict, encode | protein, prediction, embedding |
| `deepviscosity` | DeepViscosity | predict | antibody, prediction |
| `diamond` | DIAMOND | predict | protein, prediction, blast, alignment |
| `dna-chisel` | DNA Chisel | predict | dna, prediction, analytics, design |
| `dnabert2` | DNABERT-2 | predict, encode | dna, bert, prediction, embedding |
| `dsm-150m-base` | DSM 150M Base | predict, encode, generate | protein, generation, embedding |
| `dsm-650m-base` | DSM 650M Base | predict, encode, generate | protein, generation, embedding |
| `dsm-650m-ppi` | DSM 650M PPI | predict, encode, generate | protein, generation, embedding |
| `e1-150m` | E1 150M | predict, encode | protein, prediction, embedding |
| `e1-300m` | E1 300M | predict, encode | protein, prediction, embedding |
| `e1-600m` | E1 600M | predict, encode | protein, prediction, embedding |
| `esm-if1` | ESM-IF1 | generate | structure, esm, generation, inverse-fold |
| `esm1b` | ESM1b | predict, encode | prediction, esm, embedding |
| `esm1v-all` | ESM-1v | predict | prediction, esm, mlm |
| `esm2-150m` | ESM-2 150M | predict, encode | protein, prediction, esm, embedding |
| `esm2-35m` | ESM-2 35M | predict, encode | protein, prediction, esm, embedding |
| `esm2-3b` | ESM-2 3B | predict, encode | protein, prediction, esm, embedding |
| `esm2-650m` | ESM-2 650M | predict, encode | protein, prediction, esm, embedding |
| `esm2-8m` | ESM-2 8M | predict, encode | protein, prediction, esm, embedding |
| `esm2stabp` | ESM2StabP | predict | prediction, esm |
| `esmc-300m` | ESM C 300M | predict, encode | protein, prediction, esm, embedding |
| `esmc-600m` | ESM C 600M | predict, encode | protein, prediction, esm, embedding |
| `esmfold` | ESMFold | predict | protein, structure, prediction, folding |
| `evo-15-8k-base` | Evo 1.5 8k Base | predict, generate | protein, dna, prediction, generation |
| `evo-v1.5-8k` | Evo v1.5-8k Base | predict, generate | dna, rna |
| `evo2-1b-base` | Evo 2 1B Base | predict, encode, generate | protein, dna, prediction, generation |
| `global-label-membrane-mpnn` | Global Label Membrane MPNN | generate | protein, structure, generation, mpnn |
| `hyper-mpnn` | HyperMPNN | generate | protein, structure, generation, mpnn |
| `igbert-paired` | IgBert Paired | predict, encode, generate | antibody, bert, prediction, generation |
| `igbert-unpaired` | IgBert Unpaired | predict, encode, generate | antibody, bert, prediction, generation |
| `igt5-paired` | IgT5 Paired | encode | antibody, t5, embedding |
| `igt5-unpaired` | IgT5 Unpaired | encode | antibody, t5, embedding |
| `immunefold-antibody` | ImmuneFold Antibody | predict | antibody, structure, prediction, folding |
| `immunefold-tcr` | ImmuneFold TCR | predict | structure, prediction, folding, tcr |
| `ligand-mpnn` | LigandMPNN | generate | protein, structure, generation, mpnn |
| `mpnn` | MPNN | generate | protein, design, mpnn |
| `msa-transformer` | MSA Transformer | encode | prediction, embedding |
| `nanobert` | nanoBERT | predict, encode, generate | antibody, bert, prediction, generation |
| `nanobodybuilder2` | NanoBodyBuilder2 | predict | antibody, structure, prediction, nanobody |
| `omni-dna-1b` | Omni-DNA 1B | predict, encode | dna, prediction, embedding |
| `peptides` | Peptides | encode | protein, antibody, feature-extraction, peptide |
| `per-residue-label-membrane-mpnn` | Per-Residue Label Membrane MPNN | generate | protein, structure, generation, mpnn |
| `pro4s-classification` | Pro4S Classification | predict | structure, prediction |
| `pro4s-regression` | Pro4S Regression | predict | structure, prediction |
| `progen2-bfd90` | ProGen2 BFD90 | generate | generation |
| `progen2-large` | ProGen2 Large | generate | generation |
| `progen2-medium` | ProGen2 Medium | generate | generation |
| `progen2-oas` | ProGen2 OAS | generate | generation |
| `prostt5-aa2fold` | ProstT5 AA2Fold | encode, generate | protein, structure, folding, t5 |
| `prostt5-fold2aa` | ProstT5 Fold2AA | encode, generate | protein, structure, inverse-fold, t5 |
| `protein-mpnn` | ProteinMPNN | generate | protein, structure, generation, mpnn |
| `sadie-antibody` | Sadie Antibody | predict | antibody, prediction, feature-extraction |
| `soluble-mpnn` | Soluble ProteinMPNN | generate | protein, structure, generation, mpnn |
| `soluprot` | SoluProt | predict | protein, prediction |
| `spurs` | SPURS | predict | protein, structure, prediction, ddg |
| `tcrbuilder2` | TCRBuilder2 | predict | structure, prediction, folding, tcr |
| `tcrbuilder2-plus` | TCRBuilder2+ | predict | structure, prediction, folding, tcr |
| `temberture-classifier` | TemBERTure Classifier | predict, encode | prediction, embedding |
| `temberture-regression` | TemBERTure Regression | predict, encode | prediction, embedding |
| `tempro-3b` | TEMPRO 3B | predict | protein, prediction |
| `tempro-650m` | TEMPRO 650M | predict | protein, prediction |
| `thermompnn` | ThermoMPNN | predict | protein, structure, prediction, ddg |
| `thermompnn-d` | ThermoMPNN-D | predict | protein, structure, prediction, ddg |
| `zymctrl` | ZymCTRL | encode, generate | generation, embedding |

**76 algorithms.** The `score` and `search` actions are also valid Protocol
actions but are not reflected in the capability flags above. Check the live
schema API to discover if a given model supports these actions.

---

## Endpoint detail (Sept 2025 OpenAPI snapshot)

This table provides item keys, notable parameters, response keys, and response
paths for 39 algorithms (63 endpoints) as captured from the OpenAPI specs.
Models not listed here either did not exist at the time of the snapshot or did
not have detailed specs available — use the Discovery methods below.

| Slug | Action | Item keys | Notable params | Response keys | Response path |
|---|---|---|---|---|---|
| `ablang2` | `encode` | heavy, light | include, align | seqcoding | `results[*]` |
| `ablang2` | `predict` | heavy, light | include | likelihood, sequence_tokens, vocab_tokens | `results[*]` |
| `abodybuilder2` | `predict` | — | — | pdb | `results[*]` |
| `abodybuilder3-language` | `predict` | H, L | plddt, seed | pdb | `results[*]` |
| `abodybuilder3-plddt` | `predict` | H, L | plddt, seed | pdb, plddt | `results[*]` |
| `alphafold2` | `encode` | sequence | databases, return_templates, msa_iterations, max_msa_sequences (+1) | alignments, templates | `results[*]` |
| `alphafold2` | `predict` | sequence | databases, predictions_per_model, relax, return_templates (+3) | pdbs | `results[*]` |
| `antifold` | `encode` | pdb | heavy_chain, light_chain | embeddings | `results[*]` |
| `antifold` | `generate` | pdb | heavy_chain, light_chain, regions | sequences | `results[*]` |
| `antifold` | `predict` | pdb | heavy_chain, light_chain | global_score, heavy, light | `results[*]` |
| `biolmtox2` | `encode` | sequence | — | mean_representation | `results[*]` |
| `biolmtox2` | `predict` | sequence | — | label, score | `results[*]` |
| `chai1` | `predict` | — | — | cif | `results[0][*]` |
| `dna-chisel` | `predict` | sequence | — | gc_content, cai, hairpin_score, melting_temperature (+14) | `results[*]` |
| `dnabert2` | `encode` | sequence | — | embedding | `results[*]` |
| `dnabert2` | `predict` | sequence | — | log_prob | `results[*]` |
| `esm-if1` | `generate` | pdb | chain, num_samples, temperature, multichain_backbone | sequence, recovery | `results[0][*]` |
| `esm1v-all` | `predict` | sequence | — | esm1v-n1, esm1v-n2, esm1v-n4, esm1v-n5 (+1) | `results[*]` |
| `esm2-150m` | `encode` | sequence | repr_layers, include | sequence_index, embeddings, contacts | `results[*]` |
| `esm2-150m` | `predict` | sequence | — | logits, sequence_tokens, vocab_tokens | `results[*]` |
| `esm2-35m` | `encode` | sequence | — | sequence_index, embeddings | `results[*]` |
| `esm2-35m` | `predict` | sequence | — | logits, sequence_tokens, vocab_tokens | `results[*]` |
| `esm2-650m` | `encode` | sequence | repr_layers, include | sequence_index, embeddings, per_token_embeddings | `results[*]` |
| `esm2-650m` | `predict` | sequence | — | logits, sequence_tokens, vocab_tokens | `results[*]` |
| `esm2-8m` | `encode` | sequence | repr_layers, include | sequence_index, embeddings, per_token_embeddings | `results[*]` |
| `esm2-8m` | `predict` | sequence | — | logits, sequence_tokens, vocab_tokens | `results[*]` |
| `esmc-300m` | `encode` | sequence | repr_layers, include | embeddings, per_token_embeddings | `results[*]` |
| `esmc-300m` | `predict` | sequence | — | logits, sequence_tokens, vocab_tokens | `results[*]` |
| `esmc-600m` | `encode` | sequence | — | embeddings | `results[*]` |
| `esmc-600m` | `predict` | sequence | — | logits, sequence_tokens, vocab_tokens | `results[*]` |
| `esmfold` | `predict` | sequence | — | pdb, mean_plddt, ptm | `results[*]` |
| `evo-15-8k-base` | `generate` | prompt | max_new_tokens, temperature, top_k, top_p | generated, score | `results[*]` |
| `evo-15-8k-base` | `predict` | sequence | — | log_prob | `results[*]` |
| `evo2-1b-base` | `encode` | sequence | — | embeddings | `results[*]` |
| `evo2-1b-base` | `generate` | prompt | — | generated | `results[*]` |
| `evo2-1b-base` | `predict` | sequence | — | log_prob | `results[*]` |
| `igbert-paired` | `encode` | heavy, light | — | embeddings | `results[*]` |
| `igbert-paired` | `generate` | sequence | — | sequence | `results[*]` |
| `igbert-paired` | `predict` | heavy, light | — | log_prob | `results[*]` |
| `igbert-unpaired` | `encode` | sequence | include | embeddings, residue_embeddings | `results[*]` |
| `igbert-unpaired` | `generate` | sequence | — | sequence | `results[*]` |
| `igbert-unpaired` | `predict` | sequence | — | log_prob | `results[*]` |
| `igt5-paired` | `encode` | heavy, light | include | embeddings, residue_embeddings | `results[*]` |
| `igt5-unpaired` | `encode` | sequence | include | embeddings, residue_embeddings | `results[*]` |
| `immunefold-antibody` | `predict` | H, pdb | contact_idx | — | `results[*]` |
| `immunefold-tcr` | `predict` | — | — | ptm, full_plddt, plddt, pdb | `results[*]` |
| `nanobert` | `encode` | sequence | — | embeddings | `results[*]` |
| `nanobert` | `generate` | sequence | — | sequence | `results[*]` |
| `nanobert` | `predict` | sequence | — | log_prob | `results[*]` |
| `nanobodybuilder2` | `predict` | H | — | pdb | `results[*]` |
| `omni-dna-1b` | `encode` | sequence | — | mean | `results[*]` |
| `omni-dna-1b` | `predict` | sequence | — | log_prob | `results[*]` |
| `peptides` | `encode` | sequence | include | features | `results[*]` |
| `progen2-bfd90` | `generate` | context | temperature, top_p, num_samples, max_length | sequence, ll_sum, ll_mean | `results[0][*]` |
| `progen2-large` | `generate` | context | temperature, top_p, num_samples, max_length | sequence, ll_sum, ll_mean | `results[0][*]` |
| `progen2-medium` | `generate` | context | temperature, top_p, num_samples, max_length | sequence, ll_sum, ll_mean | `results[0][*]` |
| `progen2-oas` | `generate` | context | temperature, top_p, num_samples, max_length | sequence, ll_sum, ll_mean | `results[0][*]` |
| `prostt5-aa2fold` | `encode` | — | — | mean_representation | `results[*]` |
| `prostt5-aa2fold` | `generate` | — | — | sequence | `results[0][*]` |
| `prostt5-fold2aa` | `encode` | sequence | direction | mean_representation | `results[*]` |
| `prostt5-fold2aa` | `generate` | sequence | direction, temperature, top_p, top_k (+2) | sequence | `results[0][*]` |
| `sadie-antibody` | `predict` | sequence | region_assign, scheme, scfv, allowed_chain | domain_no, hmm_species, chain_type, e_value (+27) | `results[*]` |
| `tcrbuilder2-plus` | `predict` | — | — | pdb | `results[*]` |

**63 endpoints across 39 algorithms.** Models not in this table (37 newer
algorithms) do not yet have detailed endpoint specs in this snapshot.

---

## Discovery — getting the latest information

Both catalog tables above are static snapshots. New algorithms and endpoint
changes are deployed regularly. Use these strategies to get current information,
choosing the method that matches your runtime environment:

### In Claude Code (full network access)

Claude Code runs on the user's machine and can make HTTP requests directly.

**Schema API** — authoritative source for a single endpoint's request/response
shape:

```bash
# Get the JSON Schema for a specific slug + action
curl -s -H 'Content-Type: application/json' \
  https://biolm.ai/api/v3/schema/<slug>/<action>/
```

Example: `curl -s -H 'Content-Type: application/json' https://biolm.ai/api/v3/schema/esm2-650m/predict/`

This returns the full request body JSON Schema including required item keys,
optional parameters, and response structure.

**Community models list** — authoritative list of all currently available
algorithms and their capabilities:

```bash
curl -s -H 'Content-Type: application/json' \
  https://biolm.ai/api/ui/community-api-models/
```

Returns a JSON array with `model_slug`, `model_name`, and boolean capability
flags (`predictor`, `encoder`, `generator`, etc.) for every algorithm.

**Model docs pages** — human-readable documentation with examples:

```
https://biolm.ai/models/<slug>/
```

Example: `https://biolm.ai/models/esm2-650m/`

**Postman collection** — browsable API documentation with request/response
examples:

```
https://api.biolm.ai/
```

Individual endpoint anchors follow the pattern `https://api.biolm.ai/#<uuid>`.

### In Cowork or Chat (no direct HTTP, but web search available)

Cowork and Chat cannot `curl` or `WebFetch` biolm.ai domains directly.
Use **WebSearch** to find information:

1. Search for `"<model_name> BioLM API"` or `"<slug> BioLM"` to find the
   model docs page and indexed content.
2. Search results often include useful snippet-level details about input
   parameters, output fields, and usage examples from indexed BioLM pages.
3. If WebSearch does not return enough detail, note in the Protocol YAML
   comments what information could not be verified and suggest the user check
   the live docs.

**Fallback order:**

1. Check the Endpoint Detail table above.
2. If not found there, check the Quick Reference table for available actions.
3. Use WebSearch with `"<model_name> BioLM"` to find current details.
4. If still unclear, add a comment in the generated YAML noting the
   assumption and suggesting verification against the live schema API.
