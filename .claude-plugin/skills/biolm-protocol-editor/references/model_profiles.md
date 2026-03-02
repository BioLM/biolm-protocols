# BioLM Model Profiles

Condensed model profiles extracted from BioLM documentation (.rst sources).
Use this reference for **model selection** (choosing the right algorithm for
a task) and **response mapping verification** (confirming exact field names
and nesting structure).

This is a static snapshot. For the latest information, use the discovery
methods in `algorithm_catalog.md`.

---

## Domain Index

Quick lookup — which models are suited for which domains:

- **DNA**: `dna-chisel`, `dna_chisel`, `dnabert`, `dnabert2`, `evo-15-8k-base`, `evo2`, `evo2-1b-base`, `omni-dna-1b`
- **RNA**: `boltz`, `dnabert2`, `evo-15-8k-base`, `evo2`
- **TCR**: `abodybuilder2`, `immunefold-tcr`, `tcrbuilder2`, `tcrbuilder2-plus`
- **antibody**: `ablang2`, `abodybuilder2`, `abodybuilder3-language`, `abodybuilder3-plddt`, `antifold`, `biolm-ablef`, `biolmsol`, `deepviscosity`, `igbert-paired`, `igbert-unpaired`, `igt5-paired`, `igt5-unpaired`, `immunefold-antibody`, `progen2-oas`, `propermab`, `sadie-antibody`
- **enzyme**: `clean`, `e1-150m`, `e1-300m`, `e1-600m`, `zymctrl`
- **general-protein**: `dsm-150m-base`, `dsm-650m-base`, `dsm-650m-ppi`, `e1-150m`, `esm1b`, `esm1v`, `esm1v-all`, `esm2-150m`, `esm2-35m`, `esm2-3b`, `esm2-650m`, `esm2-8m`, `esm3-open-small`, `esmc-300m`, `esmc-600m`, `esmfold`, `example`, `igbert-unpaired`, `msa-transformer`, `progen2`, `progen2-bfd90`, `progen2-large`, `progen2-medium`, `progen2-oas`, `prostt5`, `protgpt2`
- **inverse-folding**: `antifold`, `esm-if1`, `global-label-membrane-mpnn`, `ligand-mpnn`, `per-residue-label-membrane-mpnn`, `prostt5-fold2aa`, `protein-mpnn`, `soluble-mpnn`
- **ligand**: `boltz`, `boltz2`, `chai1`, `ligand-mpnn`
- **membrane**: `global-label-membrane-mpnn`, `per-residue-label-membrane-mpnn`
- **nanobody**: `abodybuilder2`, `immunefold-antibody`, `nanobert`, `nanobodybuilder2`, `tempro-3b`, `tempro-650m`
- **peptide**: `biolmtox2`, `peptides`
- **sequence-alignment**: `diamond`
- **solubility**: `biolmsol`, `pro4s-classification`, `pro4s-regression`, `soluble-mpnn`, `soluprot`
- **stability**: `dsm-150m-base`, `dsm-650m-base`, `e1-150m`, `e1-300m`, `e1-600m`, `esm2stabp`, `hyper-mpnn`, `pro4s-classification`, `pro4s-regression`, `spurs`, `tempro-3b`, `tempro-650m`, `thermompnn`, `thermompnn-d`
- **structural-comparison**: `abodybuilder2`, `biotite`, `tcrbuilder2-plus`
- **structure-prediction**: `abodybuilder2`, `abodybuilder3-plddt`, `af2_nim`, `alphafold2`, `boltz`, `boltz2`, `chai1`, `dsm-650m-ppi`, `e1-300m`, `esm3-open-small`, `esmfold`, `immunefold-antibody`, `immunefold-tcr`, `nanobodybuilder2`, `prostt5`, `prostt5-aa2fold`, `tcrbuilder2`, `tcrbuilder2-plus`
- **thermostability**: `biolm-ablef`, `esm2stabp`, `hyper-mpnn`, `spurs`, `temberture-classifier`, `temberture-regression`, `tempro-3b`, `tempro-650m`, `thermompnn`, `thermompnn-d`
- **toxin**: `biolmtox`, `biolmtox2`
- **viscosity**: `biolm-ablef`, `deepviscosity`, `pro4s-regression`, `propermab`

---

## Model Profiles

### `ablang2` — AbLang-2 API

**Description:** AbLang-2 is an antibody-specific language model optimized to reduce germline bias and better model non-germline residues relevant for binding and developability. Trained on 35.6M unpaired and 1.26M paired VH/VL sequences from OAS using a modified ...

**Domain:** antibody

**Actions:** predict, encode

**Output formats:** CIF, embedding, log-probability, pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `seqcoding`
  - **predict**: `likelihood, sequence_tokens, vocab_tokens`

**Best for:**
- Antibody sequence optimization to prioritize non‑germline mutations that are frequently observed in affinity‑matured ...
- In silico ranking of mutated antibody panels by comparing per‑residue likelihood profiles across VH/VL variants that ...
- Computational assessment of somatic hypermutation patterns within clonotypes by embedding paired VH/VL sequences with...

**Related/alternatives:**
- `IgT5 Paired` — ``IgT5 Paired`` – Sequence-to-sequence modeling for paired heavy/light chains
- `IgBert Paired` — ``IgBert Paired`` – Produces transformer embeddings for paired chains
- `ABodyBuilder3 pLDDT` — ``ABodyBuilder3 pLDDT`` – Antibody structure prediction with per-residue confidence
- `ImmuneFold Antibody` — ``ImmuneFold Antibody`` – Antibody structure prediction from sequence

---

### `abodybuilder2` — ABodyBuilder2 API

**Description:** ABodyBuilder2 is an antibody-specific deep learning model for rapid, high-accuracy prediction of Fv structures, including all six CDR loops, from paired heavy (H) and light (L) chain sequences. It achieves a mean CDR-H3 backbone RMSD of 2.81 Å on ...

**Domain:** antibody, nanobody, TCR, structure-prediction, structural-comparison

**Actions:** predict

**Output formats:** CIF, PDB, RMSD (float), embedding, pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `pdb, plddt`

**Best for:**
- Accurate prediction of antibody variable region (VH/VL) structures for therapeutic design, providing all-atom Fv mode...
- Rapid structural modeling of large antibody panels (up to 8 VH/VL pairs per API call, each chain ≤2048 residues), ena...
- Nanobody and TCR structure prediction via the broader ImmuneBuilder API, allowing teams to use the same interface for...

**Related/alternatives:**
- `NanoBodyBuilder2` — ``NanoBodyBuilder2`` – Nanobody-specific ImmuneBuilder model using the same architecture as ABody...
- `TCRBuilder2` — ``TCRBuilder2`` – ImmuneBuilder model for paired TCR α/β chains, complementary when comparing ant...
- `ImmuneFold Antibody` — ``ImmuneFold Antibody`` – Alternative antibody structure prediction method for cross-checking ABo...
- `ABodyBuilder3 pLDDT` — ``ABodyBuilder3 pLDDT`` – Next-generation antibody model providing per-residue confidence scores,...

---

### `abodybuilder3-language` — ABodyBuilder3 Language API

**Description:** ABodyBuilder3 is an antibody-specific language model designed to accurately restore missing residues in antibody sequences without requiring germline information. The model, trained on the Observed Antibody Space (OAS) database, leverages transfor...

**Domain:** antibody

**Actions:** predict

**Output formats:** CIF, PDB, embedding, log-probability, pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `pdb, plddt`

**Best for:**
- Restoring incomplete antibody sequences from high-throughput sequencing data by accurately predicting missing residue...
- Enhancing antibody maturation and optimization efforts by generating residue-level predictions for potential mutation...
- Filtering and ranking antibody candidates by generating sequence-level embeddings (seq-codings) that capture germline...

**Related/alternatives:**
- `AbLang-2` — ``AbLang-2`` – An updated version of AbLang, this model further enhances antibody sequence comple...
- `ESM-1v` — ``ESM-1v`` – A general protein language model that, while not antibody-specific, can complement A...
- `ImmuneFold Antibody` — ``ImmuneFold Antibody`` – Focused on antibody structure prediction, this algorithm can be used al...
- `IgT5 Paired` — ``IgT5 Paired`` – Specializes in paired antibody sequence analysis, offering a complementary appr...

---

### `abodybuilder3-plddt` — ABodyBuilder3 pLDDT API

**Description:** ABodyBuilder3 pLDDT is a GPU-accelerated antibody structure prediction model that infers atomic-level 3D structures from heavy and light chain amino acid sequences, specifically trained for accurate modeling of antibody variable regions and CDR lo...

**Domain:** antibody, structure-prediction

**Actions:** predict

**Output formats:** CIF, PDB, RMSD (float), embedding, pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `pdb, plddt`

**Best for:**
- Rapid antibody candidate screening and prioritization by predicting structural confidence (pLDDT), enabling biotech c...
- Antibody affinity maturation workflows, where structural confidence scores (pLDDT) help protein engineers identify st...
- Computational epitope mapping and characterization by assessing antibody structural reliability (pLDDT), allowing res...

**Related/alternatives:**
- `NanoBodyBuilder2` — ``NanoBodyBuilder2`` – Predicts nanobody structures with similar accuracy and speed, complementin...
- `TCRBuilder2` — ``TCRBuilder2`` – Provides accurate structure predictions for T-cell receptors, complementing ant...
- `ImmuneFold Antibody` — ``ImmuneFold Antibody`` – Offers alternative antibody structure predictions, useful for cross-val...
- `ABodyBuilder3 Language` — ``ABodyBuilder3 Language`` – Generates antibody structures using language-model-based approaches,...

---

### `af2_nim` — Af2 Nim API

**Description:** AlphaFold-2 NIM is a 3-GPU Alphafold2 variant that predicts protein structures and provides multiple-sequence alignments (MSAs).

**Domain:** structure-prediction

**Actions:** encode, predict

**Output formats:** PDB

**Best for:**
- High-accuracy monomer structure prediction
- Template-free modelling for novel proteins
- Downstream feature extraction for ML

---

### `alphafold2` — AlphaFold2 API

**Description:** AlphaFold2 is a GPU-accelerated protein structure prediction model that infers atomic 3D coordinates from amino acid sequences using MSAs and structural templates, with typical backbone accuracies near 1 Å r.m.s.d.\ :sub:`95` on CASP14 targets. Th...

**Domain:** structure-prediction

**Actions:** predict, encode

**Output formats:** CIF, PDB, pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `alignments, small_bfd, mgnify, uniref90, templates[*].{index, name, aligned_cols, sum_probs, query, hit_sequence, indices_query, indices_hit}`
  - **predict**: `pdbs`

**Best for:**
- Rapid prediction of monomeric protein 3D structures directly from amino acid sequences, enabling faster iteration in ...
- High-resolution backbone and side-chain modeling to inform rational mutagenesis, supporting stability engineering, ac...
- Structural characterization of novel or poorly annotated protein domains, including sequences without close structura...

**Related/alternatives:**
- `ESMFold` — ``ESMFold`` – Fast single-sequence protein structure prediction
- `ABodyBuilder3 pLDDT` — ``ABodyBuilder3 pLDDT`` – Antibody-focused structure prediction with per-residue confidence
- `ImmuneFold Antibody` — ``ImmuneFold Antibody`` – Specialized antibody and immune receptor modeling
- `ProstT5 AA2Fold` — ``ProstT5 AA2Fold`` – Sequence-to-structure generation leveraging protein language modeling

---

### `antifold` — AntiFold API

**Description:** AntiFold is an antibody-specific inverse folding model fine-tuned from ESM-IF1 for structure-constrained design of variable domains, including nanobodies. It accepts IMGT-numbered antibody or antibody–antigen PDBs and returns per-residue mutation ...

**Domain:** antibody, inverse-folding

**Actions:** predict, encode, generate

**Output formats:** sequence (heavy/light chains), embedding, scores (float)

**Input:** PDB structure + chain ID params (`heavy_chain`, `light_chain`, or `nanobody_chain`)

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `embeddings` (optional: `residue_embeddings`, `logits`, `perplexity`, `vocab` etc.)
  - **generate**: `sequences[*].{global_score, score, heavy, light, temperature, mutations, seq_recovery}` (optional: `logprobs`, `logits`, `perplexity`, `vocab` etc.)
  - **predict**: `global_score, heavy, light`

**IMPORTANT for generate:** AntiFold generate returns sequences nested at `results[*].sequences[*]`, NOT `results[0][*]`. Each sequence object has separate `heavy` and `light` fields (not a single `sequence` field).

**Best for:**
- Antibody affinity maturation by generating backbone-constrained CDR variants from input VH/VL structures, helping the...
- Optimization of antibody humanization workflows by scoring humanized VH/VL variants for structural compatibility with...
- Antibody and nanobody library design for display technologies by sampling structurally diverse, backbone-constrained ...

**Related/alternatives:**
- `ABodyBuilder3 Language` — ``ABodyBuilder3 Language`` – Predicts paired antibody structures from sequence, providing backbon...
- `ImmuneFold Antibody` — ``ImmuneFold Antibody`` – Antibody-focused structure prediction to refold and validate AntiFold-d...
- `IgBert Paired` — ``IgBert Paired`` – Antibody sequence language model that can score AntiFold-generated variants f...
- `AbLang-2` — ``AbLang-2`` – Antibody language model for post hoc filtering of AntiFold designs by humanness an...

---

### `biolm-ablef` — BioLM-AbLEF API

**Description:** BioLM-AbLEF is an antibody developability prediction service that uses frozen AbLang embeddings of paired heavy/light Fv sequences to estimate hydrophobic interaction chromatography retention time (HIC-RT) and Fab melting temperature by DSF (Fab T...

**Domain:** antibody, thermostability, viscosity

**Actions:** predict, encode

**Output formats:** CIF, Tm (float), embedding, pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `embedding`
  - **predict**: `hic_rt, fab_tm, embedding`

**Best for:**
- In silico screening of therapeutic IgG1 antibody panels for HIC-RT and Fab melting temperature (Fab Tm) to down-selec...
- Early-stage antibody lead optimization where affinity-matured or humanized IgG1 variants are iteratively proposed and...
- Developability-aware library design for display campaigns and high-throughput antibody generation, where sequence eng...

**Related/alternatives:**
- `AbLang-2` — ``AbLang-2`` – Antibody-specific language model for rapid, sequence-only screening of large reper...
- `ABodyBuilder3 pLDDT` — ``ABodyBuilder3 pLDDT`` – High-accuracy antibody Fv structure predictor that can generate startin...
- `ImmuneFold Antibody` — ``ImmuneFold Antibody`` – End-to-end antibody structure prediction model that can provide alterna...
- `PROPERMAB` — ``PROPERMAB`` – Antibody developability predictor offering complementary sequence- and structure-...

---

### `biolmsol` — BioLMSol API

**Description:** BioLMTox is a GPU-accelerated, single-sequence protein toxin classifier fine-tuned from the 650M-parameter ESM-2 model on a unified benchmark dataset spanning bacterial and eukaryotic proteins and peptides (5–15,639 AA), with ambiguously labeled s...

**Domain:** antibody, solubility

**Actions:** predict

**Output formats:** CIF, embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `solubility_score, profile`

**Best for:**
- High-throughput solubility triage in protein and peptide engineering campaigns BioLMSol can rapidly score variant lib...
- Expression construct design and buffer selection for recombinant protein production CROs, CDMOs, and platform teams c...
- Preclinical developability assessment for biologic portfolios Companies developing protein therapeutics (e.g., enzyme...

**Related/alternatives:**
- `BioLMTox-2` — ``BioLMTox-2`` – General protein toxin classifier fine-tuned on an improved multi-domain toxin da...
- `SoluProt` — ``SoluProt`` – Sequence-based protein solubility predictor
- `Soluble ProteinMPNN` — ``Soluble ProteinMPNN`` – Sequence-design model biased toward soluble proteins
- `ESM-2 650M` — ``ESM-2 650M`` – Protein language model family underlying several BioLM models

---

### `biolmtox` — BioLMTox API

**Description:** *This page explains applications of BioLMTox and documents it's usage for classification and embedding extraction on BioLM.*

**Domain:** toxin

**Output formats:** embedding

---

### `biolmtox2` — BioLMTox-2 API

**Description:** BioLMTox-2 is a GPU-accelerated toxin classification model fine-tuned from ESM-2, offering rapid (<1s per sequence) binary predictions (toxin/not-toxin) directly from amino acid sequences without requiring alignment-based preprocessing. Trained on...

**Domain:** toxin, peptide

**Actions:** predict, encode

**Output formats:** CIF, embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `mean_representation`
  - **predict**: `label, score`

**Best for:**
- Rapid computational screening of engineered therapeutic proteins and peptides for toxicity, enabling biotech companie...
- Biosecurity risk assessment of novel protein sequences generated by generative AI or directed evolution, allowing syn...
- Computational prioritization of peptide-based drug candidates by identifying potential toxicity early in the developm...

**Related/alternatives:**
- `ESM-2 650M` — ``ESM-2 650M`` – BioLMTox-2 is fine-tuned from ESM-2 650M embeddings, making ESM-2 useful for fur...
- `Peptides` — ``Peptides`` – Predicts bioactive peptide properties, complementing BioLMTox-2 by identifying the...
- `ESMFold` — ``ESMFold`` – Provides rapid structure prediction from sequences, enabling structural insights in...
- `AlphaFold2` — ``AlphaFold2`` – Generates highly accurate protein structures, useful for detailed structural ana...

---

### `biotite` — Biotite PDB RMSD API

**Description:** The Biotite PDB RMSD service computes optimal backbone or all-atom root-mean-square deviation between pairs of protein (or other macromolecular) structures using Biotite’s NumPy/Cython-accelerated structure routines. Given two validated PDB string...

**Domain:** structural-comparison

**Actions:** predict

**Output formats:** CIF, PDB, RMSD (float), embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `rmsd`

**Best for:**
- Structural similarity screening in protein engineering campaigns, by computing backbone or Cα RMSD between designed v...
- Conformational stability assessment in enzyme optimization, by measuring RMSD across MD-derived PDB snapshots or betw...
- Antibody structure benchmarking for developability workflows, by comparing predicted or experimentally solved 3D stru...

**Related/alternatives:**
- `AlphaFold2` — ``AlphaFold2`` – Predict high-confidence 3D protein structures from sequence, which can then be c...
- `ESMFold` — ``ESMFold`` – Generate protein structures directly from language models, providing alternative co...
- `ESM-IF1` — ``ESM-IF1`` – Design or evaluate sequences on a fixed backbone
- `TEMPRO 3B` — ``TEMPRO 3B`` – Model protein conformational ensembles and dynamics, where Biotite PDB RMSD is us...

---

### `boltz` — Boltz API

**Description:** Boltz-2 provides GPU-accelerated, structure- and affinity-aware protein–ligand cofolding. Given a protein sequence (FASTA) and small-molecule SMILES, it predicts 3D complex poses plus binding likelihood and a continuous affinity value (IC50‑like),...

**Domain:** structure-prediction, RNA, ligand

**Output formats:** CIF, PDB, RMSD (float), embedding, log-probability, pLDDT, sequence

**Best for:**
- High-throughput virtual screening with structure-based affinity scoring: Use Boltz-2’s binding likelihood plus IC50-l...
- Hit-to-lead and lead optimization ranking within congeneric series: Prioritize analogs by predicted affinity differen...
- Generative design scoring loop for synthesizable lead discovery: Integrate Boltz-2 as the reward signal inside a synt...

**Related/alternatives:**
- `Chai-1` — ``Chai-1`` – Alternative co-folding model
- `ESMFold` — ``ESMFold`` – Fast monomer folding to create apo/domain templates, aiding Boltz-2 pocket localiza...
- `ESM-IF1` — ``ESM-IF1`` – Inverse folding on Boltz-2 poses to propose protein mutations that strengthen pocke...
- `Biotite PDB RMSD` — ``Biotite PDB RMSD`` – Compute RMSD between Boltz-2 poses and references (or across samples) to r...

---

### `boltz2` — Boltz-2 API

**Description:** Boltz-2 is a GPU-accelerated, structure- and affinity-aware biomolecular co-folding model for proteins, nucleic acids, and small-molecule ligands. It predicts 3D complex structures with per-complex and per-chain confidence metrics (pLDDT, pTM, ipT...

**Domain:** structure-prediction, ligand

**Actions:** predict

**Output formats:** CIF, PDB, RMSD (float), embedding, log-probability, pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `cif, plddt, pae, pde, embeddings, s, z, confidence, confidence_score, ptm, iptm, ligand_iptm, protein_iptm, complex_plddt, complex_iplddt, complex_pde, complex_ipde, chains_ptm, pair_chains_iptm, affinity, affinity_pred_value, affinity_probability_binary, affinity_pred_value1, affinity_probability_binary1, affinity_pred_value2, affinity_probability_binary2`

**Best for:**
- High-throughput virtual screening with structure-based affinity scoring: Use Boltz-2’s binding likelihood and IC50-li...
- Hit-to-lead and lead optimization ranking within congeneric series: Prioritize analogs by predicted affinity differen...
- Generative design scoring loop for synthesizable lead discovery: Integrate Boltz-2 as the reward signal inside a synt...

**Related/alternatives:**
- `Chai-1` — ``Chai-1`` – Alternative co-folding model for protein–ligand and multimer complexes
- `ESMFold` — ``ESMFold`` – Fast monomer folding to generate apo/domain protein structures that serve as templa...
- `ESM-IF1` — ``ESM-IF1`` – Inverse folding on ``Boltz-2`` protein–ligand poses to propose sequence mutations t...
- `Biotite PDB RMSD` — ``Biotite PDB RMSD`` – Compute RMSD between ``Boltz-2``-predicted complexes and reference structu...

---

### `chai1` — Chai-1 API

**Description:** Chai-1 is a GPU-accelerated, multi-modal biomolecular structure prediction model that infers 3D structures of proteins, nucleic acids, ligands, and complexes from sequence and SMILES inputs. The API accepts up to 5 molecules per request (proteins ...

**Domain:** structure-prediction, ligand

**Actions:** predict

**Output formats:** CIF, PDB, RMSD (float), embedding, pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `cif, pae, plddt`

**Best for:**
- Protein–small molecule complex prediction to prioritize medicinal chemistry leads
- Antibody–antigen interface modeling to support therapeutic antibody optimization and affinity maturation
- Multimeric protein complex structure prediction to guide protein engineering and synthetic biology

**Related/alternatives:**
- `AlphaFold2` — ``AlphaFold2`` – High-accuracy protein monomer and complex prediction
- `ESMFold` — ``ESMFold`` – Single-sequence protein folding
- `ESM-IF1` — ``ESM-IF1`` – Protein–protein interface prediction
- `ABodyBuilder3 pLDDT` — ``ABodyBuilder3 pLDDT`` – Antibody structure prediction

---

### `clean` — CLEAN API

**Description:** CLEAN is a GPU-accelerated enzyme function prediction service that assigns EC numbers from amino acid sequences using contrastive learning over ESM-1b embeddings refined into a 128-dimensional, function-aware space. The API provides two endpoints:...

**Domain:** enzyme

**Actions:** predict, encode

**Output formats:** CIF, embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `embedding`
  - **predict**: `predictions[*].{ec_number, distance, confidence}`

**Best for:**
- High-confidence EC annotation for novel and low-homology enzyme leads in discovery pipelines, enabling teams to move ...
- Automated functional triage of large enzyme libraries (e.g., >10⁴–10⁶ variants) in directed evolution or semi-rationa...
- Detection and curation of misannotated or ambiguous enzymes in proprietary or public sequence collections, where CLEA...

**Related/alternatives:**
- `ESM-1b` — ``ESM-1b`` – Base protein language model used inside CLEAN
- `ESM-2-650M` — ``ESM-2-650M`` – Newer protein language model that provides alternative embeddings
- `DIAMOND` — ``DIAMOND`` – Fast alignment-based sequence search
- `ZymCTRL` — ``ZymCTRL`` – Enzyme-focused generative model

---

### `deepviscosity` — DeepViscosity API

**Description:** DeepViscosity is an ensemble deep learning classifier for high-concentration monoclonal antibody viscosity that predicts low (≤20 cP) vs high (>20 cP) viscosity at 150 mg/mL directly from paired Fv VH/VL sequences. The service computes 30 DeepSP-d...

**Domain:** antibody, viscosity

**Actions:** predict

**Output formats:** CIF, embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `viscosity_class, probability_mean, probability_std, is_high_viscosity, deepsp_features, SAP_pos_CDRH1, SAP_pos_CDRH2, SAP_pos_CDRH3, SAP_pos_CDRL1, SAP_pos_CDRL2, SAP_pos_CDRL3, SAP_pos_CDR, SAP_pos_Hv, SAP_pos_Lv, SAP_pos_Fv, SCM_neg_CDRH1, SCM_neg_CDRH2, SCM_neg_CDRH3, SCM_neg_CDRL1, SCM_neg_CDRL2, SCM_neg_CDRL3, SCM_neg_CDR, SCM_neg_Hv, SCM_neg_Lv, SCM_neg_Fv, SCM_pos_CDRH1, SCM_pos_CDRH2, SCM_pos_CDRH3, SCM_pos_CDRL1, SCM_pos_CDRL2, SCM_pos_CDRL3, SCM_pos_CDR, SCM_pos_Hv, SCM_pos_Lv, SCM_pos_Fv`

**Best for:**
- In silico screening of monoclonal antibody (mAb) viscosity at clinical concentrations (formulated around 150 mg/mL in...
- Sequence-based design-space exploration for subcutaneous mAb products, where protein engineers generate large panels ...
- Portfolio-level viscosity risk assessment for antibody pipelines, allowing organizations to submit all preclinical or...

**Related/alternatives:**
- `DeepSP` — ``DeepSP`` – Generates the 30 sequence-derived DeepSP surface descriptors (SCM/SAP features) that...
- `DeepSCM` — ``DeepSCM`` – Earlier CNN surrogate for spatial charge maps and viscosity risk from sequence
- `BioLMSol` — ``BioLMSol`` – Predicts apparent solubility and aggregation risk from sequence
- `Pro4S Regression` — ``Pro4S Regression`` – Sequence-based regression for continuous protein stability/solubility scores

---

### `diamond` — DIAMOND API

**Description:** DIAMOND is a fast, sensitive protein sequence alignment service for large-scale homology search, matching BLASTP-level sensitivity in very-sensitive and ultra-sensitive modes while providing ~80–360× speedups. The API accepts protein sequences (va...

**Domain:** sequence-alignment

**Actions:** predict

**Output formats:** CIF, embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `id`

**Best for:**
- High-throughput homology search in enzyme engineering campaigns, using DIAMOND’s very-sensitive or ultra-sensitive mo...
- Tree-of-life scale target scouting for biocatalyst discovery, by aligning metagenomic or proprietary protein catalogs...
- Large-scale off-target and liability assessment for therapeutic protein constructs, calling the DIAMOND predictor end...

**Related/alternatives:**
- `MMseqs2` — ``MMseqs2`` – Alternative large-scale sequence search and clustering engine
- `ESM-2 650M` — ``ESM-2 650M`` – Protein language model for embeddings
- `ESMFold` — ``ESMFold`` – Structure prediction from sequence
- `AlphaFold2` — ``AlphaFold2`` – High-accuracy structure prediction

---

### `dna-chisel` — DNA Chisel API

**Description:** DNA Chisel is a Python-based DNA sequence optimization algorithm designed to solve multi-objective sequence design challenges in synthetic biology. It enables users to define constraints and objectives via Python scripts or annotated Genbank files...

**Domain:** DNA

**Actions:** predict

**Output formats:** CIF, embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `gc_content, cai, hairpin_score, melting_temperature, restriction_site_count, codon_usage_entropy, rare_codon_frequency, homopolymer_run_length, dinucleotide_frequencies, sequence_length, tata_box_count, non_unique_6mer_count, in_frame_stop_codon_count, methionine_frequency, at_skew, gc_skew, nucleotide_entropy, tandem_repeat_count, gc_content_std_dev, kozak_sequence_strength`

**Best for:**
- Codon optimization for heterologous protein expression in industrial host strains, enabling improved protein yield an...
- Removal of problematic restriction enzyme sites and repetitive DNA motifs from synthetic gene constructs, preventing ...
- Multi-constraint DNA sequence optimization for gene therapy vector design, balancing GC content, CpG dinucleotide fre...

**Related/alternatives:**
- `DNABERT-2` — ``DNABERT-2`` – Complements DNA Chisel by providing robust DNA sequence embeddings useful for ide...
- `Omni-DNA 1B` — ``Omni-DNA 1B`` – Enhances DNA Chisel workflows by predicting functional DNA elements, aiding tar...
- `Evo 2 1B Base` — ``Evo 2 1B Base`` – Works alongside DNA Chisel to generate optimized sequences informed by evolut...
- `ESM-2 650M` — ``ESM-2 650M`` – Provides protein-level insights and constraints, useful when DNA Chisel is optim...

---

### `dna_chisel` — DNA-Chisel API

**Description:** DNA-Chisel is a lightweight analytics engine that extracts >20 sequence-level features from raw DNA.  It supports GC-content, codon-adaptation index (CAI), hairpin detection, melting temperature and many more statistics in a single call.

**Domain:** DNA

**Output formats:** embedding, sequence

**Best for:**
- **Codon-usage checks** before synthesis.
- **Promoter / terminator QC** in synthetic circuits.
- **Large-scale feature extraction** feeding sequence-to-vector ML pipelines.

**Related/alternatives:**
- `DNABERT-2` — ``DNABERT-2`` – language-model embeddings for DNA  
- `OmniDNA` — ``OmniDNA`` – large-context DNA transformer

---

### `dnabert` — DNABERT Fine-Tuning

**Description:** On this page, we will show and explain the use of DNABERT. As well as document the BioLM API for fine-tuning, demonstrate no-code and code interfaces.

**Domain:** DNA

**Output formats:** CIF

---

### `dnabert2` — DNABERT-2 API

**Description:** DNABERT-2 is a GPU-accelerated Transformer encoder for genome sequence analysis that uses Byte Pair Encoding (BPE) tokenization and Attention with Linear Biases (ALiBi) to efficiently handle long, multi-species DNA sequences. The API supports batc...

**Domain:** DNA, RNA

**Actions:** predict, encode

**Output formats:** CIF, embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `embedding`
  - **predict**: `embedding, log_prob`

**Best for:**
- Identification and classification of transcription factor binding sites in genomic sequences, enabling researchers to...
- Rapid detection and differentiation of viral genome variants, such as SARS-CoV-2 lineages, allowing biotech firms to ...
- Prediction of promoter and core promoter regions in human genomes, facilitating design of synthetic gene circuits and...

**Related/alternatives:**
- `Omni-DNA 1B` — ``Omni-DNA 1B`` – Large-scale DNA language model offering alternative BPE-based sequence embeddin...
- `nanoBERT` — ``nanoBERT`` – Optimized for nanopore sequencing reads
- `Chai-1` — ``Chai-1`` – Focused on chromatin interaction and 3D genome organization
- `Evo 2 1B Base` — ``Evo 2 1B Base`` – Evolution-aware DNA model that captures cross-species conservation signals, c...

---

### `dsm-150m-base` — DSM 150M Base API

**Description:** DSM 150M Base is a 150M-parameter diffusion-based protein language model extending the ESM2-150M encoder with a masked diffusion objective for both representation learning and sequence generation. The API provides GPU-accelerated embeddings (mean,...

**Domain:** general-protein, stability

**Actions:** predict, encode, generate

**Output formats:** CIF, embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `sequence_index, embeddings, per_residue_embeddings, cls_embeddings`
  - **generate**: `sequence, log_prob, perplexity, sequence2`
  - **predict**: `sequence, log_prob, perplexity, sequence2, sequence_index, embeddings, per_residue_embeddings, cls_embeddings, log_prob, perplexity, sequence_length`

**Best for:**
- High-throughput generative protein design for large-scale sequence exploration, using DSM 150M to sample biomimetic b...
- Sequence completion and repair for noisy or partially known protein constructs, leveraging DSM’s ability to reconstru...
- Embedding-based protein property modeling and ranking, by calling the ``encoder`` endpoint to obtain DSM 150M latent ...

**Related/alternatives:**
- `DSM 650M Base` — ``DSM 650M Base`` – Larger DSM model with the same masked diffusion objective as ``DSM 150M Base`...
- `DSM 650M PPI` — ``DSM 650M PPI`` – Conditional DSM variant fine-tuned for protein–protein interaction design
- `ESM-2 150M` — ``ESM-2 150M`` – The underlying masked language model that DSM further trains with a diffusion ob...
- `Evo 2 1B Base` — ``Evo 2 1B Base`` – A larger, general-purpose sequence modeling and design model that complements...

---

### `dsm-650m-base` — DSM 650M Base API

**Description:** DSM 650M Base is a 650M-parameter diffusion-based protein language model built on an ESM2-650M encoder with a masked diffusion objective for unified representation learning and generative design. The service provides GPU-accelerated endpoints for ...

**Domain:** general-protein, stability

**Actions:** predict, encode, generate

**Output formats:** CIF, embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `sequence_index, embeddings, per_residue_embeddings, cls_embeddings`
  - **generate**: `sequence, log_prob, perplexity, sequence2`
  - **predict**: `sequence, log_prob, perplexity, sequence2, sequence_index, embeddings, per_residue_embeddings, cls_embeddings, log_prob, perplexity, sequence_length`

**Best for:**
- De novo protein sequence generation for industrial enzyme leads, using DSM 650M’s unconditional diffusion sampling (e...
- High-corruption sequence reconstruction to explore local fitness landscapes, by masking large regions (e.g., 30–90%) ...
- Protein representation for downstream predictive models, by encoding sequences with the ``encoder`` endpoint (mean, p...

**Related/alternatives:**
- `DSM 150M Base` — ``DSM 150M Base`` – Smaller DSM variant with the same masked-diffusion objective
- `DSM 650M PPI` — ``DSM 650M PPI`` – DSM variant fine-tuned for conditional binder design
- `ESM-2 650M` — ``ESM-2 650M`` – Underlying MLM checkpoint used to initialize DSM
- `ESM C 600M` — ``ESM C 600M`` – High-performance protein encoder

---

### `dsm-650m-ppi` — DSM 650M PPI API

**Description:** DSM 650M PPI is a 650M-parameter diffusion sequence model for protein–protein interaction–aware design, extended from ESM2 and fine-tuned on ∼646k high-confidence STRING PPI pairs. It generates candidate binders conditioned on a target protein or ...

**Domain:** general-protein, structure-prediction

**Actions:** predict, encode, generate

**Output formats:** CIF, embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `sequence_index, embeddings, per_residue_embeddings, cls_embeddings`
  - **generate**: `sequence, log_prob, perplexity, sequence2`
  - **predict**: `sequence, log_prob, perplexity, sequence2`

**Best for:**
- De novo design of protein binders against therapeutic and industrial targets (DSM 650M PPI) for rapid hit discovery, ...
- Virtual affinity maturation and diversification of known binders by partially masking an existing binder sequence and...
- Large-scale virtual binder library generation for downstream in silico screening by conditionally or unconditionally ...

**Related/alternatives:**
- `DSM 650M Base` — ``DSM 650M Base`` – Shares the same diffusion sequence backbone as ``DSM 650M PPI`` but is traine...
- `Synteract2` — ``Synteract2`` – Protein–protein interaction predictor from the DSM*ppi* work
- `ProteinMPNN` — ``ProteinMPNN`` – Structure-conditioned sequence design model
- `ESM-2 650M` — ``ESM-2 650M`` – The masked language model backbone extended by DSM

---

### `e1-150m` — E1 150M API

**Description:** E1 150M is a retrieval-augmented protein encoder that produces masked language model logits and embeddings for individual residues and whole sequences. The API supports GPU-accelerated, batched inference (up to 8 items, 2,048 residues each, with u...

**Domain:** enzyme, general-protein, stability

**Actions:** predict, encode

**Output formats:** CIF, embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `embeddings[*].{layer, embedding}, per_token_embeddings[*].{layer, embeddings}, logits, vocab_tokens, context_sequence_count`
  - **predict**: `logits, sequence_tokens, vocab_tokens`

**Best for:**
- Zero-shot variant impact scoring for protein engineering campaigns, using E1 150M as a drop-in fitness predictor on w...
- Retrieval-augmented homolog conditioning for difficult protein families where multiple sequence alignments are shallo...
- Embedding-based structure-aware analysis for downstream modeling, where E1 150M encoder outputs (mean or per-token em...

**Related/alternatives:**
- `E1 300M` — ``E1 300M`` – Larger retrieval-augmented encoder from the same E1 family
- `E1 600M` — ``E1 600M`` – Highest-capacity E1 model, offering best zero-shot fitness and contact-map performance
- `ESM-2 650M` — ``ESM-2 650M`` – Strong single-sequence protein language model
- `MSA Transformer` — ``MSA Transformer`` – MSA-based model that explicitly consumes aligned homologs

---

### `e1-300m` — E1 300M API

**Description:** E1 300M is a 300M-parameter retrieval-augmented protein encoder that generates sequence embeddings and zero-shot mutation scores from amino acid input, optionally conditioned on up to 50 unaligned homologous context sequences (each up to 2048 resi...

**Domain:** enzyme, structure-prediction, stability

**Actions:** predict, encode

**Output formats:** CIF, embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `embeddings[*].{layer, embedding}, per_token_embeddings[*].{layer, embeddings}, logits, vocab_tokens, context_sequence_count`
  - **predict**: `logits, sequence_tokens, vocab_tokens`

**Best for:**
- Zero-shot fitness scoring and variant ranking for protein engineering campaigns, using E1 300M log-likelihoods (via `...
- Retrieval-augmented lead optimization within protein families by supplying homologous context sequences in ``encoder`...
- Structure-aware filtering of designed proteins by deriving unsupervised long-range contact signals from E1 300M per-t...

**Related/alternatives:**
- `E1 150M` — ``E1 150M`` – Smaller retrieval-augmented E1 variant
- `E1 600M` — ``E1 600M`` – Larger E1 model that scales performance for zero-shot fitness and structure-related...
- `ESM C 300M` — ``ESM C 300M`` – Sequence-only protein encoder that does not use retrieval
- `DIAMOND` — ``DIAMOND`` – Fast sequence search tool for building homolog sets from large databases

---

### `e1-600m` — E1 600M API

**Description:** E1 600M is a 600M-parameter retrieval-augmented protein encoder that produces sequence embeddings, masked-token logits, and log-probability–based zero-shot variant effect scores from amino acid sequences. The API supports GPU-optimized, batched in...

**Domain:** enzyme, stability

**Actions:** predict, encode

**Output formats:** CIF, embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `embeddings[*].{layer, embedding}, per_token_embeddings[*].{layer, embeddings}, logits, vocab_tokens, context_sequence_count`
  - **predict**: `logits, sequence_tokens, vocab_tokens`

**Best for:**
- Zero-shot fitness scoring and variant ranking for protein engineering campaigns using experimental data such as deep ...
- Retrieval-augmented design of improved enzyme variants by conditioning E1 600M on unaligned homologous sequences from...
- Structure-aware screening and triage of designed protein libraries using unsupervised contact-map signals derived fro...

**Related/alternatives:**
- `E1 300M` — ``E1 300M`` – Smaller retrieval-augmented encoder from the same E1 family
- `ESM-2 650M` — ``ESM-2 650M`` – Single-sequence protein language model
- `MSA Transformer` — ``MSA Transformer`` – Retrieval-augmented model that consumes explicit MSAs
- `ProteinMPNN` — ``ProteinMPNN`` – Structure-conditioned sequence design model

---

### `esm-if1` — ESM-IF1 API

**Description:** ESM-IF1 is an inverse folding model that designs amino acid sequences conditioned on protein backbone atom coordinates (N, Cα, C), using an autoregressive transformer with invariant geometric layers trained on 12M AlphaFold2-predicted structures p...

**Domain:** inverse-folding

**Actions:** generate

**Output formats:** CIF, PDB, embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **generate**: `sequence, recovery` — **NOTE: response path is `results[0][*]`** (array of arrays)

**IMPORTANT:** ESM-IF1 is a GENERAL-PURPOSE inverse folding model (trained on UniProt/PDB, not antibody-specific). For antibody inverse folding, prefer `antifold` which is fine-tuned from ESM-IF1 specifically for antibodies.

**Best for:**
- Fixed-backbone protein sequence design from 3D coordinates, enabling rapid exploration of sequence variants for a giv...
- Optimization of protein–protein interfaces by proposing sequences at a specified backbone interface, supporting desig...
- Zero-shot scoring of sequence variants on a fixed backbone to prioritize mutations that are more compatible with a de...

**Related/alternatives:**
- `AlphaFold2` — ``AlphaFold2`` – Predicts protein structures from amino acid sequences, which you can feed into `...
- `ESMFold` — ``ESMFold`` – Fast single-sequence structure prediction
- `ESM-2 650M` — ``ESM-2 650M`` – Protein language model capturing evolutionary constraints
- `ProstT5 AA2Fold` — ``ProstT5 AA2Fold`` – Sequence-to-structure model that provides alternative backbone predictions,...

---

### `esm1b` — ESM1b API

**Description:** ESM-1b is a 33-layer, ~650M-parameter Transformer protein language model trained on 250M UniRef50 protein sequences (86B amino acids) with a masked language modeling objective. The API provides GPU-accelerated encoder and predictor endpoints that ...

**Domain:** general-protein

**Actions:** predict, encode

**Output formats:** CIF, embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `sequence_index, embeddings[*].{layer, embedding}, bos_embeddings[*].{layer, embedding}, per_token_embeddings[*].{layer, embeddings}, attentions, logits, vocab_tokens`
  - **predict**: `logits, sequence_tokens, vocab_tokens`

**Best for:**
- Protein fitness and mutational scanning surrogates for engineering campaigns: use ESM1b log-likelihoods or masked-tok...
- Structure- and contact-aware feature generation for downstream ML models: use the ``encoder`` endpoint to extract mea...
- Remote homology and scaffolding search in large sequence libraries: embed proprietary or public protein sequence coll...

**Related/alternatives:**
- `ESM-2 650M` — ``ESM-2 650M`` – Next-generation ESM model
- `MSA Transformer` — ``MSA Transformer`` – ESM family model that conditions on multiple sequence alignments
- `ESMFold` — ``ESMFold`` – Structure prediction model built on ESM representations
- `ESM-1v` — ``ESM-1v`` – Variant-effect–oriented ESM model

---

### `esm1v` — ESM-1v API

**Description:** ESM-1v is a masked protein language model ensemble for zero-shot prediction of the effects of mutations on protein function. ESM-1v supports single-residue masked token prediction for mutation effect inference, library scoring, and protein design....

**Domain:** general-protein

**Actions:** predict

**Output formats:** sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `token, token_str, score, sequence`

**Best for:**
- Mutation effect prediction via masked language modeling
- Single-residue functional scoring and variant prioritization
- Ensemble scoring by averaging across model variants

---

### `esm1v-all` — ESM-1v API

**Description:** ESM-1v is a GPU-accelerated protein language model for zero-shot inference of mutational effects on protein function, predicting variant impact directly from amino acid sequences without task-specific training or MSAs. It returns log-odds variant ...

**Domain:** general-protein

**Actions:** predict

**Output formats:** CIF, embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `token, token_str, score, sequence`

**Best for:**
- Predicting functional impacts of protein mutations to accelerate protein engineering workflows, enabling rapid filter...
- Rapid computational screening of protein variants to identify stabilizing mutations or improve thermostability, enabl...
- Identifying critical binding-site residues to guide targeted mutagenesis experiments, enabling protein engineers to f...

**Related/alternatives:**
- `ESM-2 650M` — ``ESM-2 650M`` – Provides an alternative protein language model with similar scale, useful for co...
- `ESMFold` — ``ESMFold`` – Complements ESM-1v by predicting protein structures, linking mutational effects to ...
- `AlphaFold2` — ``AlphaFold2`` – Generates highly accurate protein structures, enabling structural validation of ...
- `ESM-IF1` — ``ESM-IF1`` – Predicts functional impacts of mutations, offering complementary insights into prot...

---

### `esm2-150m` — ESM-2 150M API

**Description:** ESM-2 150M is a GPU-accelerated transformer-based protein language model trained on extensive evolutionary protein sequence data. It generates biologically plausible protein sequences and predicts structural compatibility via learned inter-residue...

**Domain:** general-protein

**Actions:** predict, encode

**Output formats:** CIF, PDB, embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `sequence, recovery`
  - **predict**: `sequence, recovery`

**Best for:**
- De novo protein design for novel therapeutic applications ESM-2 150M can generate entirely new protein sequences that...
- Fixed-backbone protein design for targeted enzyme engineering By generating sequences that fit a predetermined protei...
- Antibody and nanobody optimization for improved binding affinity ESM-2 150M can be used to design antibodies and nano...

**Related/alternatives:**
- `ESM-IF1` — ``ESM-IF1`` – Complements ESM-2 150M by providing inverse folding capabilities to generate protei...
- `ESMFold` — ``ESMFold`` – Uses the ESM architecture to predict protein structures, complementing ESM-2 150M's...
- `AlphaFold2` — ``AlphaFold2`` – Offers highly accurate structure predictions that can validate and refine protei...
- `ESM-2 650M` — ``ESM-2 650M`` – A larger-scale variant of ESM-2 150M, providing improved accuracy and generaliza...

---

### `esm2-35m` — ESM-2 35M API

**Description:** ESM-2 35M is a GPU-accelerated protein language model trained on evolutionary-scale sequence data, enabling the generation and scoring of novel protein sequences beyond known natural families. It supports structure-conditioned design (fixed-backbo...

**Domain:** general-protein

**Actions:** predict, encode

**Output formats:** PDB, embedding, log-probability, pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `pdb, mean_plddt`
  - **predict**: `pdb, mean_plddt`

**Best for:**
- Rapid structure prediction for protein engineering workflows, enabling researchers to quickly screen and prioritize c...
- High-throughput structural annotation of metagenomic protein sequences, allowing biotech companies to rapidly identif...
- Single-sequence structure modeling for protein design tasks, enabling computational design teams to evaluate designed...

**Related/alternatives:**
- `AlphaFold2` — ``AlphaFold2`` – Provides highly accurate structure predictions using MSAs, complementary to ESM-...
- `ESMFold` — ``ESMFold`` – Uses ESM-2 representations to rapidly predict atomic-level protein structures direc...
- `ESM-2 150M` — ``ESM-2 150M`` – A larger-scale version of ESM-2 35M, offering improved accuracy at the cost of i...
- `ESM-IF1` — ``ESM-IF1`` – Inverse folding model utilizing ESM representations, complementary for protein desi...

---

### `esm2-3b` — ESM-2 3B API

**Description:** ESM-2 3B is a transformer protein language model trained with masked language modeling on UniRef-derived evolutionary-scale data, providing rich sequence representations and zero-shot masked amino acid predictions. This API exposes 8M–650M paramet...

**Domain:** general-protein

**Actions:** predict, encode

**Output formats:** embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `sequence_index, embeddings[*].{layer, embedding}, bos_embeddings[*].{layer, embedding}, per_token_embeddings[*].{layer, embeddings}, contacts, attentions, logits, vocab_tokens`
  - **predict**: `sequence_index, embeddings[*].{layer, embedding}, bos_embeddings[*].{layer, embedding}, per_token_embeddings[*].{layer, embeddings}, contacts, attentions, logits, vocab_tokens, logits, sequence_tokens, vocab_tokens`

**Best for:**
- Sequence embeddings for downstream protein property models, using ESM-2 3B encoder representations (``mean``, ``bos``...
- Contact-style and distance-pattern extraction from the ``contacts`` and ``attentions`` outputs of the ``encoder`` end...
- Zero-shot mutation scoring by masking one or more residues and calling the ``predictor`` endpoint to obtain logits ov...

**Related/alternatives:**
- `ESMFold` — ``ESMFold`` – End-to-end single-sequence 3D structure prediction based on ESM-2
- `ESM-IF1` — ``ESM-IF1`` – Inverse folding model that designs sequences for a given backbone structure
- `AlphaFold2` — ``AlphaFold2`` – High-accuracy structure prediction model useful for cross-checking or further an...
- `ESM-2 150M` — ``ESM-2 150M`` – Smaller, faster ESM-2 variant suitable for rapid embedding generation and protot...

---

### `esm2-650m` — ESM-2 650M API

**Description:** ESM-2 650M is a protein language model trained on evolutionary-scale sequence data, enabling generalization beyond natural protein families for de novo protein sequence generation and structure-conditioned design tasks. The model leverages sequenc...

**Domain:** general-protein

**Actions:** predict, encode

**Output formats:** CIF, embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `sequence_index, embeddings[*].{layer, embedding}, bos_embeddings[*].{layer, embedding}, per_token_embeddings[*].{layer, embeddings}, contacts, attentions, logits, vocab_tokens`
  - **predict**: `sequence_index, embeddings[*].{layer, embedding}, bos_embeddings[*].{layer, embedding}, per_token_embeddings[*].{layer, embeddings}, contacts, attentions, logits, vocab_tokens, logits, sequence_tokens, vocab_tokens`

**Best for:**
- Generation of novel protein sequences for therapeutic protein engineering, enabling the design of stable, soluble, an...
- Fixed-backbone protein design for creating stable protein scaffolds, allowing biotech companies to reliably engineer ...
- Unconstrained protein generation for exploring novel protein folds and topologies, enabling biotech researchers to id...

**Related/alternatives:**
- `ESMFold` — ``ESMFold`` – Predicts protein 3D structures directly from sequence, complementing ``ESM-2 650M``...
- `ESM-IF1` — ``ESM-IF1`` – Inverse folding model that generates protein sequences from given structures, ideal...
- `AlphaFold2` — ``AlphaFold2`` – Provides high-accuracy structural predictions that can validate and refine seque...
- `ESM-2 150M` — ``ESM-2 150M`` – Smaller-scale, faster variant of ``ESM-2 650M`` suitable for rapid prototyping b...

---

### `esm2-8m` — ESM-2 8M API

**Description:** ESM-2 8M is a GPU-accelerated protein language model for atomic-level 3D structure prediction directly from amino acid sequences, without requiring multiple sequence alignments or templates. Using transformer-based masked language modeling trained...

**Domain:** general-protein

**Actions:** predict, encode

**Output formats:** PDB, embedding, pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `sequence_index, embeddings[*].{layer, embedding}, bos_embeddings[*].{layer, embedding}, per_token_embeddings[*].{layer, embeddings}, contacts, attentions, logits, vocab_tokens`
  - **predict**: `sequence_index, embeddings[*].{layer, embedding}, bos_embeddings[*].{layer, embedding}, per_token_embeddings[*].{layer, embeddings}, contacts, attentions, logits, vocab_tokens, logits, sequence_tokens, vocab_tokens`

**Best for:**
- Rapid generation of protein structure predictions directly from amino acid sequences, enabling biotech companies to q...
- Initial structure-based filtering of designed protein libraries, allowing protein engineering teams to efficiently pr...
- High-speed structural annotation of metagenomic protein sequences, providing biotech researchers with structural insi...

**Related/alternatives:**
- `AlphaFold2` — ``AlphaFold2`` – Provides complementary high-accuracy structure predictions, useful for validatio...
- `ESMFold` — ``ESMFold`` – Uses ESM-2 embeddings to rapidly predict atomic-level protein structures directly f...
- `ESM-IF1` — ``ESM-IF1`` – Inverse folding model that complements ESM-2 8M by generating protein sequences fro...
- `ESM-2 150M` — ``ESM-2 150M`` – A larger-scale version of ESM-2 8M, offering improved accuracy in structure pred...

---

### `esm2stabp` — ESM2StabP API

**Description:** ESM2StabP predicts protein melting temperature (Tm, °C) from amino acid sequence using esm2_t33_650M_UR50 layer-33 embeddings and a random forest regressor augmented with thermophilic classification, optimal growth temperature, and experimental-co...

**Domain:** thermostability, stability

**Actions:** predict

**Output formats:** CIF, Tm (float), embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `melting_temperature, is_thermophilic`

**Best for:**
- In-silico screening of large protein variant libraries for improved thermostability in industrial enzymes, by ranking...
- Stabilization of biocatalysts used in continuous manufacturing, by using ESM2StabP predictions to prioritize sequence...
- Thermostability-aware protein engineering campaigns, where directed evolution or generative design workflows produce ...

**Related/alternatives:**
- `TemBERTure Regression` — ``TemBERTure Regression`` – Alternative sequence-based *T\ :sub:`m`\* regressor using BERT embedd...
- `TemBERTure Classifier` — ``TemBERTure Classifier`` – Predicts thermophilic vs
- `ThermoMPNN` — ``ThermoMPNN`` – Structure-aware protein design model that proposes stabilizing mutations
- `ThermoMPNN-D` — ``ThermoMPNN-D`` – Design-optimized variant of ``ThermoMPNN`` for stability-focused sequence gene...

---

### `esm3-open-small` — ESM3 Open Small API

**Description:** ESM3 Open Small is a GPU-accelerated protein language model API designed for encoding protein sequences into numerical embeddings and predicting residue-level contacts without MSAs or structural templates. Trained on UniRef50 sequences using rotar...

**Domain:** general-protein, structure-prediction

**Actions:** predict, encode

**Output formats:** CIF, PDB, embedding, pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `pdb, mean_plddt, ptm`
  - **predict**: `pdb, mean_plddt, ptm`

**Best for:**
- Protein structure prediction for novel drug development
- ESM3 enables accurate atomic-level protein structure predictions, which are crucial for identifying binding sites and...
- Pharmaceutical companies can leverage these predictions to design more effective and targeted drugs, reducing the tim...

**Related/alternatives:**
- `ESMFold` — ``ESMFold`` – Integrates ESM language model embeddings to rapidly predict protein structures, com...
- `ESM-2 150M` — ``ESM-2 150M`` – A smaller-scale language model providing efficient embeddings for faster inferen...
- `AlphaFold2` — ``AlphaFold2`` – Provides highly accurate protein structure predictions using MSAs, complementary...
- `ESM-IF1` — ``ESM-IF1`` – Inverse folding model leveraging ESM embeddings, complementary for protein design a...

---

### `esmc-300m` — ESM C 300M API

**Description:** ESM C 300M is a GPU-accelerated protein language model that learns unsupervised representations of amino acid sequences for downstream structure- and function-related tasks. The API exposes two actions: an encoder for sequence embeddings and logit...

**Domain:** general-protein

**Actions:** predict, encode

**Output formats:** CIF, embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `embeddings[*].{layer, embedding}, per_token_embeddings[*].{layer, embeddings}, logits, vocab_tokens`
  - **predict**: `embeddings[*].{layer, embedding}, per_token_embeddings[*].{layer, embeddings}, logits, vocab_tokens, logits, sequence_tokens, vocab_tokens`

**Best for:**
- Designing novel fluorescent proteins with low sequence similarity to known variants, using embeddings from the encode...
- Engineering structurally diverse protein scaffolds around predefined functional motifs by embedding large libraries a...
- Generating and screening sequence variants consistent with specified secondary structure or solvent exposure patterns...

**Related/alternatives:**
- `ESMFold` — ``ESMFold`` – Predicts 3D protein structures from sequences using ESM representations, useful for...
- `ESM-IF1` — ``ESM-IF1`` – Performs inverse folding to design sequences for given backbones
- `AlphaFold2` — ``AlphaFold2`` – Predicts protein structures from sequence
- `ESM3 Open Small` — ``ESM3 Open Small`` – Multimodal generative model over sequence, structure, and function

---

### `esmc-600m` — ESM C 600M API

**Description:** ESM C 600M is a 600M-parameter transformer protein language model (36 layers, width 1152, 18 attention heads) trained with masked language modeling on large UniRef, MGnify, and JGI sequence datasets. The API provides GPU-accelerated encoding and m...

**Domain:** general-protein

**Actions:** predict, encode

**Output formats:** CIF, embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `embeddings[*].{layer, embedding}, per_token_embeddings[*].{layer, embeddings}, logits, vocab_tokens`
  - **predict**: `embeddings[*].{layer, embedding}, per_token_embeddings[*].{layer, embeddings}, logits, vocab_tokens, logits, sequence_tokens, vocab_tokens`

**Best for:**
- Protein embedding generation for downstream predictive modeling, enabling rapid screening of large sequence libraries...
- Unsupervised exploration and clustering of protein sequence space using embeddings from the encoder endpoint to ident...
- Representation learning for protein variant effect modeling, where embeddings serve as features in supervised models ...

**Related/alternatives:**
- `ESM C 300M` — ``ESM C 300M`` – Smaller ESM Cambrian variant with similar representations
- `ESMFold` — ``ESMFold`` – Structure prediction model that converts ESM C 600M sequence embeddings into 3D str...
- `ESM-IF1` — ``ESM-IF1`` – Inverse folding model that designs sequences from target backbones
- `ESM3 Open Small` — ``ESM3 Open Small`` – Multimodal generative model for sequence/structure/function

---

### `esmfold` — ESMFold API

**Description:** ESMFold is a GPU-accelerated, single- and multi-chain protein structure prediction model that infers atomic-level 3D structures directly from protein sequences, leveraging evolutionary-scale language model representations. ESMFold supports high-th...

**Domain:** general-protein, structure-prediction

**Actions:** predict

**Output formats:** PDB string, pLDDT (float), pTM (float)

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `pdb, mean_plddt, ptm`

**Item keys:** `sequence` (single chain or colon-separated for multimers, max 768 aa)

**IMPORTANT:** ESMFold is a GENERAL-PURPOSE structure predictor (trained on ESM-2, no antibody-specific data). For antibody Fv structure prediction, prefer `abodybuilder3-plddt` or `immunefold-antibody` which are specifically trained on antibody structures and produce more accurate CDR conformations. ESMFold outputs PDB format (not CIF).

**Best for:**
- High-throughput single- and multi-chain protein structure prediction
- Metagenomic structure annotation and large-scale dataset curation
- Protein engineering, variant prioritization, and design workflows (non-antibody)

---

### `evo-15-8k-base` — Evo 1.5 8k Base API

**Description:** Evo 1.5 8k Base is a genomic foundation model trained on prokaryotic genomes, enabling predictive modeling and generative design of DNA, RNA, and protein sequences at single-nucleotide resolution. Based on the StripedHyena architecture with GPU-ac...

**Domain:** DNA, RNA

**Actions:** predict, generate

**Output formats:** CIF, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **generate**: `generated, score, log_prob`
  - **predict**: `log_prob`

**Best for:**
- Zero-shot prediction of mutational effects on bacterial protein and RNA function, enabling rapid identification of be...
- Generative design of novel CRISPR-Cas protein-RNA complexes, enabling biotech companies to rapidly generate diverse C...
- In silico identification of essential bacterial genes at whole-genome scale, enabling rapid prioritization of antibio...

**Related/alternatives:**
- `Evo 2 1B Base` — ``Evo 2 1B Base`` – Offers similar genomic-scale modeling and design capabilities as Evo 1.5 8k B...
- `DNABERT-2` — ``DNABERT-2`` – Complements Evo by providing transformer-based nucleotide modeling, useful for sh...
- `ESMFold` — ``ESMFold`` – Works well alongside Evo by predicting protein structures from sequences, enabling ...
- `AlphaFold2` — ``AlphaFold2`` – Provides complementary high-accuracy protein structure prediction, useful for va...

---

### `evo2` — Evo2 1B Base API

**Description:** Evo2-1B-Base is a 1-billion-parameter transformer trained **jointly on DNA, RNA and protein** with an 8 k context window. It exposes three unified BioLM API actions: embed (*encode*), score (*predict*) and autoregressively *generate* new tokens.

**Domain:** DNA, RNA

**Actions:** generate

**Output formats:** embedding, log-probability, sequence

**Best for:**
- **Variant effect prediction** across coding & non-coding regions
- **Multi-omic embeddings** feeding downstream ML classifiers or regressors
- **De-novo biomolecule generation** with long-range context (>8 k tokens)

**Related/alternatives:**
- `DNABERT-2` — ``DNABERT-2`` – Pre-trained masked-language model (DNA-only)
- `OmniDNA` — ``OmniDNA`` – 10 k-context DNA transformer under development

---

### `evo2-1b-base` — Evo 2 1B Base API

**Description:** Evo 2 1B Base is a GPU-accelerated genomic foundation model trained on 1 trillion nucleotides from a curated dataset of prokaryotic and eukaryotic genomes. With a context length of 8,192 tokens and single-nucleotide resolution, Evo 2 1B Base suppo...

**Domain:** DNA

**Actions:** predict, encode, generate

**Output formats:** CIF, embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `embeddings[*].{layer, mean, last}, metadata, processing_time`
  - **generate**: `embeddings[*].{layer, mean, last}, log_prob, generated`
  - **predict**: `embeddings[*].{layer, mean, last}`

**Best for:**
- Predicting functional impacts of genetic variants in protein-coding and noncoding regions, enabling accurate prioriti...
- Designing novel DNA sequences with user-defined chromatin accessibility patterns via inference-time search, allowing ...
- Identification and annotation of genomic features such as exon-intron boundaries, transcription factor binding sites,...

**Related/alternatives:**
- `Evo 1.5 8k Base` — ``Evo 1.5 8k Base`` – Earlier Evo model version optimized for shorter context genomic modeling, u...
- `Omni-DNA 1B` — ``Omni-DNA 1B`` – DNA-focused language model complementary to Evo 2 1B Base, suitable for compara...
- `DNABERT-2` — ``DNABERT-2`` – Transformer-based DNA language model providing alternative embeddings useful for ...
- `ESMFold` — ``ESMFold`` – Protein structure prediction algorithm that complements Evo 2 by generating structu...

---

### `example` — ModelName API

**Description:** Short description of the model and its purpose. Mention key features, supported actions, and typical applications.

**Domain:** general-protein

**Output formats:** varies

**Best for:**
- Bullet points about technical use cases.

---

### `global-label-membrane-mpnn` — Global Label Membrane MPNN API

**Description:** Global Label Membrane MPNN is a ProteinMPNN variant trained to generate protein sequences consistent with membrane protein–like topologies while allowing a global transmembrane vs soluble label. Given backbone structures as PDB input, it produces ...

**Domain:** inverse-folding, membrane

**Actions:** generate

**Output formats:** CIF, PDB, embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **generate**: `sequence, pdb, overall_confidence, ligand_confidence, seq_rec, log_probs, sampling_probs, pdb_packed`

**Best for:**
- Design of soluble GPCR-like scaffolds for ligand screening and SAR campaigns, enabling teams to port the canonical 7T...
- Generation of soluble analogues of other complex membrane-like folds (e.g., rhomboid-like and claudin-like topologies...
- Creation of custom soluble receptor surrogates for high-throughput binder discovery (display libraries, DNA-encoded l...

**Related/alternatives:**
- `Soluble ProteinMPNN` — ``Soluble ProteinMPNN`` – Soluble-only variant of ProteinMPNN used in the original AF2seq–MPNNsol...
- `Per-Residue Label Membrane MPNN` — ``Per-Residue Label Membrane MPNN`` – Adds residue-level membrane/solvent labels instead of a sin...
- `ProteinMPNN` — ``ProteinMPNN`` – General-purpose backbone-conditioned sequence design model
- `LigandMPNN` — ``LigandMPNN`` – Extends MPNN-style design to protein–ligand complexes

---

### `hyper-mpnn` — HyperMPNN API

**Description:** HyperMPNN is a retrained ProteinMPNN-style inverse folding model optimized on AlphaFold-predicted hyperthermophilic proteomes to design protein sequences with thermostability-associated amino acid biases (more apolar cores, more positively charged...

**Domain:** thermostability, stability

**Actions:** generate

**Output formats:** CIF, PDB, Tm (float), embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **generate**: `sequence, pdb, overall_confidence, ligand_confidence, seq_rec, log_probs, sampling_probs, pdb_packed`

**Best for:**
- Thermostabilization of existing industrial enzymes given a reliable 3D structure (experimental or AlphaFold2), by usi...
- Design of highly heat-stable self-assembling protein nanoparticles and carriers (e.g., I53-50-like scaffolds) by rest...
- Pre-screening of stabilizing sequence variants before wet-lab campaigns in protein engineering pipelines, by using Hy...

**Related/alternatives:**
- `ProteinMPNN` — ``ProteinMPNN`` – Base backbone-conditioned sequence design model that HyperMPNN retrains on hype...
- `ThermoMPNN` — ``ThermoMPNN`` – Supervised stability change (ddG) predictor using ProteinMPNN-derived features
- `ESM2StabP` — ``ESM2StabP`` – Sequence-based protein stability predictor
- `TemBERTure Regression` — ``TemBERTure Regression`` – Sequence-based optimal temperature regression model

---

### `igbert-paired` — IgBert Paired API

**Description:** IgBert Paired is an antibody-specific language model trained on over two million naturally paired variable region antibody sequences from the Observed Antibody Space (OAS) database. Utilizing a BERT architecture pretrained on unpaired antibody seq...

**Domain:** antibody

**Actions:** predict, encode, generate

**Output formats:** CIF, embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `embeddings` (optional: `residue_embeddings`, `logits` via `include` param)
  - **generate**: `heavy`, `light`, or `sequence` (depending on paired vs unpaired input)
  - **predict**: `log_prob` (default; optional: `embeddings`, `residue_embeddings`, `logits` via `include` param)

**Item keys:** predict/encode use `heavy` + `light` (paired) or `sequence` (unpaired). generate uses `sequence` with `*` mask placeholders, or `heavy` + `light` with `*` masks.

**Best for:**
- Antibody affinity maturation by predicting beneficial mutations in paired heavy and light chain variable regions, ena...
- Identification of antibody sequences with high naturalness scores (low pseudo-perplexity), enabling researchers and b...
- Generation of contextualized embeddings for antibody variable region sequences, facilitating clustering and visualiza...

**Related/alternatives:**
- `IgT5 Paired` — ``IgT5 Paired`` – Complementary T5-based antibody language model trained on the same paired antib...
- `IgBert Unpaired` — ``IgBert Unpaired`` – Unpaired version of IgBert, useful for tasks where paired data is unavailab...
- `ABodyBuilder3 Language` — ``ABodyBuilder3 Language`` – Predicts antibody structures using language model embeddings, integr...
- `AntiFold` — ``AntiFold`` – Antibody structure prediction algorithm that can leverage embeddings from IgBert P...

---

### `igbert-unpaired` — IgBert Unpaired API

**Description:** IgBert Unpaired is an antibody-specific masked language model (MLM) trained on over two billion unpaired antibody variable region sequences from the Observed Antibody Space (OAS) dataset. Based on a ProtBert architecture, IgBert Unpaired has 420M ...

**Domain:** antibody, general-protein

**Actions:** predict, encode, generate

**Output formats:** CIF, embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `embeddings, residue_embeddings, logits`
  - **generate**: `embeddings, residue_embeddings, logits, heavy, light, sequence, log_prob`
  - **predict**: `embeddings, residue_embeddings, logits`

**Best for:**
- Antibody sequence recovery for therapeutic design and optimization, enabling researchers to accurately reconstruct mi...
- Binding affinity prediction for antibody-antigen interactions, allowing biotech companies to evaluate and rank potent...
- Expression level prediction for engineered antibodies, assisting in the identification of antibody variants with opti...

**Related/alternatives:**
- `IgBert Paired` — ``IgBert Paired`` – Extends IgBert Unpaired by incorporating paired heavy and light chain data, e...
- `IgT5 Unpaired` — ``IgT5 Unpaired`` – Complements IgBert Unpaired by using a T5 architecture, providing alternative...
- `ABodyBuilder3 Language` — ``ABodyBuilder3 Language`` – Uses language model embeddings like IgBert Unpaired to enhance antib...
- `AbLang-2` — ``AbLang-2`` – Similar BERT-based antibody language model, useful for comparative analysis or ens...

---

### `igt5-paired` — IgT5 Paired API

**Description:** IgT5 Paired is an antibody-specific encoder-decoder transformer model trained on 2 million natively paired heavy and light chain sequences from the Observed Antibody Space (OAS) dataset. Leveraging GPU acceleration, the model generates residue-lev...

**Domain:** antibody

**Actions:** encode

**Output formats:** CIF, embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `embeddings, residue_embeddings`

**Best for:**
- Antibody affinity maturation by predicting beneficial mutations in complementarity-determining regions (CDRs), enabli...
- Identification of antibody sequence liabilities through sequence recovery analysis, enabling early detection of probl...
- Generation of antibody embeddings for predictive modeling of antigen binding affinity, enabling ranking and prioritiz...

**Related/alternatives:**
- `IgBert Paired` — ``IgBert Paired`` – Similar paired antibody language model trained on the same OAS dataset
- `AntiFold` — ``AntiFold`` – Antibody structure prediction model that complements IgT5 Paired by translating se...
- `ImmuneFold Antibody` — ``ImmuneFold Antibody`` – Predicts antibody structures from sequences, integrating well with IgT5...
- `ABodyBuilder3 Language` — ``ABodyBuilder3 Language`` – Generates antibody structural models informed by language model embe...

---

### `igt5-unpaired` — IgT5 Unpaired API

**Description:** IgT5 Unpaired is a GPU-accelerated antibody language model trained specifically for the analysis and design of immunoglobulin variable region sequences. Derived from the ProtT5 architecture and pre-trained on approximately 700 million unpaired ant...

**Domain:** antibody

**Actions:** encode

**Output formats:** CIF, embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `embeddings, residue_embeddings`

**Best for:**
- Antibody affinity maturation and optimization by predicting beneficial mutations in complementarity-determining regio...
- Identification of antibody paratope residues through sequence embeddings, allowing efficient prioritization of residu...
- High-throughput antibody sequence filtering and ranking based on predicted antigen binding affinity, enabling rapid s...

**Related/alternatives:**
- `IgT5 Paired` — ``IgT5 Paired`` – A complementary version of IgT5 Unpaired, trained on paired antibody sequences ...
- `IgBert Unpaired` — ``IgBert Unpaired`` – Similar to IgT5 Unpaired but based on the BERT architecture, providing an a...
- `ABodyBuilder3 Language` — ``ABodyBuilder3 Language`` – Predicts antibody structure from sequence embeddings, making it comp...
- `AntiFold` — ``AntiFold`` – Uses inverse folding to design antibody sequences from structural constraints, com...

---

### `immunefold-antibody` — ImmuneFold Antibody API

**Description:** ImmuneFold Antibody is a GPU-accelerated algorithm providing antibody and nanobody structure prediction via parameter-efficient fine-tuning of ESMFold's Evoformer module, using low-rank adaptation (LoRA). It supports inference of unbound antibody ...

**Domain:** antibody, nanobody, structure-prediction

**Actions:** predict

**Output formats:** CIF, PDB, RMSD (float), pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `ptm, full_plddt, plddt, pdb`

**Best for:**
- Rapid antibody structure prediction for therapeutic antibody optimization, enabling biotech companies to efficiently ...
- Nanobody engineering for targeted therapeutics, allowing researchers to quickly predict and optimize nanobody binding...
- Computational antibody-antigen docking and binding affinity estimation, enabling biotech companies to rapidly screen ...

**Related/alternatives:**
- `ESMFold` — ``ESMFold`` – Serves as the foundational model for ImmuneFold, providing the initial protein stru...
- `AlphaFold2` — ``AlphaFold2`` – Known for its high accuracy in protein structure prediction, it complements Immu...
- `TCRBuilder2` — ``TCRBuilder2`` – Focuses on T-cell receptor structure prediction, complementing ImmuneFold's cap...
- `IgFold` — ``IgFold`` – Specializes in antibody structure prediction using language models, offering complem...

---

### `immunefold-tcr` — ImmuneFold TCR API

**Description:** ImmuneFold TCR is a GPU-accelerated structure prediction model for T-cell receptor (TCR) proteins, utilizing parameter-efficient transfer learning (LoRA) based on the ESMFold architecture. It accurately predicts atomic-resolution 3D structures dir...

**Domain:** TCR, structure-prediction

**Actions:** predict

**Output formats:** CIF, PDB, RMSD (float), pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `ptm, full_plddt, plddt, pdb`

**Best for:**
- Accurate TCR structure prediction for immunotherapy development Predicting the structure of T-cell receptors (TCRs) i...
- Zero-shot TCR-epitope binding prediction for neoantigen identification ImmuneFold's integration with Rosetta software...
- Antibody and nanobody structure prediction for therapeutic development Predicting the structures of antibodies and na...

**Related/alternatives:**
- `ImmuneFold Antibody` — ``ImmuneFold Antibody`` – Uses the same LoRA-based fine-tuning of ESMFold specifically adapted fo...
- `TCRBuilder2` — ``TCRBuilder2`` – Provides an alternative deep-learning approach to TCR structure prediction, all...
- `AlphaFold2` — ``AlphaFold2`` – General-purpose protein structure prediction tool that complements ImmuneFold TC...
- `ESMFold` — ``ESMFold`` – The foundational model fine-tuned by ImmuneFold TCR, useful for benchmarking perfor...

---

### `ligand-mpnn` — LigandMPNN API

**Description:** LigandMPNN is a GPU-accelerated, structure-conditioned protein sequence design model that incorporates explicit atomic context from small molecules, nucleic acids, metals, and optional sidechain coordinates. Given fixed backbone and ligand coordin...

**Domain:** inverse-folding, ligand

**Actions:** generate

**Output formats:** CIF, PDB, embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **generate**: `sequence, pdb, overall_confidence, ligand_confidence, seq_rec, log_probs, sampling_probs, pdb_packed`

**Best for:**
- Structure-based optimization of small-molecule binders around fixed ligand poses, using LigandMPNN to redesign residu...
- Design and maturation of protein-based small-molecule sensors, where LigandMPNN is used to refine or re-pack binding ...
- Sequence design of protein–DNA interfaces for sequence-specific DNA binders, by conditioning on the atomic coordinate...

**Related/alternatives:**
- `ProteinMPNN` — ``ProteinMPNN`` – Backbone-only sequence design model that LigandMPNN extends
- `SolubleMPNN` — ``SolubleMPNN`` – ProteinMPNN variant biased toward soluble designs
- `ESMFold` — ``ESMFold`` – Structure prediction from sequence
- `AlphaFold2` — ``AlphaFold2`` – High-accuracy structure prediction for designed complexes

---

### `msa-transformer` — MSA Transformer API

**Description:** MSA Transformer is an unsupervised protein language model that operates directly on multiple sequence alignments using interleaved row and column (axial) attention to capture both evolutionary covariation and sequence-level patterns with ~100M par...

**Domain:** general-protein

**Actions:** encode

**Output formats:** CIF, embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `sequence_index, embeddings[*].{layer, embedding}, per_token_embeddings[*].{layer, embeddings}, row_attentions, contacts`

**Best for:**
- Unsupervised contact map estimation from MSAs for structure-aware protein engineering: use attention-derived contact ...
- MSA-driven feature generation for supervised property predictors: extract per-residue embeddings and pairwise feature...
- Triage of designed or mutagenized variant libraries using approximate structural constraints: run candidate variant M...

**Related/alternatives:**
- `ESM1b` — ``ESM1b`` – Single-sequence protein language model whose attention-based contact signals provide ...
- `ESM-2 650M` — ``ESM-2 650M`` – Next-generation transformer for single protein sequences
- `Evo 1.5 8k Base` — ``Evo 1.5 8k Base`` – Evolutionary sequence model operating on families of related sequences
- `ESMFold` — ``ESMFold`` – End-to-end 3D structure prediction model

---

### `nanobert` — nanoBERT API

**Description:** nanoBERT is a transformer-based nanobody (VHH) language model for context-aware amino acid substitution prediction and sequence scoring. Trained on 10 million camelid nanobody sequences from INDI, it operates in a germline-agnostic manner and supp...

**Domain:** nanobody

**Actions:** predict, encode, generate

**Output formats:** CIF, embedding, log-probability, pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `embeddings, residue_embeddings, logits`
  - **generate**: `sequence`
  - **predict**: `embeddings, residue_embeddings, logits`

**Best for:**
- Predicting feasible amino acid substitutions in therapeutic nanobodies by scoring masked positions with the predictor...
- Computational humanization of llama-derived nanobodies by comparing nanoBERT nativeness-style scores or positional pr...
- Fine-tuning nanoBERT embeddings on experimentally measured nanobody thermostability datasets (e.g., NBThermo) using t...

**Related/alternatives:**
- `NanoBodyBuilder2` — ``NanoBodyBuilder2`` – Predicts 3D structures of nanobodies, complementing ``nanoBERT``’s sequenc...
- `AbLang-2` — ``AbLang-2`` – Antibody-specific language model useful for comparing human antibody and nanobody ...
- `ABodyBuilder3 pLDDT` — ``ABodyBuilder3 pLDDT`` – Builds antibody variable-domain structures with confidence scores
- `ImmuneFold Antibody` — ``ImmuneFold Antibody`` – Antibody structure prediction model that can be applied to single-domai...

---

### `nanobodybuilder2` — NanoBodyBuilder2 API

**Description:** NanoBodyBuilder2 is a nanobody-specific deep learning model from the ImmuneBuilder suite that predicts all-atom 3D structures for single-chain VHH sequences. Given a heavy-chain-only amino acid sequence (H) up to 2048 residues, the API returns a r...

**Domain:** nanobody, structure-prediction

**Actions:** predict

**Output formats:** CIF, PDB, RMSD (float), embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `pdb`

**Best for:**
- Structure-guided nanobody humanization by modelling how candidate framework and CDR substitutions perturb the 3D Ig f...
- Affinity maturation support for nanobodies by rapidly predicting 3D structures of mutational variants so in silico li...
- Stability-oriented engineering of nanobody candidates using predicted structures to flag mutations that improve core ...

**Related/alternatives:**
- `nanoBERT` — ``nanoBERT`` – Transformer model trained for nanobody sequence infilling
- `ABodyBuilder2` — ``ABodyBuilder2`` – Antibody-specific structure prediction
- `TCRBuilder2` — ``TCRBuilder2`` / ``TCRBuilder2+`` – T-cell receptor structure prediction
- `AbLang-2` — ``AbLang-2`` – Antibody language model for sequence completion and optimisation

---

### `omni-dna-1b` — Omni-DNA 1B API

**Description:** Omni-DNA 1B is an autoregressive genomic foundation model (1B parameters) enabling GPU-accelerated inference for cross-modal and multi-task DNA sequence analysis. The model supports tasks including DNA classification, functional annotation generat...

**Domain:** DNA

**Actions:** predict, encode

**Output formats:** CIF, embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `mean[*].{embedding}, last[*].{embedding}`
  - **predict**: `mean[*].{embedding}, last[*].{embedding}, log_prob`

**Best for:**
- Identification and annotation of regulatory DNA elements in genomic sequences, enabling researchers to rapidly pinpoi...
- Simultaneous prediction of multiple epigenetic modifications (e.g., acetylation and methylation) from DNA sequence al...
- Cross-modal mapping of DNA sequences to functional textual annotations, allowing rapid functional characterization of...

**Related/alternatives:**
- `DNABERT-2` — ``DNABERT-2`` – Complements Omni-DNA by providing efficient bidirectional transformer-based model...
- `ESM-2 650M` — ``ESM-2 650M`` – Offers protein sequence modeling capabilities that pair well with Omni-DNA's gen...
- `ESMFold` — ``ESMFold`` – Enables structural predictions from protein sequences, complementing Omni-DNA's cro...
- `AlphaFold2` — ``AlphaFold2`` – Provides state-of-the-art protein structure prediction, integrating effectively ...

---

### `peptides` — Peptides API

**Description:** Peptides computes a focused set of physicochemical descriptors for antimicrobial peptide (AMP) analysis, including sequence length, amino acid composition, net charge, aliphatic index, molecular weight, isoelectric point, hydrophobicity (GRAVY), i...

**Domain:** peptide

**Actions:** encode

**Output formats:** CIF, embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `features, aliphatic_index, boman, charge, descriptors, length, amino_acid_composition, Tiny, number, mole_percent, Small, number, mole_percent, Aliphatic, number, mole_percent, Aromatic, number, mole_percent, NonPolar, number, mole_percent, Polar, number, mole_percent, Charged, number, mole_percent, Basic, number, mole_percent, Acidic, number, mole_percent, frequencies, hydrophobic_moment, hydrophobicity, instability_index, isoelectric_point, mass_shift, molecular_weight, mz, hydrophobic_moment_profile, hydrophobicity_profile, linker_preference_profile`

**Best for:**
- Rapid screening and prioritization of antimicrobial peptide (AMP) candidates by computing physicochemical descriptors...
- Optimization of membrane-active peptide antibiotics by quantifying Boman index, hydrophobicity, and hydrophobic momen...
- Stability and manufacturability assessment of AMP hits using aliphatic and instability indices alongside molecular we...

**Related/alternatives:**
- `ESM-1v` — ``ESM-1v`` – Provides sequence-based protein function predictions that can be combined with Pepti...
- `BioLMTox-2` — ``BioLMTox-2`` – Predicts peptide toxicity, complementing Peptides physicochemical profiles when ...
- `AlphaFold2` — ``AlphaFold2`` – Predicts 3D structures from sequences, useful for structurally validating AMP de...
- `ESMFold` — ``ESMFold`` – Offers fast sequence-to-structure predictions that can help interpret how Peptides-...

---

### `per-residue-label-membrane-mpnn` — Per-Residue Label Membrane MPNN API

**Description:** Per-Residue Label Membrane MPNN is a CPU-accelerated variant of ProteinMPNN that designs amino acid sequences on fixed backbones while explicitly modeling membrane environments at per-residue resolution. Given a PDB structure, it samples sequences...

**Domain:** inverse-folding, membrane

**Actions:** generate

**Output formats:** CIF, PDB, embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **generate**: `sequence, pdb, overall_confidence, ligand_confidence, seq_rec, log_probs, sampling_probs, sequence, pdb, overall_confidence, ligand_confidence, seq_rec, log_probs, sampling_probs, pdb_packed`

**Best for:**
- Rapid assessment of transmembrane vs soluble exposure in designed proteins by assigning per-residue membrane/soluble ...
- Design of soluble analogues of membrane protein scaffolds for screening and mechanistic assays by mapping per-residue...
- Targeted re-embedding and re-parameterization of membrane-facing residues in computational protein redesign pipelines...

**Related/alternatives:**
- `Soluble ProteinMPNN` — ``Soluble ProteinMPNN`` – Backbone-conditioned sequence design model trained only on soluble prot...
- `Global Label Membrane MPNN` — ``Global Label Membrane MPNN`` – Uses a single global membrane/soluble label to bias designs
- `ProteinMPNN` — ``ProteinMPNN`` – General-purpose backbone-conditioned sequence design model underlying the membr...
- `LigandMPNN` — ``LigandMPNN`` – Ligand-aware MPNN variant for designing binding sites

---

### `pro4s-classification` — Pro4S Classification API

**Description:** Pro4S Classification is a multimodal protein solubility predictor that fuses ESM-based sequence embeddings, AlphaFold-derived 3D structure graphs, and MaSIF-style surface descriptors using GNNs and contrastive learning. The API takes amino acid se...

**Domain:** stability, solubility

**Actions:** predict

**Output formats:** CIF, PDB, embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `solubility_score, solubility_label`

**Best for:**
- Up-front solubility screening of therapeutic protein leads (e.g., enzymes, cytokines, scaffolds) before expression ca...
- Solubility-aware protein engineering workflows, where rational or generative variant libraries are ranked by Pro4S sc...
- Optimization of expression constructs and domain architectures for industrial proteins (e.g., multi-domain catalysts,...

**Related/alternatives:**
- `BioLMSol` — ``BioLMSol`` – Sequence-based solubility predictor for cases without structural data
- `SoluProt` — ``SoluProt`` – Solubility predictor for recombinant expression in *E
- `Soluble ProteinMPNN` — ``Soluble ProteinMPNN`` – Sequence design model biased toward soluble proteins
- `ThermoMPNN` — ``ThermoMPNN`` – Stability-oriented sequence design model

---

### `pro4s-regression` — Pro4S Regression API

**Description:** Pro4S Regression predicts quantitative protein solubility (0–1 scale) from protein sequence plus 3D structure, combining ESM-3B sequence embeddings, AlphaFold-derived structure graphs, and MaSIF surface descriptors in a unified GNN. The API accept...

**Domain:** stability, solubility, viscosity

**Actions:** predict

**Output formats:** CIF, PDB, embedding, pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `solubility_score, solubility_label`

**Best for:**
- Upfront triage of *de novo* designed binders and other recombinant proteins for expression campaigns by ranking candi...
- Optimization of recombinant protein constructs for *E
- Developability risk assessment for therapeutic and industrial protein leads by comparing regression solubility scores...

**Related/alternatives:**
- `SoluProt` — ``SoluProt`` – Sequence-only solubility predictor useful when no structure is available
- `BioLMSol` — ``BioLMSol`` – PLM-based solubility regression model
- `ThermoMPNN` — ``ThermoMPNN`` – Structure-based protein design model
- `ESMFold` — ``ESMFold`` – Fast structure prediction from sequence

---

### `progen2` — ProGen2 API

**Description:** ProGen2 is a large-scale autoregressive protein language model suite for protein sequence generation and fitness prediction. BioLM provides scalable API access to multiple ProGen2 variants (OAS, Medium, Large, BFD90) for modern protein engineering...

**Domain:** general-protein

**Actions:** generate

**Output formats:** CIF, embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **generate**: `sequence, ll_sum, ll_mean`

**Best for:**
- De novo protein sequence generation from motifs or partial sequences
- Antibody/nanobody library design (e.g., CDR diversification)
- Enzyme engineering and synthetic protein design

---

### `progen2-bfd90` — ProGen2 BFD90 API

**Description:** ProGen2 BFD90 is a 2.7B-parameter autoregressive protein language model trained on ~1B sequences from UniRef90 and a 90% identity–clustered BFD metagenomic dataset, emphasizing broader metagenomic coverage than standard ProGen2. The API exposes GP...

**Domain:** general-protein

**Actions:** generate

**Output formats:** CIF, embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **generate**: `sequence, ll_sum, ll_mean`

**Best for:**
- Protein variant scoring for industrial enzyme campaigns, using ProGen2 BFD90 log-likelihoods to rank mutation librari...
- Zero-shot fitness-style prioritization of low-homology or highly mutated protein designs by comparing ProGen2 BFD90 l...
- Generative expansion of protein families for library design, by using the generator endpoint to sample ProGen2 BFD90 ...

**Related/alternatives:**
- `ProGen2 Large` — ``ProGen2 Large`` – Same model family as ProGen2 BFD90 but trained on a different sequence mix
- `ESM-1v` — ``ESM-1v`` – Zero-shot mutation effect predictor that complements ProGen2 BFD90 by independently ...
- `ProteinMPNN` — ``ProteinMPNN`` – Structure-conditioned sequence designer that pairs well with ProGen2 BFD90 by r...
- `ESMFold` — ``ESMFold`` – Fast structure prediction model that can be used after ProGen2 BFD90 to assess fold...

---

### `progen2-large` — ProGen2 Large API

**Description:** ProGen2 Large is a 2.7B-parameter autoregressive protein language model for sequence continuation and likelihood-based scoring of amino acid sequences. The API accepts raw protein sequences up to 512 residues and can generate up to 3 continuations...

**Domain:** general-protein

**Actions:** generate

**Output formats:** CIF, PDB, embedding, log-probability, pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **generate**: `sequence, ll_sum, ll_mean`

**Best for:**
- Protein variant scoring to prioritize candidates before wet-lab screening, using ProGen2 Large log-likelihoods (``ll_...
- Zero-shot fitness estimation on mutational landscapes that are several edits away from any known sequence for challen...
- Generative library design for protein engineering campaigns, where ProGen2 Large (via the ``generator`` endpoint with...

**Related/alternatives:**
- `ProGen2 Medium` — ``ProGen2 Medium`` – Smaller ProGen2 variant with the same architecture and training objective
- `ProGen2 BFD90` — ``ProGen2 BFD90`` – ProGen2 model trained with a metagenomic-enriched dataset
- `ESM-1v` — ``ESM-1v`` – Zero-shot fitness prediction–oriented protein language model
- `ProteinMPNN` — ``ProteinMPNN`` – Structure-conditioned sequence design model

---

### `progen2-medium` — ProGen2 Medium API

**Description:** ProGen2 Medium is a 764M-parameter autoregressive protein language model trained on ~1B sequences (UniRef90 plus metagenomic BFD30/BFD90) to model natural protein sequence distributions, generate variants, and score sequences by log-likelihood. Th...

**Domain:** general-protein

**Actions:** generate

**Output formats:** CIF, embedding, log-probability, sequence

**Best for:**
- Zero-shot prioritization of protein variant libraries in engineering campaigns, by scoring large in silico mutagenesi...
- Generative design of de novo protein sequences with realistic folds, by sampling from ProGen2 Medium under tuned temp...
- Architecture-focused diversification of protein families, by fine-tuning ProGen2 Medium on a curated set of proteins ...

**Related/alternatives:**
- `ProGen2 Large` — ``ProGen2 Large`` – Larger ProGen2 variant trained on the same data mixture
- `ProGen2 BFD90` — ``ProGen2 BFD90`` – ProGen2 model emphasizing metagenomic diversity
- `ESM-1v` — ``ESM-1v`` – Protein language model optimized for zero-shot variant effect prediction
- `ProteinMPNN` — ``ProteinMPNN`` – Structure-conditioned sequence design model

---

### `progen2-oas` — ProGen2 OAS API

**Description:** ProGen2 OAS is a 764M-parameter antibody-focused autoregressive protein language model trained on 554M redundancy-reduced sequences from the Observed Antibody Space (OAS) database. This API variant generates heavy-chain variable-region sequences c...

**Domain:** antibody, general-protein

**Actions:** generate

**Output formats:** CIF, embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **generate**: `sequence, ll_sum, ll_mean`

**Best for:**
- Generation of diverse, human-like antibody VH sequence libraries for discovery campaigns using PROGEN2-OAS, enabling ...
- In silico exploration of developability-relevant VH sequence neighborhoods (e.g., aggregation propensity, solubility,...
- Zero-shot prioritization of VH variants in affinity maturation or library-mining campaigns by using PROGEN2-OAS log-l...

**Related/alternatives:**
- `ProGen2 Large` — ``ProGen2 Large`` – General-purpose ProGen2 model trained on diverse proteins
- `ESM-1v` — ``ESM-1v`` – Zero-shot variant effect predictor that complements ProGen2 OAS by ranking antibody ...
- `IgT5 Unpaired` — ``IgT5 Unpaired`` – Antibody-specific sequence model for unpaired chains that can be combined wit...
- `ImmuneFold Antibody` — ``ImmuneFold Antibody`` – Antibody structure prediction model to assess structural plausibility a...

---

### `propermab` — PROPERMAB API

**Description:** PROPERMAB is an *in silico* antibody developability framework exposed as an API for feature extraction from paired VH/VL sequences. Given heavy and light chain Fv inputs (100–200 aa, batch size 1), it predicts Fv structures via ABodyBuilder2 and c...

**Domain:** antibody, viscosity

**Actions:** predict

**Output formats:** CIF, embedding, pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `sequence_features, theoretical_pi, n_charged_res, n_charged_res_fv, fv_charge, fv_csp, fc_charge, fab_fc_csp, structure_features, net_charge, exposed_net_charge, net_charge_cdr, exposed_net_charge_cdr, scm, dipole_moment, hyd_asa, hph_asa, hyd_moment, heiden_score, hyd_patch_area, hyd_patch_area_cdr, pos_patch_area, pos_patch_area_cdr, neg_patch_area, neg_patch_area_cdr, aromatic_asa, aromatic_cdr, exposed_aromatic, pos_ann_index, neg_ann_index, aromatic_ann_index, pos_ripley_k, neg_ripley_k, aromatic_ripley_k, Fv_chml, exposed_Fv_chml, cdr_h3_length, metadata, num_runs, isotype, lc_type, structure_prediction_method, feature_calculation_version`

**Best for:**
- Lead candidate triage for therapeutic mAbs based on developability risk, using PROPERMAB-predicted HIC retention time...
- In silico liability mapping and sequence optimization for mAb manufacturability, by quantifying charge asymmetry, hyd...
- Rapid developability screening of large antibody repertoires (10⁴–10⁶ sequences) by first extracting sequence-only ch...

**Related/alternatives:**
- `ABodyBuilder3 pLDDT` — ``ABodyBuilder3 pLDDT`` – High-accuracy antibody Fv structure prediction that can be used upstrea...
- `ImmuneFold Antibody` — ``ImmuneFold Antibody`` – Alternative antibody structure predictor that can generate ensembles of...
- `DeepViscosity` — ``DeepViscosity`` – A viscosity-focused deep learning model that complements PROPERMAB by providi...
- `BioLMSol` — ``BioLMSol`` – Sequence-based solubility predictor that can be run alongside PROPERMAB to provide...

---

### `prostt5` — ProstT5 API

**Description:** ProstT5 is a bilingual protein language model capable of translating between amino acid (AA) sequences and 3Di structure tokens, enabling both sequence-to-structure (folding) and structure-to-sequence (inverse folding) tasks. ProstT5 supports two ...

**Domain:** general-protein, structure-prediction

**Actions:** encode, generate

**Output formats:** CIF, RMSD (float), embedding, pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `token, token_str, score, sequence`
  - **generate**: `sequence`

**Best for:**
- High-throughput protein design: Translate between sequence and structure representations for novel protein engineerin...
- Structure annotation: Rapidly annotate large protein datasets with 3Di structure tokens, enabling proteome-scale and ...
- Remote homology detection: Use ProstT5-predicted 3Di tokens as input to structure-based search tools (Foldseek), enab...

---

### `prostt5-aa2fold` — ProstT5 AA2Fold API

**Description:** ProstT5 AA2Fold is a GPU-accelerated encoder-decoder model that translates amino acid (AA) sequences into 3Di structure tokens, enabling rapid protein structure inference. Derived by fine-tuning ProtT5 on 17M AlphaFold2 predictions encoded via Fol...

**Domain:** structure-prediction

**Actions:** encode, generate

**Output formats:** embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `mean_representation`
  - **generate**: `mean_representation, token, token_str, score, sequence`

**Best for:**
- Rapid structural annotation of novel protein sequences by predicting 3Di structure tokens directly from amino acid se...
- Accelerated inverse folding tasks by generating diverse amino acid sequences conditioned on a target 3Di structural r...
- High-throughput fold classification of large protein libraries by leveraging ProstT5 embeddings for unsupervised stru...

**Related/alternatives:**
- `ProstT5 Fold2AA` — ``ProstT5 Fold2AA`` – Complementary to ``ProstT5 AA2Fold``, translates structure (3Di) sequences ...
- `Foldseek` — ``Foldseek`` – Directly complementary, uses 3Di sequences generated by ``ProstT5 AA2Fold`` to rap...
- `ESMFold` — ``ESMFold`` – Complementary structure prediction algorithm
- `AlphaFold2` — ``AlphaFold2`` – Provides accurate 3D structure predictions used to train ``ProstT5 AA2Fold``, es...

---

### `prostt5-fold2aa` — ProstT5 Fold2AA API

**Description:** ProstT5 Fold2AA is a GPU-accelerated bilingual protein language model that translates structural protein representations (Foldseek 3Di tokens) into corresponding amino acid sequences. Utilizing a transformer-based encoder-decoder architecture trai...

**Domain:** inverse-folding

**Actions:** encode, generate

**Output formats:** CIF, RMSD (float), embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `mean_representation`
  - **generate**: `sequence`

**Best for:**
- Rapid screening of metagenomic sequence databases for remote homolog detection by converting amino acid sequences dir...
- High-throughput inverse folding for protein design by generating novel amino acid sequences conditioned on a desired ...
- Structural embedding extraction for protein fold classification and annotation transfer, leveraging ProstT5 embedding...

**Related/alternatives:**
- `ProstT5 AA2Fold` — ``ProstT5 AA2Fold`` – Complementary to ProstT5 Fold2AA, translates amino acid sequences into 3Di ...
- `Foldseek` — ``Foldseek`` – Uses 3Di sequences for rapid structural similarity searches
- `ESMFold` — ``ESMFold`` – Predicts atomic-level 3D structures from amino acid sequences
- `AlphaFold2` — ``AlphaFold2`` – Provides highly accurate protein structure predictions used to train ProstT5 Fol...

---

### `protein-mpnn` — ProteinMPNN API

**Description:** ProteinMPNN is a graph-based inverse folding model for fixed-backbone protein sequence design, supporting single- and multi-chain complexes, symmetry tying, and order-agnostic decoding. Given atomic protein backbones (PDB), it generates diverse am...

**Domain:** inverse-folding

**Actions:** generate

**Output formats:** CIF, PDB, RMSD (float), embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **generate**: `sequence, pdb, overall_confidence, ligand_confidence, seq_rec, log_probs, sampling_probs, pdb_packed`

**Best for:**
- Fixed–backbone sequence design to improve stability and expression of industrial proteins and biologics from an exper...
- Symmetry-aware sequence design for self-assembling protein nanoparticles and multimeric scaffolds, where ProteinMPNN ...
- Interface redesign for protein–protein binders and adaptor proteins by mixing fixed and designable regions on multi-c...

**Related/alternatives:**
- `ESM-IF1` — ``ESM-IF1`` – Structure-conditioned inverse folding model that, like ProteinMPNN, designs sequenc...
- `LigandMPNN` — ``LigandMPNN`` – Extends ProteinMPNN to protein–ligand complexes
- `Soluble ProteinMPNN` — ``Soluble ProteinMPNN`` – ProteinMPNN variant trained on soluble proteins
- `ESMFold` — ``ESMFold`` – Sequence-to-structure predictor frequently used after ProteinMPNN to rapidly check ...

---

### `protgpt2` — ProtGPT2 Info

**Description:** On this page, we will show and explain the use of ProtGPT2. As well as document the BioLM API for fine-tuning, demonstrate no-code and code interfaces.

**Domain:** general-protein

**Output formats:** CIF, log-probability, sequence

---

### `sadie-antibody` — Sadie Antibody API

**Description:** Sadie Antibody is an antibody-specific inverse folding model fine-tuned from ESM-IF1, optimized for generating structurally accurate antibody sequences from solved or predicted variable domain structures. It predicts residue-level mutation toleran...

**Domain:** antibody

**Actions:** predict

**Output formats:** CIF, PDB, RMSD (float), embedding, pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `sequences[*].{global_score, score, heavy, light, temperature, mutations, seq_recovery}, logprobs, logits, pdb_posins, pdb_chain, pdb_res, top_res, perplexity, vocab`

**Best for:**
- Antibody design and optimization for therapeutic applications: AntiFold enables researchers to design antibodies with...
- Predictive modeling for antibody-antigen binding affinity: AntiFold's ability to predict binding affinity in a zero-s...
- Structural conservation in antibody engineering: By generating sequences that preserve the structural fold, AntiFold ...

**Related/alternatives:**
- `ABodyBuilder3 pLDDT` — ``ABodyBuilder3 pLDDT`` – Accurately predicts antibody variable domain structures, providing reli...
- `IgBert Paired` — ``IgBert Paired`` – Predicts antibody properties from paired heavy-light chain sequences, complem...
- `ImmuneFold Antibody` — ``ImmuneFold Antibody`` – Generates antibody structural models, useful as inputs for Sadie Antibo...
- `AntiFold` — ``AntiFold`` – Performs inverse folding for antibody design, effectively guiding Sadie Antibody's...

---

### `soluble-mpnn` — Soluble ProteinMPNN API

**Description:** Soluble ProteinMPNN is a structure-conditioned sequence design service for generating soluble analogs of integral membrane protein folds. Given a single backbone in PDB format (one structure per request), it applies a ProteinMPNN model retrained o...

**Domain:** inverse-folding, solubility

**Actions:** generate

**Output formats:** CIF, PDB, RMSD (float), embedding, log-probability, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **generate**: `sequence, pdb, overall_confidence, ligand_confidence, seq_rec, log_probs, sampling_probs, pdb_packed`

**Best for:**
- Designing soluble GPCR-like scaffolds for high-throughput ligand screening and SAR campaigns by generating water-solu...
- Engineering soluble analogues of other membrane folds (for example, claudin-like and rhomboid-like helices) as tracta...
- Creation of de novo soluble helical scaffolds with predefined complex topologies for small-molecule and peptide binde...

**Related/alternatives:**
- `AlphaFold2` — ``AlphaFold2`` – Predicts 3D structures for Soluble ProteinMPNN-designed sequences, enabling in s...
- `ESM-IF1` — ``ESM-IF1`` – Provides an alternative structure-conditioned sequence design model for the same ba...
- `ESMFold` — ``ESMFold`` – Performs fast single-sequence structure prediction for redesigned proteins, enablin...
- `ProteinMPNN` — ``ProteinMPNN`` – Backbone-conditioned sequence design model trained on mixed soluble/membrane data

---

### `soluprot` — SoluProt API

**Description:** SoluProt predicts the probability that a protein will be expressed in a soluble form in *E. coli* using a gradient boosting classifier trained on a curated, length-balanced subset of the TargetTrack database and evaluated on an independent NESG be...

**Domain:** solubility

**Actions:** predict

**Output formats:** CIF, embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `soluble, is_soluble`

**Best for:**
- Pre-screening enzyme variant libraries for soluble overexpression in *E
- Selecting soluble orthologs or homologs from metagenomic or public sequence databases as starting points for industri...
- Prioritizing soluble protein constructs (e.g., domain boundaries, truncations, tag-only fusions) for structural biolo...

**Related/alternatives:**
- `BioLMSol` — ``BioLMSol`` – Sequence-based solubility predictor that can be used alongside SoluProt to cross-c...
- `Soluble ProteinMPNN` — ``Soluble ProteinMPNN`` – Sequence design model that optimizes both foldability and solubility
- `ThermoMPNN` — ``ThermoMPNN`` – Predicts and optimizes protein thermostability
- `ESM-2 650M` — ``ESM-2 650M`` – General protein language model that can generate or score diverse sequence variants

---

### `spurs` — SPURS API

**Description:** SPURS predicts protein thermostability changes (ΔΔG, kcal/mol) for all single amino acid substitutions in a protein in a single forward pass. It rewires an ESM protein language model with structure priors from ProteinMPNN via Adapter attention, co...

**Domain:** stability, thermostability

**Actions:** predict

**Output formats:** CIF, PDB, Tm (float), log-probability, pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `mutations, ddG, ddG_contributions, ddG_matrix, values, residue_axis, amino_acid_axis`

**Best for:**
- High-throughput stabilizing-mutation discovery for industrial enzymes by running single-pass ΔΔG scans of all 20 subs...
- Antibody and nanobody developability optimization by prioritizing framework or periphery CDR substitutions predicted ...
- Recombinant construct rescue for difficult-to-express proteins by proposing stabilizing point mutations that improve ...

**Related/alternatives:**
- `AlphaFold2` — ``AlphaFold2`` – Provides 3D structures when no experimental model is available
- `ESM-1v` — ``ESM-1v`` – Zero-shot variant effect model
- `ESM-IF1` — ``ESM-IF1`` – Structure-conditioned sequence model
- `TemBERTure Regression` — ``TemBERTure Regression`` – Sequence-only melting temperature regressor

---

### `tcrbuilder2` — TCRBuilder2 API

**Description:** TCRBuilder2 is a deep learning model for predicting paired T-cell receptor (TCR) α/β variable domain 3D structures directly from amino acid sequences. It is trained on TCR-specific structural data and predicts backbone and side-chain coordinates f...

**Domain:** TCR, structure-prediction

**Actions:** predict

**Output formats:** CIF, PDB, RMSD (float), embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `pdb`

**Best for:**
- Rapid in silico prediction of paired TCR (α/β) variable-domain structures for engineered T-cell therapies, enabling s...
- Structure-guided analysis of TCR–peptide–MHC binding regions by inspecting CDR loop conformations and predicted chemi...
- High-throughput structural characterization of TCR repertoires derived from next-generation sequencing by converting ...

**Related/alternatives:**
- `TCRBuilder2+` — ``TCRBuilder2+`` – Updated TCRBuilder2 weights with improved accuracy for TCR α/β structure predi...
- `ImmuneFold TCR` — ``ImmuneFold TCR`` – Alternate TCR-focused structure predictor, useful for cross-checking and ben...
- `NanoBodyBuilder2` — ``NanoBodyBuilder2`` – ImmuneBuilder nanobody model
- `ABodyBuilder2` — ``ABodyBuilder2`` – ImmuneBuilder antibody model for paired VH/VL structures, enabling comparativ...

---

### `tcrbuilder2-plus` — TCRBuilder2+ API

**Description:** TCRBuilder2+ is a GPU-accelerated deep learning model specialized for rapidly predicting accurate 3D structures of T-cell receptor (TCR) variable domains from amino acid sequences. Optimized specifically for TCR modeling, it predicts backbone conf...

**Domain:** TCR, structure-prediction, structural-comparison

**Actions:** predict

**Output formats:** CIF, PDB, RMSD (float), embedding, pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `pdb`

**Best for:**
- Predicting T-cell receptor (TCR) structural conformations to accelerate therapeutic candidate selection, enabling rap...
- Structural modeling of TCR-antigen interactions to guide rational design and affinity maturation, providing detailed ...
- High-throughput structural screening of large-scale TCR sequence datasets from next-generation sequencing (NGS), enab...

**Related/alternatives:**
- `TCRBuilder2` — ``TCRBuilder2`` – Provides rapid, accurate structural predictions for T-cell receptors, serving a...
- `ImmuneFold TCR` — ``ImmuneFold TCR`` – Complements ``TCRBuilder2+`` by leveraging a different deep-learning archite...
- `NanoBodyBuilder2` — ``NanoBodyBuilder2`` – Offers specialized deep-learning models for nanobody structure prediction,...
- `ABodyBuilder3 pLDDT` — ``ABodyBuilder3 pLDDT`` – Predicts antibody structures with residue-level confidence scores, comp...

---

### `temberture-classifier` — TemBERTure Classifier API

**Description:** TemBERTure Classifier (TemBERTureCLS) predicts protein thermostability class (thermophilic >60°C vs non-thermophilic) from primary amino acid sequence using a protBERT-BFD backbone with Pfeiffer adapter fine-tuning. The API supports GPU-accelerate...

**Domain:** thermostability

**Actions:** predict, encode

**Output formats:** CIF, Tm (float), embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `sequence_index, embeddings, per_residue_embeddings, cls_embeddings`
  - **predict**: `prediction, classification`

**Best for:**
- High-throughput triage of enzyme variant libraries for hot-process biocatalysis: use TemBERTureCLS via the ``predicto...
- Genome and metagenome mining for thermostable homolog discovery: apply the classifier score across UniProt/NCBI/metag...
- Design-loop guidance in enzyme engineering pipelines: integrate the TemBERTureCLS class score as a lightweight object...

**Related/alternatives:**
- `TemBERTure Regression` — ``TemBERTure Regression`` – Predicts melting temperature (Tm) from sequence
- `TEMPRO 3B` — ``TEMPRO 3B`` – Alternative thermostability predictor based on protein language models
- `ESM-2 650M` — ``ESM-2 650M`` – Provides high-quality sequence embeddings
- `ESMFold` — ``ESMFold`` – Predicts 3D structure

---

### `temberture-regression` — TemBERTure Regression API

**Description:** TemBERTure Regression (TemBERTureTm) predicts protein melting temperature (°C) from amino acid sequence using a protBERT-BFD backbone with adapter-based fine-tuning, exposed via classifier/regression prediction and embedding encoder endpoints. The...

**Domain:** thermostability

**Actions:** predict, encode

**Output formats:** CIF, Tm (float), embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `sequence_index, embeddings, per_residue_embeddings, cls_embeddings`
  - **predict**: `prediction, classification`

**Best for:**
- High-throughput triage of enzyme variant libraries for thermostability
- Use predicted melting temperature (Tm) from sequence to rank and down-select large mutational or recombination librar...
- Valuable for industrial biocatalysts (e.g., hydrolases for detergents, lignocellulose-degrading enzymes for biomass p...

**Related/alternatives:**
- `TemBERTure Classifier` — ``TemBERTure Classifier`` – Predicts thermophilic/non-thermophilic class from sequence
- `TEMPRO 3B` — ``TEMPRO 3B`` – Independent thermostability/Tm regressor
- `ESM-1v` — ``ESM-1v`` – Variant effect model for single/multi-point mutations
- `ESMFold` — ``ESMFold`` – Structure prediction from sequence

---

### `tempro-3b` — TEMPRO 3B API

**Description:** TEMPRO 3B predicts nanobody (VHH) melting temperature (Tm, °C) directly from amino acid sequence using ESM-2 t36_3B UR50D per-residue embeddings (2560-d) and a deep neural network regressor trained on 567 nanobodies. The service performs single or...

**Domain:** nanobody, thermostability, stability

**Actions:** predict

**Output formats:** CIF, Tm (float), embedding, pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `tm`

**Best for:**
- High-throughput triage of nanobody libraries from display or AI generation: run TEMPRO 3B on thousands to millions of...
- Variant ranking for thermostabilization campaigns: enumerate targeted point mutations or small combinatorial sets (fo...
- Developability gating for therapeutic and diagnostic nanobodies: apply predicted Tm as an early developability filter...

**Related/alternatives:**
- `TEMPRO 650M` — ``TEMPRO 650M`` – Lightweight TEMPRO variant using smaller ESM-2 embeddings
- `NanoBodyBuilder2` — ``NanoBodyBuilder2`` – Builds VHH structures and numbering from sequences
- `Peptides` — ``Peptides`` – Computes sequence-level physicochemical descriptors (e.g., aliphatic index, GRAVY,...

---

### `tempro-650m` — TEMPRO 650M API

**Description:** TEMPRO 650M predicts nanobody (VHH/sdAb) melting temperature (Tm, °C) directly from amino acid sequence using ESM-2 t33_650M embeddings (≈650M parameters, ~2.4 GB) and a DNN regressor trained on 567 curated sequences (NbThermo plus internal, 80/20...

**Domain:** nanobody, thermostability, stability

**Actions:** predict

**Output formats:** Tm (float), embedding, pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `tm`

**Best for:**
- High-throughput triage of VHH sequence libraries before expression: use TEMPRO 650M to rapidly score large VHH/nanobo...
- Stability gating during affinity maturation and humanization: rank-order proposed VHH variants by predicted Tm to mai...
- Developability risk assessment prior to scale-up and formulation: combine predicted Tm from TEMPRO 650M with orthogon...

**Related/alternatives:**
- `TEMPRO 3B` — ``TEMPRO 3B`` – Higher‑capacity TEMPRO variant using ESM‑2 3B embeddings
- `ESM-2 650M` — ``ESM-2 650M`` – Provides the sequence embeddings TEMPRO 650M is trained on
- `Peptides` — ``Peptides`` – Computes physicochemical descriptors (aliphatic index, GRAVY, charge, etc.) used i...
- `ABodyBuilder3 pLDDT` — ``ABodyBuilder3 pLDDT`` – Antibody‑focused structure prediction with regional pLDDT

---

### `thermompnn` — ThermoMPNN API

**Description:** ThermoMPNN is a structure-based ΔΔG predictor for single amino acid substitutions that uses ProteinMPNN graph-neural embeddings and a lightweight attention/MLP head to estimate stability changes from a wild-type 3D backbone, without modeling mutan...

**Domain:** thermostability, stability

**Actions:** predict

**Output formats:** CIF, PDB, embedding, log-probability, pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `mutation, position, wildtype, mutation_aa, ddg`

**Best for:**
- Rapid in silico triaging of protein point mutations for thermodynamic stability using a single wild-type structure (e...
- Stability-aware lead optimization for therapeutic proteins such as Fc-fusion constructs, cytokines, and growth factor...
- Design and ranking of more thermostable variants for industrial and synthetic-biology enzymes (e.g., hydrolases, oxid...

**Related/alternatives:**
- `ProteinMPNN` — ``ProteinMPNN`` – Parent sequence-design GNN whose structural embeddings ThermoMPNN uses via tran...
- `ThermoMPNN-D` — ``ThermoMPNN-D`` – ThermoMPNN-based model for double mutants (and related variants)
- `ESM2StabP` — ``ESM2StabP`` – Sequence-only stability predictor using ESM-2 embeddings
- `AlphaFold2` — ``AlphaFold2`` – Structure prediction model for proteins without experimental structures

---

### `thermompnn-d` — ThermoMPNN-D API

**Description:** ThermoMPNN-D is a structure-based neural network for predicting protein stability changes (ΔΔG, kcal/mol) for single and double point mutations from input PDB structures. It extends ThermoMPNN with a Siamese architecture using ProteinMPNN node, ed...

**Domain:** thermostability, stability

**Actions:** predict

**Output formats:** CIF, PDB, embedding, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **predict**: `mutation, position, position1, position2, wildtype, wildtype1, wildtype2, mutation_aa, mutation_aa1, mutation_aa2, ddg, distance`

**Best for:**
- Prioritizing double-mutant stability scans in industrial protein engineering campaigns (for example, thermostabilizin...
- Designing stabilizing mutation pairs for developability optimization of recombinant protein therapeutics (such as Fc-...
- Engineering robust double-mutant backbones for high-temperature or harsh-process biocatalysts in industrial bioproces...

**Related/alternatives:**
- `ThermoMPNN` — ``ThermoMPNN`` – Single-mutation DDG predictor using the same ProteinMPNN backbone and feature space
- `ESM2StabP` — ``ESM2StabP`` – Sequence-only protein stability predictor that complements structure-based ``Ther...
- `ProteinMPNN` — ``ProteinMPNN`` – Structure-conditioned sequence-design GNN that supplies the residue and edge em...
- `ESM-IF1` — ``ESM-IF1`` – Inverse folding model for sampling alternative local sequences around mutation site...

---

### `zymctrl` — ZymCTRL API

**Description:** ZymCTRL is a conditional protein language model for controllable enzyme sequence generation, trained as a 738M-parameter GPT2/CTRL-style Transformer decoder on ~36M BRENDA enzymes. It generates amino acid sequences up to 1,024 residues conditioned...

**Domain:** enzyme

**Actions:** encode, generate

**Output formats:** CIF, embedding, log-probability, pLDDT, sequence

**Response fields per action** (at `results[*]` unless noted):
  - **encode**: `sequence_index, embedding, per_token_embeddings`
  - **generate**: `sequence, perplexity`

**Best for:**
- Rapid in silico generation of enzyme variant libraries conditioned on EC number for industrial biocatalyst developmen...
- Lead-finding for new enzymatic routes in process development, where process chemists and protein engineers need novel...
- Enzyme portfolio expansion around underrepresented or poorly characterized EC classes, where only tens of natural seq...

**Related/alternatives:**
- `ProGen2 Large` — ``ProGen2 Large`` – General-purpose protein LM for conditional generation by family or function
- `ProteinMPNN` — ``ProteinMPNN`` – Structure-conditioned sequence design
- `ESMFold` — ``ESMFold`` – Fast structure prediction from sequence
- `ProteInfer` — ``ProteInfer`` – Functional annotation from sequence

---
