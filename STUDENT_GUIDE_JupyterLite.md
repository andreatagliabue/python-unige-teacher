# Student Guide — Working with JupyterLite

Welcome! In this course you will write and run Python directly **in your web browser**, using a tool called **JupyterLite**. There is **nothing to install**, **no account to create**, and **nothing to download**: when you open the course link, all the notebooks and data files are already there. This short guide tells you everything you need. Read it once before the first lesson.

---

## 1. What is JupyterLite (and what you need)

- **JupyterLite** is a version of the Jupyter Notebook environment that runs entirely inside your browser. Python itself runs on your computer, *inside the browser tab* — not on a remote server.
- **You do NOT need to install Python, Anaconda, or anything else.**
- **You do NOT need to log in or create an account.**
- **You do NOT need to upload anything:** the course materials are already loaded for you.

**What you DO need:**

- A laptop (Windows, macOS, or Linux — it does not matter).
- A recent version of **Google Chrome, Microsoft Edge, or Firefox**. Avoid Internet Explorer and very old browsers.
- An **internet connection**, especially the first time you open it (the browser downloads Python and the scientific libraries the first time — this can take 30–60 seconds).
- Do **not** use the browser in *Private / Incognito* mode for the course: your work would be deleted every time you close the window (see Section 5).

---

## 2. Opening the course

1. Open your browser.
2. Go to the course link:

   > **Course link:** **https://andreatagliabue.github.io/python-unige/**
   >
   > (To jump straight into the first lesson, you can also use:
   > `https://andreatagliabue.github.io/python-unige/notebooks/index.html?path=01_introduction.ipynb`)

3. Wait a few seconds. You will see the **JupyterLab interface**:
   - on the **left**, a *file browser* showing the course notebooks and a `data` folder (these are already there for you);
   - in the **center**, the working area where notebooks open as tabs.

> 💡 **First time is slower.** The very first time you run Python code, the browser downloads the Python engine and libraries. This happens only once per browser; afterwards it is fast.

---

## 3. Opening and running a notebook

1. In the **left file browser**, **double-click** the notebook for the lesson (a file ending in `.ipynb`). It opens as a tab in the center.
2. A notebook is a sequence of **cells**. There are two kinds:
   - **text cells** (explanations, in formatted text);
   - **code cells** (grey boxes containing Python).
3. To **run a cell**: click on it, then press **`Shift` + `Enter`**.
   - The code runs, the result appears just below the cell, and the selection moves to the next cell.
4. The small number in brackets to the left of a code cell, e.g. `[1]`, shows the order in which cells were run. While a cell is running you see `[*]`.

### Run everything in order

The **order matters**: later cells often depend on earlier ones. The safest way to start a lesson is:

- Open the menu **`Run` → `Run All Cells`** to execute the whole notebook from top to bottom.

If something behaves strangely, reset and start clean:

- **`Kernel` → `Restart Kernel and Run All Cells…`** — this clears everything and runs the notebook again from the beginning.

> 💡 **Golden rule for beginners:** if you get confused, do **Restart Kernel and Run All Cells**. Most "it doesn't work" problems disappear after this.

---

## 4. Editing and experimenting

- Click inside a code cell to edit the Python text, then press **`Shift` + `Enter`** to run it again.
- Add a new cell with the **`+`** button in the toolbar.
- It is completely normal to get **error messages** (red text). An error does not mean you are bad at this — it just means Python found something it could not understand. Read the **last line** of the error: it usually tells you what went wrong.

---

## 5. Saving your work (please read — this is important!)

Your work lives **inside your browser's storage on this specific computer**. This has two consequences:

- ✅ If you simply close and reopen the tab on the **same browser and same computer**, your work is still there.
- ❌ Your work is **NOT** synced to the cloud and is **NOT** available on another computer.
- ❌ If you clear your browser history / cached data, or use *Private / Incognito* mode, your work **will be lost**.

**Therefore, at the end of every lesson, download a copy of your notebook to your computer:**

1. Open the menu **`File` → `Download`** (or right-click the file in the left browser → **Download**).
2. Save the `.ipynb` file somewhere safe on your computer.

If you ever want to continue your downloaded notebook later, you can drag it back into the file browser to upload it.

---

## 6. Troubleshooting

| Problem | What to do |
|---|---|
| **`NameError: name '...' is not defined`** | You probably ran cells out of order, or skipped one. Do **Kernel → Restart Kernel and Run All Cells**. |
| **First cell takes very long / seems stuck** | The first run downloads Python and the libraries. Wait up to a minute and make sure you have internet. |
| **A cell shows `[*]` and never finishes** | It may be waiting or stuck. Use **Kernel → Interrupt Kernel**, then run again. |
| **Everything looks broken (errors everywhere)** | This is almost always the *running state*, not your file. Do **Kernel → Restart Kernel and Run All Cells** — reloading the page also resets the running state. This fixes the large majority of issues. |
| **I edited the notebook and want the ORIGINAL back** | Your saved edits live in the browser, so **a reload keeps them** (it does not undo your changes). To restore the original course version, clear this site's data in your browser (browser settings → *clear site data / browsing data* for this site), then reopen the link. Files you never edited are always the originals. |
| **`FileNotFoundError` when reading a data file** | Make sure you are running the notebook from the course link (where the `data` folder is provided). If it persists, reload the page and tell the instructor. |

---

## 7. Quick reference

- **Run a cell:** `Shift` + `Enter`
- **Run the whole notebook:** `Run` → `Run All Cells`
- **Clean restart:** `Kernel` → `Restart Kernel and Run All Cells…`
- **Save to your computer:** `File` → `Download`
- **Add a cell:** `+` button in the toolbar

You are ready. See you in the first lesson! 🚀
