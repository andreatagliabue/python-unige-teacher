# Lesson 5 — Three Independent Modules

Choose the topic that interests you most. Each notebook is fully self-contained: you can run it on its own, in any order, without opening the others.

---

## 05a — Nonlinear Curve Fitting (`05a_curve_fitting.ipynb`)

**What you will do:** fit a mathematical model to experimental data and extract physical parameters with their uncertainty.

**The example:** the stress-strain curve of a quenched steel specimen (S03) is modelled with a Voce-type saturation law up to the ultimate tensile strength. `scipy.optimize.curve_fit` returns the best-fit parameters and their covariance matrix, from which we compute uncertainties and the coefficient of determination R².

**Key tools:** `scipy.optimize.curve_fit`, `np.sqrt(np.diag(pcov))`, residual plots.

---

## 05b — Statistics and Uncertainty (`05b_statistics.ipynb`)

**What you will do:** decide whether an observed difference between two groups is statistically meaningful or just compatible with measurement noise.

**The example:** hardness measurements (HV) from four steel treatments. We compute mean, standard deviation, and standard error of the mean, draw error-bar plots, and run a two-sample Welch t-test to compare quenched vs annealed samples.

**Key tools:** `groupby().agg()`, `plt.bar(..., yerr=...)`, `scipy.stats.ttest_ind`.

---

## 05c — Automation: Many Files (`05c_automation.ipynb`)

**What you will do:** write an analysis function once and run it automatically on every file in a folder, collecting results into a single summary table.

**The example:** five tensile-test files (one per specimen) are processed in a loop. A reusable `analyse_tensile_file()` function extracts the UTS and strain at UTS from each file; the results are assembled into a DataFrame and saved as a CSV.

**Key tools:** `pathlib.Path`, `glob.glob`, `pd.DataFrame`, `to_csv`.
