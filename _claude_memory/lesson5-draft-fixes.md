---
name: lesson5-draft-fixes
description: "Traps fixed when reworking Andrea's Lesson 5 draft (JupyterLite + dataset coherence)"
metadata: 
  node_type: memory
  type: project
  originSessionId: 0427cae0-db4c-4b31-9d5c-059ccd3d86f9
---

Andrea drafted Lesson 5 himself (the draft he uploaded; the reworked, published version is `05_final/notebooks/05_fitting_stats_automation.ipynb`) and asked for a review. The structure was good; the rework fixed concrete traps. Useful as a checklist for reviewing any future student/draft notebook. See [[python-course-plan]], [[jupyterlite-gotchas]], [[tensile-dataset-coherence]].

- **`Path.exists()` everywhere → would crash on Pyodide.** The draft's `find_data_dir`/`load_table` and per-part `Path("../data/...").exists()` ternaries all hit the documented OSError trap. Replaced with the robust try/except + HTTP-fallback `load_table` (see [[jupyterlite-gotchas]]).
- **Draft shipped its OWN generated dataset, incoherent with L1-L4.** Its fallback generator invented treatments (`tempered`) and samples (S01-**S06**) and **Voce-shaped (monotonic) curves**. The canonical data is S01-S05 / annealed-quenched-aged-as_received and the real σ-ε curves **peak at ~70% then neck downward** (S03: 516→360 MPa). Consequence: full-curve Voce on real data gives a visibly wrong fit (R²=0.83). **Fix that Andrea approved ("canonical where it holds, ad-hoc only where forced"): fit Voce only on the hardening region up to the UTS** (`idxmax`) → S03 R²=0.95, σ_s=494±2 MPa (realistic). Canonical data turned out to work in all 3 parts, so no ad-hoc data was needed.
- **`runs/` folder didn't exist.** Part C now builds it at runtime by splitting `tensile_raw.csv` by `sample_id` (framed honestly: "in real research these files already exist; here we make them so the example is self-contained"). This also sidesteps the risk that `glob` won't see HTTP-served files on the Pyodide FS — glob runs over a freshly-written local dir.
- **`hardness_positions.csv` has an `is_valid` column** (bool) the draft ignored; added the filter (matches L4). Valid counts: annealed 11, quenched/aged/as_received 6 each. t-test quenched(168 HV) vs annealed(115 HV) → p=3.9e-13.
- **Course recap was off by one** (called L3 "plotting", L4 "complete workflow"). Corrected to the real L1-L5 map.
