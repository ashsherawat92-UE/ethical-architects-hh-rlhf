# Lexical patterns and measurement sensitivity in AI preference data

Analysis code and results for **Project 14** of the PW26 elective *AI, Power & Responsibility: Governing Intelligence* — Team Ethical Architects (Agathiyan Akilan, Ashwani Sherawat).

**Target journal:** Ethics and Information Technology (Springer Nature)
**Anchor paper:** Dahlgren Lindström et al. (2025), *Helpful, harmless, honest? Sociotechnical limits of AI alignment and safety through RLHF*, [10.1007/s10676-025-09837-2](https://doi.org/10.1007/s10676-025-09837-2). Closest dataset audit: Chehbouni et al. (2025), *Beyond the Safety Bundle*.
**Dataset:** [Anthropic/hh-rlhf](https://huggingface.co/datasets/Anthropic/hh-rlhf) (MIT), `helpful-base` and `harmless-base` test splits, pinned revision `d8d97fe9e96fcd19cc8fc31bd04ee393422abfbb`

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.24090950.svg)](https://doi.org/10.5281/zenodo.24090950)

## What the study measures

Every pair in HH-RLHF holds two AI-generated replies to the same conversation and a human annotator's record of which one was preferred. We measure literal expression patterns in the chosen and rejected replies and test how sensitive those measurements are to analytic choices. A dictionary hit cannot establish factual accuracy, ethical adequacy, stakeholder protection, or why an annotator preferred a reply; the preference label is the object of study, not a standard.

## Research questions and outputs

Sixteen registered outputs, four per research question. Every output has a CSV table in `results/tables/`; nine also have a figure in `results/figures/` (PDF + PNG). `results/RESULTS_README.txt` and `results/run_manifest.json` describe each one.

| RQ | Question | Outputs |
|---|---|---|
| **RQ1** Response expressions | How do chosen and rejected replies differ in the occurrence of four literal response-expression families (refusal, assistance, redirection, safety), in their within-reply co-occurrence, and in the number of families present? | `RQ1_01` occurrence (fig) · `RQ1_02` co-occurrence (fig) · `RQ1_03` expression contributions · `RQ1_04` breadth transitions (fig) |
| **RQ2** Uncertainty and assertion wording | Do chosen and rejected replies differ in the density of uncertainty and assertion expressions per 100 tokens, overall, by pair length, and as presence/absence? | `RQ2_01` paired densities · `RQ2_02` density by length group (fig) · `RQ2_03` presence patterns · `RQ2_04` rate distributions (fig) |
| **RQ3** Explicit references to others | How often do replies explicitly refer to the requester, other persons, or collectives, how do these references combine, and how do they differ when the shared context already mentions others? | `RQ3_01` reference occurrence (fig) · `RQ3_02` reference combinations · `RQ3_03` differences within lexical contexts (fig) · `RQ3_04` individual expressions |
| **RQ4** Measurement sensitivity | How stable are the RQ1–RQ3 estimates under an expanded dictionary, relaxed prior-turn eligibility, alternative context representatives, and response-length restrictions? | `RQ4_01` dictionary sensitivity (fig) · `RQ4_02` structural eligibility · `RQ4_03` length restrictions (fig) · `RQ4_04` complete stability summary |

Statistics: paired differences in percentage points or matches per 100 tokens, pointwise 95% pair-bootstrap intervals (2,000 replicates, seed 20261026), exact McNemar tests with Holm correction within `RQ1_01`, `RQ1_02`, `RQ2_03` and `RQ3_01`. Hypothesis tests are exploratory. Undefined groups are reported as unestimable rather than imputed.

## Repository layout

```
notebooks/
  Ethical_Architects_notebook.ipynb        executed notebook: M1 configure → M2 verify → M3 clean → M4 dictionaries
                                           → M5 cohorts → M6 statistics → RQ1–RQ4 (16 outputs) → M7 package
  archive/                                 earlier versions (v2.0 of 9 Nov 2026)
results/
  RESULTS_README.txt                       key to every output
  run_manifest.json                        run id, code version, configuration hash, cohort sizes, file hashes
  figures/                                 RQ1_01 … RQ4_03 as PDF + PNG
  tables/                                  RQ1_01 … RQ4_04 as CSV; diagnostics/ holds cleaning_flow, context_overlap, denominators
  diagnostics/                             environment.json, experiment_config.json (seed, dictionaries, tokeniser), source_provenance.json
  archive/                                 zipped outputs of earlier runs
docs/
  proposal/                                finalized proposal; Submission-2 version with expected results
  presentations/                           initial and current presentations
  paper/                                   Overleaf export (LaTeX source and PDF)
  ChatGPT_Link_Confirming_Code_Execution.txt
  Github_Zenodo_Links.txt
target_journal.txt                         "Ethics and Information Technology"
requirements.txt · LICENSE (MIT) · CITATION.cff · .zenodo.json
```

Data files are not stored in this repository.

## Data

This project uses the `helpful-base` and `harmless-base` **test** splits of [Anthropic HH-RLHF](https://huggingface.co/datasets/Anthropic/hh-rlhf), pinned to revision `d8d97fe9e96fcd19cc8fc31bd04ee393422abfbb`.

### Download the test files

- [helpful-base/test.jsonl.gz](https://huggingface.co/datasets/Anthropic/hh-rlhf/resolve/d8d97fe9e96fcd19cc8fc31bd04ee393422abfbb/helpful-base/test.jsonl.gz) → save as `data/helpful-base/test.jsonl.gz`
- [harmless-base/test.jsonl.gz](https://huggingface.co/datasets/Anthropic/hh-rlhf/resolve/d8d97fe9e96fcd19cc8fc31bd04ee393422abfbb/harmless-base/test.jsonl.gz) → save as `data/harmless-base/test.jsonl.gz`

### Verify the files

From the repository root:

```sh
gzip -dc data/helpful-base/test.jsonl.gz | sha256sum
gzip -dc data/harmless-base/test.jsonl.gz | sha256sum
```

The hashes apply to the decompressed JSONL contents.

| Test split | SHA-256 |
|---|---|
| helpful-base | `41c3a32fe45a7075f7e32b11bd29383f9fe650287354a95568f61295a8e694da` |
| harmless-base | `14d765196c9f18d84f9bb3a78bac608c8f2915110ebcbd74ec95db7b7198b008` |

### Expected preprocessing counts

| Test split | Raw pairs | Pairs after cleaning |
|---|---|---|
| helpful-base | 2,354 | 2,333 |
| harmless-base | 2,312 | 2,297 |
| **Total** | **4,666** | **4,630** |

Each retained pair contains the chosen and rejected final assistant replies for the same conversation context. Pairs are excluded when the two transcripts differ before the final reply, when a final reply is empty, when the replies are identical, or when the same context appears more than once (one representative is kept). Density comparisons use 4,625 pairs after five zero-token exclusions.

## How to run

### Kaggle
1. New notebook → File → Import → `notebooks/Ethical_Architects_notebook.ipynb`.
2. Settings → **Internet: On** (the notebook downloads and hash-verifies the two test files), or attach the verified files as a Kaggle dataset. No GPU needed.
3. **Run All**. M7 checks that all sixteen outputs exist and writes a results ZIP under the run folder printed by the last cell.

### Local
```sh
pip install -r requirements.txt
jupyter notebook notebooks/Ethical_Architects_notebook.ipynb
```
Place the data files under `data/` as described above, then run all cells.

## What the results do and do not show

The analysis is a transparent, reproducible measurement of literal expression patterns on released text. It uses no human coding files, no external claim verdicts and no AI judge. It does not establish the factual accuracy or ethical adequacy of any reply, does not identify a primary stakeholder, and makes no claim about annotator intentions. Results describe one annotator pool under one instruction set in 2022 and say nothing about current models or current RLHF practice. Inference beyond the analysed pairs requires exchangeable contexts.

## Citation

See `CITATION.cff`, or cite the Zenodo record: [10.5281/zenodo.24090950](https://doi.org/10.5281/zenodo.24090950).
Dataset: Bai et al. (2022), *Training a Helpful and Harmless Assistant with Reinforcement Learning from Human Feedback*, arXiv:2204.05862.
