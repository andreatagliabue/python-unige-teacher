---
name: feedback-always-share-link
description: Andrea wants the live course link included at the end of every reply
metadata: 
  node_type: memory
  type: feedback
  originSessionId: fd352b6a-67e4-4a13-b026-42a1fcc9eee6
---

After working on the course, always end the reply with the live course link (and the direct lesson-1 link).

**Why:** Andrea repeatedly asks "dammi link" / "ridammi link" to open and check the deployed site; providing it proactively saves a round-trip.

**How to apply:** Close each substantive reply with the course link, plus the direct link to whichever notebook was being worked on. Pattern: `.../lab/index.html?path=<notebook>.ipynb`.
- Course: https://andreatagliabue.github.io/python-unige/
Deployed notebook names were uniformed to `0N_topic.ipynb` (2026-06-09) — source AND deployed copies now share identical names. **Direct links use the `lab/` app (NOT `notebooks/`)** — fixed 2026-06-09: the `notebooks/` single-document view has no file-browser sidebar, so students opening a direct link couldn't see the `data/` folder and asked "where's the data?"; `lab/` shows the sidebar (notebooks + data), matching the course home. Data is always on the same in-browser drive either way (`pd.read_csv("data/...")` works in both). Direct links:
- Lesson 1 direct: https://andreatagliabue.github.io/python-unige/lab/index.html?path=01_introduction.ipynb
- Lesson 2 direct: https://andreatagliabue.github.io/python-unige/lab/index.html?path=02_python_basics.ipynb
- Lesson 3 direct: https://andreatagliabue.github.io/python-unige/lab/index.html?path=03_data_io_and_tables.ipynb
- Lesson 4 direct: https://andreatagliabue.github.io/python-unige/lab/index.html?path=04_plotting_and_analysis.ipynb
- Lesson 5 overview: https://andreatagliabue.github.io/python-unige/lab/index.html?path=05_overview.md
- Lesson 5a direct: https://andreatagliabue.github.io/python-unige/lab/index.html?path=05a_curve_fitting.ipynb
- Lesson 5b direct: https://andreatagliabue.github.io/python-unige/lab/index.html?path=05b_statistics.ipynb
- Lesson 5c direct: https://andreatagliabue.github.io/python-unige/lab/index.html?path=05c_automation.ipynb
Suggest opening in a private window (Ctrl+Shift+P) to bypass JupyterLite cache. See [[jupyterlite-deployment]].
