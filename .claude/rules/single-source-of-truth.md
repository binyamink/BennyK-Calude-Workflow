---
paths:
  - "Figures/**/*"
  - "Slides/**/*.tex"
---

# Single Source of Truth: Enforcement Protocol

**The Beamer `.tex` file is the authoritative source for ALL slide content.** Everything else is derived.

## The SSOT Chain

```
Beamer .tex (SOURCE OF TRUTH)
  ├── extract_tikz.tex → PDF → SVGs (derived)
  ├── Bibliography_base.bib (shared)
  └── Figures/LectureN/*.rds → figures (data source)

NEVER edit derived artifacts independently.
ALWAYS propagate changes from source → derived.
```

---

## TikZ Freshness Protocol (MANDATORY)

**Before using ANY TikZ SVG, verify it matches the current Beamer source.**

### Diff-Check Procedure

1. Read the TikZ block from the Beamer `.tex` file
2. Read the corresponding block from `Figures/LectureN/extract_tikz.tex`
3. Compare EVERY coordinate, label, color, opacity, and anchor point
4. If ANY difference exists: update `extract_tikz.tex` from Beamer, recompile, regenerate SVGs

### When to Re-Extract

Re-extract ALL TikZ diagrams when:
- The Beamer `.tex` file has been modified since last extraction
- Any TikZ-related quality issue is reported

---

## Content Fidelity Checklist

```
[ ] Math check: every equation appears with identical notation
[ ] Citation check: every \cite resolves in bibliography
[ ] Figure check: every \includegraphics references an existing file
[ ] No orphaned figures: every figure in Figures/ is referenced somewhere
```
