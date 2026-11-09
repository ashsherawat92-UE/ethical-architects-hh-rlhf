# Submission checklist — "technically complete and publication ready"

The professor asked for four items. Do them in this order; items 2 and 1 depend on the Kaggle run.

---

## 0. Official final run on Kaggle (30 min, mostly waiting)

1. kaggle.com → New Notebook → File → Import → `Ethical_Architects_Final_Implementation.ipynb`.
2. Right panel → Settings → Internet **On**. Accelerator: None.
3. **Save Version → Save & Run All (Commit)**. Wait for the run to finish (~10 min on the test split).
4. Open the committed version → Output tab → download `final_results.zip` from `ethical_architects_dev/run_<timestamp>/`.
5. Rename it `final_results_test_split_<YYYY-MM-DD>.zip` and put it in `results/`.
6. From the committed version, File → Download notebook. This copy has the executed outputs inside. Replace `Ethical_Architects_Final_Implementation.ipynb` in the repo with it.

If Kaggle's internet fetch fails: download the two test files from the links in the notebook header, upload them as a Kaggle dataset, attach it, rerun. The loader matches them by hash.

---

## 2. GitHub repository + Zenodo DOI (20 min)

### GitHub
1. Create a **public** repository, e.g. `ethical-architects-hh-rlhf`. Do not initialise with a README.
2. Unzip this package locally, then:
   ```bash
   cd ethical-architects-hh-rlhf
   git init && git add . && git commit -m "Final implementation v2.0 with executed run and results"
   git branch -M main
   git remote add origin https://github.com/<USER>/ethical-architects-hh-rlhf.git
   git push -u origin main
   ```
3. Check that no `*_KEY.csv`, no `private/` folder and no `.jsonl` data file was pushed (the `.gitignore` blocks them, but look).
4. In `CITATION.cff`, replace `<ORG-OR-USER>` with the real GitHub path. Commit.

### Zenodo
1. zenodo.org → Log in with GitHub → top-right menu → **GitHub**.
2. Flip the switch **On** next to `ethical-architects-hh-rlhf`.
3. Back on GitHub: Releases → **Draft a new release** → tag `v2.0`, title `Final implementation v2.0 — executed run and results` → Publish.
4. Zenodo picks the release up within a minute and mints a DOI (metadata comes from `.zenodo.json`).
5. Copy the DOI badge markdown from the Zenodo record into `README.md` (replace the `XXXXXXX` placeholder). Commit — this does not need a new release.

Submit: the GitHub URL and the Zenodo DOI URL (`https://doi.org/10.5281/zenodo.<number>`).

---

## 1. ChatGPT confirmation link (15 min)

1. Open a new ChatGPT conversation. Attach three files: the executed notebook, `final_results_test_split_<date>.zip`, and `docs/Ethical_Architects_Proposal.docx`.
2. Paste the prompt below, verbatim.
3. If ChatGPT lists issues, fix them in the notebook, rerun on Kaggle, and ask again in the same conversation until it confirms.
4. Share → Create link. Save the URL in `ChatGPT_Confirmation_Link.txt` and submit it.

**Prompt to paste:**

> You are reviewing a student research project for technical completeness and publication readiness before the write-up phase. Attached are (1) the final executed Jupyter notebook, (2) the zipped results of its final run, and (3) the research proposal the code implements. The target journal is Ethics and Information Technology.
>
> Please check, against the proposal, whether the notebook: implements every data-preparation, sampling, blinding and analysis step the proposal promises; answers each research question (RQ1a, RQ1b, RQ2, RQ3, RQ4) with at least one figure and one table that are actually present in the results zip; reports appropriate statistics (confidence intervals, paired tests with multiple-comparison correction, effect sizes); logs provenance and exclusions reproducibly; separates automated lexicon measures from the planned human coding and provides the validation hook for it; and contains no unresolved errors, dead configuration or missing outputs.
>
> Then state explicitly whether the final executed run and its results are technically complete and publication ready for the write-up phase, and list any remaining issues that must be fixed first. Be specific and refer to cell numbers and file names.

---

## 3. Finalized proposal document

Submit `docs/Ethical_Architects_Proposal.docx` — the document the first draft of the code was generated from. (The Submission-2 version with expected figures is also in `docs/` if the professor wants the one with the 16 expected results.)

---

## 4. Target journal text file

Submit `target_journal.txt` (one line: `Ethics and Information Technology`).

---

## Before you send

- [ ] Kaggle run completed on the **test** split; `diagnostics/analysis_split.json` in the zip says `"test"`.
- [ ] Executed notebook committed (cells show outputs).
- [ ] `results/final_results_test_split_<date>.zip` committed.
- [ ] No keys, no data files in the repo.
- [ ] Zenodo DOI resolves and badge is in the README.
- [ ] ChatGPT link opens without login and ends with an explicit "technically complete and publication ready" statement.
- [ ] Four items posted on Teams: ChatGPT link, GitHub + Zenodo links, proposal docx, target_journal.txt.
