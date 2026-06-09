# Tester Guide — Always Loading the Latest Version

> **Read this first.** This guide is **only for us, the people testing the course right now.**
> It is **not** something students will ever have to do.
>
> We need it because the lessons are **still being updated**: every time a notebook is
> fixed and pushed, the live site changes. JupyterLite, however, **saves a personal copy
> of each notebook inside your browser** the first time you open it — and it will keep
> showing you *that* copy, not the updated one online. So while we iterate, we have to
> make sure we are looking at the freshly-deployed version, not a stale local copy.
>
> For students this never matters: they open it once and just work. The caching behaviour
> is actually a *feature* for them (their work is saved automatically in the browser).

---

## Why a normal refresh is not enough

When you open a notebook in JupyterLite, two separate things get stored in your browser:

1. **The app and its files** — cached by the browser (HTTP cache + a *service worker*).
2. **Your working copy of the notebook** — copied into a private in-browser storage area
   (IndexedDB, the JupyterLite "drive").

A normal reload — even a *hard* reload (`Ctrl/Cmd + Shift + R`) — only refreshes part of
(1). It does **not** clear (2). So if you have already opened, say, `04_plotting_and_analysis.ipynb`,
JupyterLite keeps serving you the copy saved in your browser, and you will **not** see the
new version even after we push a fix.

**Bottom line:** to be certain you are on the latest version, you must either use a
**fresh private/incognito window** or **clear the site data**.

---

## Method 1 — Private / Incognito window (recommended, simplest)

A private window starts with **empty storage**, so there is no old notebook copy and no
stale cache. This is the easiest way to guarantee a clean test.

| Browser | Open a private window |
|---|---|
| **Chrome** | `Ctrl + Shift + N` (Windows/Linux) · `Cmd + Shift + N` (Mac) |
| **Edge** | `Ctrl + Shift + N` — "InPrivate" |
| **Firefox** | `Ctrl + Shift + P` (Windows/Linux) · `Cmd + Shift + P` (Mac) |
| **Safari** | `Cmd + Shift + N` |

**Important caveat:** a private session lives until you close **all** private windows.
Opening a second incognito tab reuses the same storage. So between two tests:

1. **Close every private/incognito window** (not just the tab).
2. Open a brand-new private window.
3. Paste the link and test.

That guarantees a clean drive each time.

---

## Method 2 — Clear the site data (when you tested in a normal window)

If you have already opened the course in a normal window and need to wipe the saved copy,
clear the data **for the course origin** (`andreatagliabue.github.io`). This removes the
JupyterLite drive, the cache, and the service worker in one go.

### Chrome / Edge (fastest, via DevTools)
1. Open the course site.
2. Press `F12` to open DevTools.
3. Go to the **Application** tab → left sidebar **Storage**.
4. Click **Clear site data** (this clears IndexedDB, cache storage, *and* unregisters the
   service worker — exactly what we need).
5. Close DevTools and reload (`Ctrl/Cmd + Shift + R`).

*Alternative without DevTools:* click the **padlock / "tune" icon** left of the address bar
→ **Site settings** (or "Cookies and site data") → **Delete data**.

### Firefox
1. Click the **padlock icon** left of the address bar.
2. **Clear cookies and site data…** → confirm.
   *(Or: Settings → Privacy & Security → Cookies and Site Data → Manage Data → search
   `andreatagliabue.github.io` → Remove Selected.)*
3. Reload (`Ctrl/Cmd + Shift + R`).

### Safari
1. **Safari → Settings → Privacy → Manage Website Data…**
2. Search `andreatagliabue.github.io` → **Remove** → **Done**.
   *(To also flush caches: enable the Develop menu in Settings → Advanced, then
   **Develop → Empty Caches**.)*
3. Reload.

---

## Method 3 — A separate browser / profile

If you do not want to disturb your main browser, just test in a **different browser** (or a
fresh browser profile) that you never use for the course. First open there = clean state.

---

## One more thing: give the deploy a moment

After a push, GitHub Pages takes **~1–2 minutes** to rebuild and publish, and its CDN may
serve the old files for a short while. So the safe sequence is:

1. Push the change.
2. Wait ~1–2 minutes.
3. Open in a **fresh incognito window** (Method 1).

If you still see the old version, it is almost always (a) the deploy hasn't finished, or
(b) you are looking at a saved copy — redo Method 1 or Method 2.

---

## Course links

**Course home:** https://andreatagliabue.github.io/python-unige/

Direct notebook links (`?path=` opens that notebook straight away):

| Lesson | Link |
|---|---|
| 1 — Introduction | https://andreatagliabue.github.io/python-unige/notebooks/index.html?path=01_introduction.ipynb |
| 2 — Python basics | https://andreatagliabue.github.io/python-unige/notebooks/index.html?path=02_python_basics.ipynb |
| 3 — Data I/O and tables | https://andreatagliabue.github.io/python-unige/notebooks/index.html?path=03_data_io_and_tables.ipynb |
| 4 — Plotting and analysis | https://andreatagliabue.github.io/python-unige/notebooks/index.html?path=04_plotting_and_analysis.ipynb |
| 5 — Fitting, statistics, automation | https://andreatagliabue.github.io/python-unige/notebooks/index.html?path=05_fitting_stats_automation.ipynb |

---

## Quick checklist for every test

- [ ] Pushed change, waited ~1–2 min for the deploy.
- [ ] Closed **all** previous incognito/private windows.
- [ ] Opened a **fresh** incognito window.
- [ ] Pasted the direct lesson link.
- [ ] (If anything looks stale) cleared site data — Method 2 — and reloaded.
