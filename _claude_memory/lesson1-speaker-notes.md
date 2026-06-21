---
name: lesson1-speaker-notes
description: "Cell-by-cell speaker cue cards (.docx + .pdf) for Lesson 1 — what they are, conventions, how to regenerate"
metadata: 
  node_type: memory
  type: project
  originSessionId: 4f35115b-5604-41b5-9f13-882423cd6e59
---

Andrea asked for a **speaker cue-card document** for the Lesson 1 lecture — "il testo da dire cella per cella, nel caso mi inchiodassi" (a safety net if he blanks mid-lecture). Built 2026-06-11. See [[python-course-plan]].

**The artifact** (one entry per notebook cell, in order, all 89 cells of `01_introduction.ipynb`):
- `Lesson1_speaker_notes.docx` + `Lesson1_speaker_notes.pdf` at the repo **root**, and an identical copy in `01_theoretical_introduction/`.
- **Pushed to teacher repo only (2026-06-21)** — committed and pushed to `teacher` remote (`python-unige-teacher`); NOT pushed to `origin` (student site). Local `main` is 1 commit ahead of `origin/main` — intentional. Do NOT push to origin unless Andrea explicitly asks.

**Conventions he settled on (apply if regenerating or doing the same for L2–L5):**
- **Schematic, not prose** — bullets, not paragraphs.
- For **every section/subsection the FIRST bullet is the key concept written as a complete, self-contained sentence** he can read aloud verbatim if stuck. ("non deve essere un raccondo in prosa" + "almeno il concetto chiave deve essere una frase completa e intelligibile".)
- **Bold the leading label** of a bullet only: `Key concept:`, `RUN it:`/`RUN it`, `Punchline:`, `Looking ahead:`, `Practical note:`, `Heads-up:`, `Final message:`. Rest of the text stays normal.
- Code cells are tagged **▶ RUN** (green); text cells tagged TEXT (grey). Tone: this lecture shows *what Python can do*, it is **not** a coding tutorial — never imply students must read the code (see [[feedback-course-pedagogy]]).
- Notes are in **English** (matches the lecture language) even though he writes requests in Italian.

**How to regenerate** (the build script lives at `/tmp/build_notes.py` during the session; re-author from the cell dump if gone):
- `pip install --break-system-packages python-docx` (PEP 668 blocks a plain pip here).
- Generate `.docx` with python-docx; convert to PDF headlessly: `soffice --headless --convert-to pdf --outdir <dir> <file>.docx` (LibreOffice 24.2 is installed; produced a 12-page PDF).
- **Close any open LibreOffice instance of the file before overwriting**, and delete its `.~lock.<file>#`.
- **`pkill -f` self-match trap:** `pkill -f "soffice.*Lesson1"` also matches the running shell's own command line (which contains that string) and kills itself (exit 144). Use the bracket trick: `pkill -f "[s]office.*Lesson1"`.
- Open on screen with `xdg-open <file>` (opens LibreOffice Writer).
