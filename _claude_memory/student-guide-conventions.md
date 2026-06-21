---
name: student-guide-conventions
description: Current conventions and design decisions in STUDENT_GUIDE_JupyterLite.md
metadata: 
  node_type: memory
  type: project
  originSessionId: a0d52734-0dcc-45e9-9065-8f4786caa972
---

The student guide (`STUDENT_GUIDE_JupyterLite.md` + `.pdf`, committed in `python-unige` repo) reflects these design decisions as of 2026-06-12:

- **Incognito mode is recommended** (reversed from original). Students are told to always open the course in a Private/Incognito window. Reason: guarantees a clean environment every session, no stale cache or leftover notebook state.
- **To reset**: close ALL incognito windows (not just the tab — the whole incognito session), then reopen. This is the documented reset procedure.
- **No internet connection requirement** listed. Removed because JupyterLite runs locally in the browser after first load (Pyodide is cached).
- **Language around available lessons**: as of 2026-06-21 ALL lessons are published on the student site simultaneously (L1–L4 + 05_overview + 05a/05b/05c). If the student guide still says "current lesson" that wording is now stale — update it if regenerating the guide.
- PDF regenerated with WeasyPrint + Python-Markdown (same script as always — see [[lesson1-speaker-notes]] for the pdf generation pattern).

See [[jupyterlite-deployment]] for the two-site setup (student vs teacher).
