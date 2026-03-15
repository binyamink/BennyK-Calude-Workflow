---
paths:
  - "Slides/**/*.tex"
  - "**/*.jl"
  - "**/*.do"
  - "**/*.py"
---

# Task Completion Verification Protocol

**At the end of EVERY task, Claude MUST verify the output works correctly.** This is non-negotiable.

## For LaTeX/Beamer Slides:
1. Compile with xelatex and check for errors
2. Open the PDF to verify figures render (`open` on macOS, `xdg-open` on Linux)
3. Check for overfull hbox warnings

## For R Scripts:
1. Run `Rscript scripts/R/filename.R`
2. Verify output files (PDF, RDS) were created with non-zero size
3. Spot-check estimates for reasonable magnitude

## For Julia Scripts:
1. Run `julia scripts/julia/filename.jl`
2. Verify output files (PDF, JLD2, CSV) were created with non-zero size
3. Check for convergence warnings in stdout
4. Spot-check results for reasonable magnitude

## For Stata Do-Files:
1. Execute via PowerShell wrapper (Windows): `powershell -Command "& '<stata_path>' /e do '<do_file>'"`
   Or batch mode (macOS/Linux): `stata-mp -b do "scripts/stata/filename.do"`
2. Check `.log` file for `r()` error codes
3. Verify output files (tables .tex, figures .pdf) were created
4. Check log for dropped observations or unexpected warnings

## For Python Scripts:
1. Run `python scripts/python/filename.py`
2. Verify output files were created with non-zero size
3. Check for tracebacks in stderr

## Common Pitfalls:
- **Assuming success**: Always verify output files exist AND contain correct content
- **Stale TikZ SVGs**: extract_tikz.tex diverges from Beamer source → always diff-check
- **Stata silent failures (Windows)**: Direct bash calls don't block → use PowerShell wrapper
- **Stata log errors**: Always grep for `r()` patterns after execution

## Verification Checklist:
```
[ ] Output file created successfully
[ ] No compilation/render errors
[ ] Images/figures display correctly
[ ] Paths resolve in deployment location (docs/)
[ ] Opened in browser/viewer to confirm visual appearance
[ ] Reported results to user
```
