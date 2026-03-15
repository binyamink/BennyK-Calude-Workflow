---
paths:
  - "**/*.do"
  - "scripts/stata/**"
---

# Stata Code Standards

**Standard:** Publication-quality replication package

---

## 1. Do-File Structure & Header

Every do-file starts with:

```stata
* ============================================================
* [Descriptive Title]
* Author: [from project context]
* Purpose: [What this script does]
* Inputs: [Data files]
* Outputs: [Figures, tables, log files]
* ============================================================

version 18              // Pin Stata version for reproducibility
set more off
clear all
set seed 20260315       // YYYYMMDD format

* Paths (relative to project root)
global root   "."
global data   "$root/data"
global output "$root/output"
global tables "$root/output/tables"
global figures "$root/output/figures"

cap mkdir "$output"
cap mkdir "$tables"
cap mkdir "$figures"

* Logging
log using "$output/analysis_log.log", replace
```

## 2. Naming & Labeling

- **Variables:** lowercase, descriptive: `log_wage`, `years_edu`, `treat_post`
- **Globals/locals:** descriptive names, not single letters
- **Label everything:** `label variable log_wage "Log hourly wage (2020 USD)"`
- **Value labels** for categorical variables

## 3. Estimation & Standard Errors

- Document clustering level and rationale
- Use `reghdfe` for high-dimensional fixed effects (not `areg`)
- Report robust/clustered SEs explicitly
- Store estimates: `estimates store model_1`

### Known Pitfalls

| Stata Pattern | Trap | Correct Approach |
|--------------|------|-----------------|
| `areg y x, absorb(id)` | Incorrect dof adjustment with clusters | Use `reghdfe y x, absorb(id) cluster(id)` |
| `cluster(id)` with few clusters | Wild bootstrap needed | Use `boottest` with <50 clusters |
| `xtreg, fe` | Drops singletons silently | Use `reghdfe` which reports drops |
| `probit` + margins | Average marginal effects vs marginal effects at means | Use `margins, dydx(*)` for AME |
| `collapse` | Destroys original data | Always `preserve`/`restore` or work on copy |

## 4. Output

### Tables

```stata
* Use estout/esttab for publication tables
esttab model_1 model_2 model_3 using "$tables/main_results.tex", ///
    replace label se star(* 0.10 ** 0.05 *** 0.01) ///
    title("Main Results") ///
    stats(N r2_a, labels("Observations" "Adjusted R-squared"))
```

### Figures

```stata
* Export figures in multiple formats
graph export "$figures/treatment_effects.pdf", replace
graph export "$figures/treatment_effects.png", replace width(1200)
```

- Use a consistent scheme: `set scheme s2color` or custom
- Explicit dimensions for exported figures
- Descriptive titles and axis labels

## 5. Logging & Error Checking

- **Always** use `log using` at the start
- **Always** `log close` at the end (or use `capture log close` at top for safety)
- Check for errors after critical commands: `assert _rc == 0` or `if _rc != 0`
- Use `confirm file` to verify inputs exist
- Use `assert` to validate data assumptions: `assert wage > 0 if !missing(wage)`

## 6. Comment Quality

- Comments explain **WHY**, not WHAT
- Section headers describe purpose: `* 3. Estimate treatment effects using staggered DID`
- No commented-out dead code in final scripts
- Document data decisions: `* Drop outliers: top/bottom 1% of wage distribution (following AKM 1999)`

## 7. Code Quality Checklist

```
[ ] version XX at top
[ ] set more off, clear all
[ ] set seed at top
[ ] All paths relative via globals
[ ] log using at start, log close at end
[ ] Variables labeled
[ ] SEs documented and appropriate
[ ] Tables via esttab with proper formatting
[ ] Figures exported as PDF + PNG
[ ] Comments explain WHY not WHAT
[ ] No hardcoded absolute paths
```
