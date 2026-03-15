---
paths:
  - "Slides/**/*.tex"
  - "scripts/**/*.R"
  - "**/*.jl"
  - "**/*.do"
---

# Quality Gates & Scoring Rubrics

## Thresholds

- **80/100 = Commit** -- good enough to save
- **90/100 = PR** -- ready for deployment
- **95/100 = Excellence** -- aspirational

## R Scripts (.R)

| Severity | Issue | Deduction |
|----------|-------|-----------|
| Critical | Syntax errors | -100 |
| Critical | Domain-specific bugs | -30 |
| Critical | Hardcoded absolute paths | -20 |
| Major | Missing set.seed() | -10 |
| Major | Missing figure generation | -5 |

## Beamer Slides (.tex)

| Severity | Issue | Deduction |
|----------|-------|-----------|
| Critical | XeLaTeX compilation failure | -100 |
| Critical | Undefined citation | -15 |
| Critical | Overfull hbox > 10pt | -10 |

## Julia Scripts (.jl)

| Severity | Issue | Deduction |
|----------|-------|-----------|
| Critical | Syntax errors | -100 |
| Critical | Hardcoded absolute paths | -20 |
| Critical | Type instability in hot loop | -20 |
| Major | Missing Random.seed!() | -10 |
| Major | Missing JLD2 serialization | -5 |
| Minor | Style violation | -1 |

## Stata Do-Files (.do)

| Severity | Issue | Deduction |
|----------|-------|-----------|
| Critical | Execution error (r() in log) | -100 |
| Critical | Hardcoded absolute paths | -20 |
| Major | Missing log file | -15 |
| Major | Missing set seed | -10 |
| Major | Missing version statement | -5 |
| Minor | Style violation | -1 |

## Enforcement

- **Score < 80:** Block commit. List blocking issues.
- **Score < 90:** Allow commit, warn. List recommendations.
- User can override with justification.

## Quality Reports

Generated **only at merge time**. Use `templates/quality-report.md` for format.
Save to `quality_reports/merges/YYYY-MM-DD_[branch-name].md`.

## Tolerance Thresholds (Research)

<!-- Customize for your domain -->

| Quantity | Tolerance | Rationale |
|----------|-----------|-----------|
| Point estimates | [e.g., 1e-6] | [Numerical precision] |
| Standard errors | [e.g., 1e-4] | [MC variability] |
| Coverage rates | [e.g., +/- 0.01] | [MC with B reps] |
