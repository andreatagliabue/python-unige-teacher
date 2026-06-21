---
name: python-course-plan
description: "Structure and goals of the \"Python for Dummies\" course for a Materials Science PhD program"
metadata: 
  node_type: memory
  type: project
  originSessionId: fd352b6a-67e4-4a13-b026-42a1fcc9eee6
---

The user (Andrea Tagliabue, Politecnico di Torino) is planning a short, beginner-level Python course for a **Materials Science PhD program**.

Course structure:
- **1 introductory lecture** of 2 hours (theory).
- **4 hands-on lessons** of 3-4 hours each, where students work on notebooks.
- Even the theoretical/introductory lecture is delivered on an online notebook.

Constraints / decisions:
- **Level: basic / introductory** ("Python for dummies") — audience is not assumed to be familiar with programming or Linux/Ubuntu.
- **Platform: JupyterLite** (runs in the browser) — chosen to avoid setup headaches for students unfamiliar with Ubuntu/local environments.
- **All course material must be in English.**
- **No accounts / no login for students.** The local-browser save model (progress kept per-browser, no cloud sync; students download `.ipynb` to keep work) is accepted — do NOT propose JupyterHub/cloud-save unless the user asks. Confirmed 2026-06-08.

**Source AND deployed notebook names are uniform and identical (fixed 2026-06-09):** source is `0N_<topic>/notebooks/0N_<name>.ipynb` (+ a `data/` sibling when it has data); the deployed copy in `course_site/content/` has the EXACT same filename. Canonical names: `01_introduction`, `02_python_basics`, `03_data_io_and_tables`, `04_plotting_and_analysis`, and for L5 three independent notebooks `05a_curve_fitting`, `05b_statistics`, `05c_automation` + a guide `05_overview.md`. The `0N_` prefix gives correct alphabetical order in the JupyterLite file browser.

Progress (as of 2026-06-11):
- **Lesson 1 (theoretical intro) is DONE and deployed.** Source: `01_theoretical_introduction/notebooks/01_introduction.ipynb`; live as `01_introduction.ipynb`. Synthetic tensile-test dataset; variables, types, lists, if, for, functions, NumPy/pandas/matplotlib, a read→compute→plot→save demo. See [[jupyterlite-deployment]].
- **Lesson 1 was restructured/polished (2026-06-11 session).** Current section order: 0 roadmap, 1 *What is Python?* (added — "a lab notebook that can compute"), 2 Why Python, 3 Jupyter Notebook (moved before the dataset), 4 scientific workflow + an inline **matplotlib tensile-test schematic** (specimen + stress-strain curve, labels match the CSV columns), 5 the data files + raw CSV shown as a plain-text block of the real `tensile_raw.csv` rows, 6 basics (subsection titles carry a purpose hint: `6.1 Variables → naming`, `6.5 For loops → automation`, …), 7 libraries (opens with a "what is a *library*?" lab-instrument analogy before NumPy/pandas/matplotlib, each subsection expanded), 8 mini demo, 9 errors, **10 Bonus: animated plots**. Instructor/speaker notes were reframed as audience-facing callouts (**Key concept / Why it matters / Tip / Going further**). See [[feedback-course-pedagogy]].
- **Lesson 1's "Section 10 — Bonus" gallery of ~12 plots, confirmed working & loved by Andrea:** 10.1 diameter-sweep + 10.2 test-scrub (tensile, matplotlib animations); then a mini-gallery — atoms melting, diffusion/Brownian, XRD, atoms-on-springs, travelling wave, Fourier→square wave, Monte-Carlo π, animated heat-diffusion heatmap, a random chaotic double pendulum, and an interactive Plotly 3D free-energy surface (drag to rotate). All built on the techniques in [[jupyterlite-gotchas]] (matplotlib `to_jshtml`; Plotly via mime bundle; robust `load_csv`; self-contained cells).
- **Lesson 2 (first hands-on) created — 2026-06-08.** Source: `02_python_basics/notebooks/02_python_basics.ipynb`; live as `02_python_basics.ipynb`. Covers variables, arithmetic, comparisons/boolean, `if`/`elif`, data types, lists, `for`, functions, dictionaries, basic plotting, a mini-project, and an optional S01-vs-S03 comparison. Coherent with the lesson-1 dataset (see [[tensile-dataset-coherence]]); teaches comments, `import` + `library.function()` notation, `round()`/f-strings, negative indexing, docstrings + `help()`. Committed to `main` and **published** (copied into `course_site/content/`, so it is live) — see the deploy rule in [[jupyterlite-deployment]].
- **Lesson 3 (second hands-on) created & published — 2026-06-09.** Source: `03_io_tables/notebooks/03_data_io_and_tables.ipynb`; live as `03_data_io_and_tables.ipynb`. Topic: file I/O and tabular data with pandas — `read → inspect → calculate → check → clean → summarize → save`. Covers paths, `read_csv`, inspection (`head`/`info`/`describe`/`dtypes`), column selection, row filtering (boolean masks, `&`/`|`/`~`), calculated columns, data-quality checks, a real cleaning step (`df_clean`), `groupby`/`agg` by sample & treatment, `to_csv`, a quick plot, a mini-project (with starter scaffold), plus optional hardness + JSON-metadata extensions. Data is coherent with lessons 1–2 but uses **distinct filenames** to avoid clobbering shared files — see [[tensile-dataset-coherence]].
- **Lesson 4 (third hands-on) created & published — 2026-06-09.** Source: `04_plotting_analysis/notebooks/04_plotting_and_analysis.ipynb`; live as `04_plotting_and_analysis.ipynb`. Topic: plotting + data analysis. Line/scatter/histogram/bar-with-error-bars/box plots, one-vs-many samples, summary metrics per sample (`idxmax`), a dedicated **fitting** section (Young's modulus via `np.polyfit` on the elastic region, fitted line drawn over the data), save-figure, mini-project, optional `fig, ax` OO style. Reuses the canonical deployed data (`tensile_raw.csv` for curves/scatter/fit; `hardness_positions.csv` — which has replication per treatment — for hist/box/error-bars). Andrea's brief: lesson is about plotting/analysis so some sections need not fit the narrative, but it must include **at least one fit**. Andrea uploaded the draft as a synthetic-data generator with wrong treatments/diameters/HV + a `Path.exists()` JupyterLite crash + a hardness schema mismatch — all rewritten/fixed. Verified with `nbconvert --execute` (0 errors).
- **Lesson 5 (final hands-on) split into three independent notebooks — 2026-06-21.** Originally one monolithic file; split so students can pick whichever topic interests them. Sources in `05_final/notebooks/`; all three live on the student site. Each notebook is self-contained (re-imports, reloads data), has a single H1, ends with the course recap. The old `05_fitting_stats_automation.ipynb` is retired.
  - **05a_curve_fitting.ipynb** — nonlinear Voce fit with `scipy.optimize.curve_fit`, params±uncertainty, R²=0.95 on S03 hardening region.
  - **05b_statistics.ipynb** — std vs SEM, error bars, Welch `ttest_ind` (quenched vs annealed, p=3.9e-13).
  - **05c_automation.ipynb** — build `runs/` at runtime, `glob`→loop→`analyse_tensile_file()`→`to_csv`. No scipy.
- Setup decisions for L5: `scipy` added at runtime via `%pip install -q scipy` (in 05a and 05b; NOT 05c). `runs/` built on the fly; gitignored along with `automated_tensile_summary.csv`.

See [[user-andrea-tagliabue]], [[jupyterlite-deployment]] and [[feedback-course-pedagogy]].
