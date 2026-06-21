---
name: jupyterlite-gotchas
description: "JupyterLite/Pyodide pitfalls when authoring course notebooks (Path.exists, ipywidgets, interactivity)"
metadata: 
  node_type: memory
  type: reference
  originSessionId: fd352b6a-67e4-4a13-b026-42a1fcc9eee6
---

Practical gotchas hit while building the course notebooks on **JupyterLite (Pyodide)**. Apply these when writing the 4 hands-on lessons too. See [[python-course-plan]] and [[jupyterlite-deployment]].

- **`Path.exists()` can RAISE instead of returning False.** On Pyodide's emulated FS, `Path("data/x.csv").exists()` may throw `OSError [Errno 28] Invalid argument` for a non-resolving path. Don't use `.exists()` for data-folder detection. Instead try to read and catch:
  ```python
  DATA_DIR = None
  for _cand in (Path("data"), Path("../data"), Path(".")):
      try:
          pd.read_csv(_cand / "tensile_raw.csv", nrows=1); DATA_DIR = _cand; break
      except Exception:
          continue
  ```

- **ipywidgets is fragile in JupyterLite — avoid it.** Tried real draggable sliders (`ipywidgets.interact`); the frontend extension bundles fine (add `ipywidgets` to requirements.txt → `@jupyter-widgets/jupyterlab-manager` lands in the build), but the widget did not render reliably in the deployed Pyodide kernel. Not worth the trouble.

- **Use matplotlib animations for "wow"/interactive plots instead.** `matplotlib.animation.FuncAnimation` + `HTML(ani.to_jshtml())` gives a self-contained HTML/JS player with play/pause + a draggable frame slider. Zero extra deps (matplotlib already loaded), works offline after render, no kernel round-trip. Keep frames ~30-90 and per-frame compute light so generation stays a few seconds in Pyodide. The lecture's section 10 bonus gallery uses this for 12 animations.

- **Bulletproof data loading (use this in every lesson).** A stale/corrupted browser drive can make even `pd.read_csv("data/x.csv")` fail (`FileNotFoundError`/`OSError`) in JupyterLite. Use a loader that tries the local file first, then falls back to fetching over HTTP from the served site. The served URL is `<base>/files/data/<name>` where `base` = the worker href cut at `/extensions/` (verified: works both at domain root and under a repo subpath like `/python-unige/`):
  ```python
  def load_csv(name):
      import pandas as pd
      for folder in ("data", "../data", "."):
          try:
              return pd.read_csv(f"{folder}/{name}")
          except Exception:
              pass
      import js
      from pyodide.http import open_url
      base = str(js.location.href).split("/extensions/")[0]
      return pd.read_csv(open_url(f"{base}/files/data/{name}"))
  ```
  Local execution (nbconvert) never reaches the `import js` branch because the local read succeeds first. NOTE: deriving base from the first path segment is WRONG (gives `.../extensions`, fetches a fallback HTML page that parses as a bogus `(12, 1)` DataFrame) — always cut at `/extensions/`.

- **Plotly works for interactive 3D (free mouse rotation) — but render via the mime bundle, not `fig`.** Plotly `go.Surface` etc. render interactively in JupyterLite (WebGL, drag-to-rotate) IF `plotly` is in `requirements.txt` (build bundles the `jupyterlab-plotly` labextension) and the cell does `%pip install -q plotly` at runtime. BUT auto-displaying the figure (`fig` as last expression) triggers plotly's `pio.show()`, which raises `ValueError: Mime type rendering requires nbformat>=4.2.0` (nbformat isn't in the Pyodide kernel) — the plot still shows but an ugly error appears. Fix: emit the mime bundle directly, bypassing `pio.show()`:
  ```python
  from IPython.display import display
  display({"application/vnd.plotly.v1+json": fig.to_dict()}, raw=True)
  ```
  No error, same interactive plot. Used for the lecture's 10.12 free-energy surface.

- **Unseeded `np.random.default_rng()` works in Pyodide** (seeds from browser entropy) — use it when you want a demo to differ every run (e.g. the random chaotic double pendulum).

- **Make every demo cell self-contained.** Students may run a single cell in isolation, so each animation cell re-imports numpy/matplotlib and re-derives its own data (no reliance on earlier cells / globals like `DATA_DIR`).

- **NotebookEdit produces noisy git diffs — usually cosmetic.** Editing a cell with NotebookEdit rewrites its `source` from a JSON array-of-lines (one `"...\n"` per element) into a single consolidated string, so git reports many deletions + few insertions even for a one-line change. Don't be alarmed by a "4 insertions, 74 deletions" stat on a tiny edit; confirm integrity with `nbformat.validate` + cell count rather than reading the line count.

- **Verifying notebooks locally:** system `python3` has numpy/pandas/matplotlib/nbconvert. `python3 -m nbconvert --to notebook --execute` confirms 0 errors. For real JupyterLite testing, build with a venv (`jupyterlite-core==0.7.6`, `jupyterlite-pyodide-kernel==0.7.2`) + serve `dist/` + drive with Playwright (cached browsers at `~/.cache/ms-playwright`).
  - **Run the `--execute` check from `course_site/content/`, NOT from the source `notebooks/` folder.** The L3-style `load_table` helper only tries `("data", ".")` (no `../data`), so from `0N_topic/notebooks/` the data sits at `../data` → both local reads miss → it falls into the browser-only `import js` branch → `ModuleNotFoundError: No module named 'js'`. In `content/` the data is a `data/` subfolder next to the notebook (flat, exactly the JupyterLite layout), so the first read succeeds. Verify the deployed copy there. (Alternatively add `"../data"` to the helper's folder list, as the lecture's `load_csv` does.)
