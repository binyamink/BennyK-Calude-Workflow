# CLAUDE.MD -- Academic Project Development with Claude Code

<!-- HOW TO USE: Replace [BRACKETED PLACEHOLDERS] with your project info.
     Customize Beamer environments for your theme.
     Keep this file under ~150 lines — Claude loads it every session.
     See docs/workflow-guide.html for reference documentation. -->

**Project:** [YOUR PROJECT NAME]
**Institution:** [YOUR INSTITUTION]
**Branch:** main

---

## Core Principles

- **Plan first** -- enter plan mode before non-trivial tasks; save plans to `quality_reports/plans/`
- **Verify after** -- compile and confirm output at the end of every task
- **Single source of truth** -- Beamer `.tex` is authoritative for slides
- **Quality gates** -- nothing ships below 80/100
- **[LEARN] tags** -- when corrected, save `[LEARN:category] wrong → right` to MEMORY.md
- **Machine config** -- see `CLAUDE.local.md` for machine-specific paths (Stata, Julia, LaTeX)

---

## Folder Structure

```
[YOUR-PROJECT]/
├── CLAUDE.MD                    # This file
├── CLAUDE.local.md              # Machine-specific config (gitignored)
├── .claude/                     # Rules, skills, agents, hooks
├── Bibliography_base.bib        # Centralized bibliography
├── Figures/                     # Figures and images
├── Preambles/header.tex         # LaTeX headers
├── Slides/                      # Beamer .tex files
├── data/                        # raw/, processed/, codebooks/ (see data-readme template)
├── scripts/                     # Code: R/, julia/, stata/, python/
├── quality_reports/             # Plans, session logs, merge reports
├── explorations/                # Research sandbox (see rules)
├── templates/                   # Session log, quality report templates
└── master_supporting_docs/      # Papers and existing slides
```

---

## Commands

```bash
# LaTeX — slides (xelatex for Unicode/custom fonts)
cd Slides && TEXINPUTS=../Preambles:$TEXINPUTS xelatex -interaction=nonstopmode file.tex
BIBINPUTS=..:$BIBINPUTS bibtex file
TEXINPUTS=../Preambles:$TEXINPUTS xelatex -interaction=nonstopmode file.tex
TEXINPUTS=../Preambles:$TEXINPUTS xelatex -interaction=nonstopmode file.tex

# LaTeX — papers (pdflatex, faster)
pdflatex -interaction=nonstopmode paper.tex

# Julia
julia scripts/julia/solve_model.jl

# Stata (Windows — MUST use PowerShell wrapper, see stata-execution rule)
powershell -Command "& '<stata_path>' /e do 'scripts/stata/analysis.do'"

# Quality score (supports .tex, .R, .jl, .do)
python scripts/quality_score.py Slides/file.tex
```

---

## Quality Thresholds

| Score | Gate | Meaning |
|-------|------|---------|
| 80 | Commit | Good enough to save |
| 90 | PR | Ready for deployment |
| 95 | Excellence | Aspirational |

---

## Skills Quick Reference

| Command | What It Does |
|---------|-------------|
| `/compile-latex [file]` | 3-pass XeLaTeX + bibtex |
| `/proofread [file]` | Grammar/typo/overflow review |
| `/visual-audit [file]` | Slide layout audit |
| `/pedagogy-review [file]` | Narrative, notation, pacing review |
| `/review-r [file]` | R code quality review |
| `/review-julia [file]` | Julia code quality review |
| `/review-stata [file]` | Stata do-file quality review |
| `/slide-excellence [file]` | Combined multi-agent review |
| `/validate-bib` | Cross-reference citations |
| `/devils-advocate` | Challenge slide design |
| `/create-lecture` | Full lecture creation |
| `/commit [msg]` | Stage, commit, PR, merge |
| `/lit-review [topic]` | Literature search + synthesis |
| `/research-ideation [topic]` | Research questions + strategies |
| `/interview-me [topic]` | Interactive research interview |
| `/review-paper [file]` | Manuscript review |
| `/data-analysis [dataset]` | End-to-end analysis (R/Julia/Stata) |
| `/learn [skill-name]` | Extract discovery into persistent skill |
| `/context-status` | Show session health + context usage |
| `/deep-audit` | Repository-wide consistency audit |

---

<!-- CUSTOMIZE: Replace the example entries below with your own Beamer environments. -->

## Beamer Custom Environments

| Environment       | Effect        | Use Case       |
|-------------------|---------------|----------------|
| `[your-env]`      | [Description] | [When to use]  |

---

## Current Project State

| Lecture | Beamer | Key Content |
|---------|--------|-------------|
| 1: [Topic] | `Lecture01_Topic.tex` | [Brief description] |
| 2: [Topic] | `Lecture02_Topic.tex` | [Brief description] |
