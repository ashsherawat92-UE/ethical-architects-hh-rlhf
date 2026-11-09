# Ethical justification and value tensions in AI preference data

Analysis code and results for **Project 14** of the PW26 elective *AI, Power & Responsibility: Governing Intelligence* — Team Ethical Architects (Agathiyan Akilan, Ashwani Sherawat).

**Target journal:** Ethics and Information Technology (Springer Nature)
**Anchor paper:** Dahlgren Lindström et al. (2025), *Helpful, harmless, honest? Sociotechnical limits of AI alignment and safety through RLHF*, [10.1007/s10676-025-09837-2](https://doi.org/10.1007/s10676-025-09837-2)
**Dataset:** [Anthropic/hh-rlhf](https://huggingface.co/datasets/Anthropic/hh-rlhf) (MIT), `helpful-base` and `harmless-base`, pinned revision `d8d97fe9e96fcd19cc8fc31bd04ee393422abfbb`

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.XXXXXXX.svg)](https://doi.org/10.5281/zenodo.XXXXXXX) ← replace after the first Zenodo release

## Research questions

| RQ | Question | Notebook step | Outputs |
|---|---|---|---|
| RQ1a | How do chosen and rejected replies differ in refusal, partial assistance, redirection and safety information in helpfulness–harmlessness conflict pairs? | 6 | `fig1a_rq1a_strategies.*`, `rq1a_strategies.csv` |
| RQ1b | How adequately are refusals justified — none, generic, or a specific risk named? | 6 | `fig1b_rq1b_justification.*`, `rq1b_justification.csv`, `rq1b_stats.json` |
| RQ2 | In the helpful subset, are hedged replies rejected in favour of confident ones? (Factual support comes from the human fact-check.) | 7 | `fig2_rq2_language.*`, `rq2_language.csv` |
| RQ3 | Who bears the harm (requester / third party / society), and are third parties' interests acknowledged? | 8 | `fig3_rq3_harm_bearer.*`, `rq3_harm_bearer.csv`, `rq3_stakeholders.csv`, `rq3_stats.json` |
| RQ4 | Does the dataset's ranking stay stable, reverse or become unresolved under frozen ethical criteria? | 9 | `fig4_rq4_reranking.*`, `rq4_reranking.csv`, `rq4_sensitivity.csv` |

## Repository layout

```
Ethical_Architects_Final_Implementation.ipynb   single notebook: load → verify → clean → sample → RQ1–RQ4 → validation hook → zip
results/
  final_results_<split>_<date>.zip               full run output (research_outputs/, diagnostics/, annotation_templates/)
  preview/*.png                                  the five figures from the latest run
docs/
  Ethical_Architects_Proposal.docx               finalized proposal used to generate the first draft of the code
  Ethical_Architects_Proposal_Submission2_expected_results.docx   proposal with four expected figures/tables per RQ
  Ethical_Architects_Presentation.pptx           initial presentation
target_journal.txt                               "Ethics and Information Technology"
requirements.txt · LICENSE (MIT) · CITATION.cff · .zenodo.json
```

## How to run

### Kaggle (official run — test split)
1. Create a new notebook and import `Ethical_Architects_Final_Implementation.ipynb`.
2. Settings → **Internet: On**. No GPU needed.
3. **Run All**. The two official test files (2,354 + 2,312 pairs) download automatically and are verified by SHA-256 against the pinned revision.
4. The last cell prints the path of `final_results.zip` under `/kaggle/working/ethical_architects_dev/run_<timestamp>/`. Download it and add it to `results/` in this repository as `final_results_test_split_<date>.zip`.
5. Use **Save & Run All (Commit)** so the saved notebook contains the executed outputs; download that version and replace the notebook in this repository with it.

### Offline / local
If no internet is available, attach the verified data files (test or train, `.jsonl` or `.jsonl.gz`) as notebook input; the loader matches them by content hash. With only train files present the notebook analyses the train split and says so in `diagnostics/analysis_split.json`. Locally: `pip install -r requirements.txt`, then run all cells; outputs go to `./hh_rlhf_outputs/run_<timestamp>/`.

## What the notebook produces

- **Provenance and data flow** — `diagnostics/source_provenance.csv`, `cleaning_flow.csv`, `exclusions.csv` (every excluded pair with its reason).
- **Samples** — `annotation_templates/pilot_40_blinded.csv` and `main_500_blinded.csv` (labels hidden, replies as Response A/B in random order), `double_coding_subset_ids.csv` (25% double-coded). The un-blinding keys are written to a `private/` folder that is never zipped or committed.
- **Figures** — one per research question, PDF + PNG, from the lexicon pass over every cleaned pair.
- **Tables** — CSV per research question with bootstrap 95% CIs on paired differences, exact McNemar tests with Holm correction, odds ratios for "the feature-bearing reply is the chosen one", χ²/Cramér's V, and the six-ordering sensitivity for RQ4.
- **Validation hook** — once `main_500_coded.csv` (and optionally `main_500_coded_coder2.csv`) is attached, step 10 computes Cohen's κ lexicon-vs-human per feature, κ between coders, and the human RQ4 outcome distribution.

## What the automated results do and do not show

The lexicon pass is a transparent, reproducible proxy. It measures phrase patterns in actual replies; it does not establish factual accuracy, ethical adequacy, or the annotators' motives. The paper's claims rest on the blinded, double-coded human sample. Results describe one 2022 annotator pool under one instruction set and say nothing about current models or current RLHF practice.

## Latest run summary (train split, 85,376 cleaned pairs, 9 Nov 2026)

| | Harmless subset | Helpful subset |
|---|---|---|
| Refusal: odds it is the chosen reply | 1.97 (Holm p < 0.001) | 0.47 (p = 0.09) |
| Redirection: odds chosen | 0.85 (p < 0.001) | 1.21 (p = 0.24) |
| Safety information: odds chosen | 0.90 (p = 0.012) | 1.55 (p = 0.013) |
| Specific-reason refusal chosen when both refuse | OR 1.00 [0.29, 3.45], n = 34 | — |
| Hedges per 100 words, chosen vs rejected | — | 1.01 vs 1.11 (Wilcoxon p < 0.001) |
| Third-party acknowledgment: odds chosen | 0.82 (McNemar p = 0.029) | 1.05 (p = 0.86) |
| Ranking under frozen criteria: stable / reversed / unresolved | 9% / 10% / 81% | 25% / 24% / 52% |

## Citation

See `CITATION.cff`. Dataset: Bai et al. (2022), *Training a Helpful and Harmless Assistant with RLHF*, arXiv:2204.05862.
