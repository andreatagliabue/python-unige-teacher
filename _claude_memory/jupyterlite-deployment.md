---
name: jupyterlite-deployment
description: How and where the Python course is published (JupyterLite + GitHub Pages)
metadata: 
  node_type: memory
  type: project
  originSessionId: fd352b6a-67e4-4a13-b026-42a1fcc9eee6
---

The course is delivered as a **JupyterLite** static site, published on **GitHub Pages**.

There are **two deployments**:
- **Student site** — `https://andreatagliabue.github.io/python-unige/` — repo `python-unige`, **all 5 lessons** + data visible (published 2026-06-21). L5 is split into 3 independent notebooks: `05a_curve_fitting`, `05b_statistics`, `05c_automation`. Send this link to students.
- **Teacher site** — `https://andreatagliabue.github.io/python-unige-teacher/` — repo `python-unige-teacher`, all 5 lessons in `course_site/content/`. For instructor use only.

Both repos share the same workflow and requirements. The teacher repo has a `teacher` remote configured locally (`git remote add teacher https://github.com/andreatagliabue/python-unige-teacher.git`). To publish a new lesson to students: copy the notebook into `course_site/content/` on the `python-unige` repo and push to `origin main`.

- **GitHub repo (student):** https://github.com/andreatagliabue/python-unige
- **GitHub repo (teacher):** https://github.com/andreatagliabue/python-unige-teacher
- **Direct lesson-1 link:** `.../lab/index.html?path=01_introduction.ipynb` (use the `lab/` app, not `notebooks/` — see note below).

Setup (in the local project at `/home/andreatagliabue/PROJECTS/python_course`):
- `course_site/content/` holds the bundled materials (notebooks + `data/`) — students get them preloaded, no upload needed.
- **ONLY `course_site/content/` is published.** The build runs `jupyter lite build --contents course_site/content`, so a notebook goes live ONLY if a copy sits in `course_site/content/`. Source notebooks kept in lesson folders (`01_theoretical_introduction/notebooks/`, `02_python_basics/`, …) are **NOT** deployed on their own — they must be copied into `course_site/content/` first. Live copies (all 5 lessons, names identical to source): `course_site/content/{01_introduction,02_python_basics,03_data_io_and_tables,04_plotting_and_analysis,05_fitting_stats_automation}.ipynb` + `course_site/content/data/`. The lesson notebooks are thus duplicated (source in `0X_*/` + published copy in `content/`) — keep both in sync, or later refactor the workflow to copy at build time.
- `.github/workflows/deploy.yml` builds the site (`jupyter lite build --contents course_site/content`) and deploys to Pages on every push to `main`.
- `requirements.txt` pins the build deps (jupyterlite-core, jupyterlite-pyodide-kernel, jupyter-server).
- `course_site/dist/` is the local build artifact — git-ignored; the Action rebuilds it.

To update the course: edit/add notebooks in `course_site/content/`, commit, push → site redeploys automatically.

Verified working end-to-end in a headless browser (Pyodide ran the notebook, `read_csv` of the 600-row CSV succeeded, plots rendered, no errors).

**Troubleshooting "the new notebook isn't showing / direct link errors":** this is almost always the student's/your **browser cache** (JupyterLite service worker + IndexedDB serve the previous file listing), NOT a deploy failure. Verify the server side first with `curl` (no auth needed, public repo):
- run status: `https://api.github.com/repos/andreatagliabue/python-unige/actions/runs?per_page=5` → check `conclusion == success`;
- file served: `curl -o /dev/null -w "%{http_code}" <base>/files/<notebook>.ipynb` → expect `200`;
- in the published listing: `curl <base>/jupyterlite/api/contents/all.json` (path may be `<base>/api/contents/all.json`) → the notebook name should appear in `content`.
If all green, tell them to open in a **private window** or clear site data (Application/Storage → Clear). Direct-link format: `<base>/lab/index.html?path=<notebook>.ipynb` — use the `lab/` app, NOT `notebooks/`: the `notebooks/` single-document view has no file-browser sidebar, so a direct link there hides the `data/` folder ("where's the data?"); `lab/` shows notebooks + data, like the course home. (`gh` CLI is NOT installed here — use the GitHub REST API via curl.)

Persistence behaviour (verified in a headless browser): student edits are saved in the browser (IndexedDB) and **survive a plain page reload** — a reload resets only the running/kernel state, NOT the file content. A fresh/empty storage shows the original bundled files. To restore the original course material after editing, the student must clear the site's browser data (or use a file they never touched). The student-guide troubleshooting section was corrected to reflect this.

Git auth: HTTPS push uses a Personal Access Token saved in `~/.git-credentials` (`credential.helper store`); pushes work non-interactively. The session is non-interactive, so git cannot prompt — credentials must be pre-stored, not entered at a prompt.

Student/instructor guides: `STUDENT_GUIDE_JupyterLite.{md,pdf}`, `INSTRUCTOR_GUIDE_JupyterLite.{md,pdf}` (committed in the repo). The PDFs are generated from the `.md` files with WeasyPrint + Python-Markdown (color emojis stripped for the PDF); regenerate after editing the markdown.

See [[python-course-plan]].
