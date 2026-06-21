---
name: tensile-dataset-coherence
description: The tensile dataset is the shared example across all course lessons; sample→treatment mapping must stay coherent
metadata: 
  node_type: memory
  type: project
  originSessionId: 0427cae0-db4c-4b31-9d5c-059ccd3d86f9
---

The whole course uses one shared running example: a tensile test. Raw data live in `01_theoretical_introduction/data/tensile_raw.csv` (~120 force/displacement points per sample, columns: sample_id, treatment, diameter_mm, gauge_length_mm, force_N, displacement_mm), loaded via a `load_csv()` helper. Companion files: `hardness_by_treatment.csv` and `tensile_summary_by_sample.csv` (the summary is *generated* by running the lesson-1 notebook).

Canonical sample → treatment → diameter mapping (any lesson must match this):
- S01 = annealed, 5.02 mm — peak stress ≈ 354 MPa, max strain ≈ 0.285
- S02 = annealed, 4.98 mm
- S03 = quenched, 5.01 mm — peak stress ≈ 516 MPa, max strain ≈ 0.125
- S04 = aged, 5.00 mm
- S05 = as_received, 4.99 mm

**Why:** hands-on notebooks (e.g. `02_python_basics/notebooks/02_python_basics.ipynb`) use short hardcoded lists for beginners, and it's easy to invent inconsistent samples (a real bug fixed: lesson 2 had "S02 quenched, diam 4.8", which contradicts lesson 1).

**How to apply:** for beginner lessons, keep short lists but down-sample real points from `tensile_raw.csv` for the named sample (e.g. ~9 points across the curve) so peak stresses match lesson 1. The natural annealed-vs-quenched comparison is **S01 vs S03**. See [[python-course-plan]] and [[jupyterlite-gotchas]].

**Deploy gotcha — distinct filenames per lesson.** The deploy dumps every lesson's data into ONE flat folder `course_site/content/data/`, so files with the same name collide. Lessons 1 & 2 share `data/tensile_raw.csv` (clean) + `data/hardness_by_treatment.csv` (3 cols, HV ~115–170). Lesson 3 needed a *dirty* tensile file and a *richer* hardness file, so it uses **distinct names**: `tensile_lab_raw.csv` (lesson-1 data + injected NaN/negative rows for the cleaning section) and `hardness_positions.csv` (same HV scale + `position_id`/`is_valid` + one flagged outlier), plus `sample_metadata.json` (canonical map). Rule: when a new lesson needs a variant of a shared dataset, give it a NEW filename rather than overwriting the shared one — keep the per-sample treatment/diameter/HV map canonical.

**Design decision (confirmed by Andrea 2026-06-09):** lesson 3's *main* file is the dirty one (`tensile_lab_raw.csv`), used for the whole `read→check→clean→analyze` pipeline — NOT a clean shared file with the dirt split into a side demo. Rationale: a data-cleaning lesson must run its quality checks on the data it actually analyzes, otherwise the checks "find nothing" and the cleaning is theater. The minor file duplication (same frozen example numbers + 6 glitched cells) is acceptable. We explicitly considered and rejected reusing the shared clean `tensile_raw.csv` for lesson 3's main flow.
