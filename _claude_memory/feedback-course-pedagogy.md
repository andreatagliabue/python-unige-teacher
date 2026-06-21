---
name: feedback-course-pedagogy
description: "How Andrea wants course notebooks reviewed and edited — opinionated pedagogy, concepts before use, keep new basics in their own lesson"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 0427cae0-db4c-4b31-9d5c-059ccd3d86f9
---

When working on the Python course notebooks, Andrea wants an **opinionated pedagogical review**, not just a syntax check: explicitly call out what basic concepts are *missing* for absolute beginners and what is *too advanced*, with a recommendation.

**Why:** audience is Materials Science PhD students at basic level; the goal is the right scope, not completeness.

**How to apply:**
- Make sure every concept is introduced *before* the exercise that needs it (e.g. negative indexing `[-1]` taught before the exercise asking for "the last element").
- When a basic concept is missing, add it **in the lesson where it's relevant** — do NOT edit the introductory lecture (`01_theoretical_introduction`) to backfill basics. He confirmed: "non toccherei la lezione introduttiva ma metterei due parole qui".
- Favored "must-have basics" he approved adding to lesson 2: `round()` + f-strings (readable float output), negative indexing, comments `#`, the `library.function()` / `math.pi` dot notation, docstrings + `help()`.
- He's fine leaving slightly-niche-but-harmless items (`//`, `%`, `zip()`) once `zip()` is explained.

**Lower the code-complexity ceiling for absolute beginners (confirmed on lesson 3, 2026-06-09 — "questi sono degli scappati di casa"):**
- **Label plumbing.** Code students don't need to understand (robust loaders, HTTP fallbacks, try/except for browser quirks) gets an explicit "🔧 plumbing — you can skip this; the normal way is just `pd.read_csv(...)`" banner. Don't make them parse infrastructure. But **don't manufacture plumbing labels where there's no infrastructure** — when asked to add them to lesson 2 (2026-06-09) the honest answer was "almost none": it loads no files, so `zip()`/`help()`/f-strings are real content, not plumbing. The only genuine flag was the cosmetic matplotlib `figsize=(7, 4)` kwarg (commented inline as "cosmetic, you can ignore it"). Flag cosmetic/infrastructure bits, not taught concepts.
- **Prefer many explicit lines/steps over dense one-liners** ("dove puoi spezza in più passaggi"). E.g. clean data with 3 sequential filters + `dropna` rather than one combined boolean mask with `|`/`~`; do groupby in two steps (`.max()` then `.sort_values()`) rather than a long method chain.
- **Introduce the simplest form first; demote the powerful/advanced form to a clearly-marked OPTIONAL box.** E.g. teach `df.groupby("k")["col"].max()` as the core move; keep `.agg(named=(col,func)).reset_index()` as "(optional) several summaries at once".
- **Mark error-prone-but-non-essential topics optional** (e.g. multi-condition filtering with `&`/`|`/`~` + parentheses), and tag the exercises that depend on them as "bonus".
- **Give the lesson a narrative thread** like lesson 2 had — frame it around one concrete question and follow it to an answer (lesson 3: "which heat treatment is strongest?" → quenched). Andrea found the first draft "noiosa" without it.
- Trim redundant cells (e.g. `df.info()` already shows columns + dtypes, so a separate shape/columns/dtypes cell is redundant).

**The intro lecture (`01_introduction`) is a "what Python can do" demo, NOT a coding tutorial — don't frame any cell as if the code must be understood (confirmed 2026-06-11).** Andrea disliked headings/notes like "Optional extension *for mixed-level students*", "*For students who already know some programming*…", "not necessary *for complete beginners*", and "don't worry if the code looks complicated *for beginners*". They imply that the *earlier* code WAS meant to be understood and only these bits are optional — wrong message for an intro whose whole point is to show the potential of Python. Fix: give the demo sections plain, descriptive titles about *what they show* ("The same workflow, for every sample at once"; "A numerical summary by treatment"). **Do NOT add repetitive "you don't need to understand this" disclaimers** — that point is made once at the top of the notebook ("You do not need to understand every line"); repeating it per-section is clutter. NOTE this is the opposite of the hands-on "mark advanced bits OPTIONAL" rule above: in the intro nothing is expected to be understood, so the optional/beginner-vs-advanced split makes no sense.

See [[python-course-plan]] and [[tensile-dataset-coherence]].
