# My Claude Code Workflow

This is my personal fork of [Pedro Sant'Anna's claude-code-my-workflow](https://github.com/pedrohcgs/claude-code-my-workflow), tailored for my research in international trade, macroeconomics, and spatial economics.

The original template was built around R and Quarto. I've customized it for my stack: **Julia** (quantitative modeling), **Stata** (empirical work), **LaTeX/Beamer** (papers and slides), and occasionally **R** and **Python**. I also removed the Quarto/RevealJS infrastructure since I don't use it.

For the original project and full documentation, see the [upstream repo](https://github.com/pedrohcgs/claude-code-my-workflow) and its [guide](https://psantanna.com/claude-code-my-workflow/).

---

## What I Changed from the Original

- **Added Julia support** — code conventions, reviewer agent, quality scoring, `/review-julia` skill
- **Added Stata support** — code conventions, reviewer agent, quality scoring, `/review-stata` skill, and the critical Windows PowerShell execution protocol
- **Added Python conventions** (lightweight, for utility scripts)
- **Added data management** — `data/raw/`, `data/processed/`, `data/codebooks/` convention with provenance tracking
- **Removed Quarto/RevealJS** — deleted all Quarto agents, skills, rules, theme, and deploy scripts
- **Added writing style conventions** — no periods on section titles, plain language
- **Added `CLAUDE.local.md.example`** — machine-specific config for Stata paths, LaTeX engine, etc.
- **Updated `.gitignore`** — added data formats (`.dta`, `.csv`, `.parquet`, spatial formats), Stata/Julia artifacts

---

## How It Works

You describe a task. Claude plans the approach, implements it, runs specialized review agents, fixes issues, re-verifies, and scores against quality gates — all autonomously. Say "just do it" and it auto-commits too.

### Quality Gates

Every file gets a score (0–100). Scores below threshold block the action:
- **80** — commit threshold
- **90** — PR threshold
- **95** — excellence (aspirational)

### Specialized Agents

Instead of one general-purpose reviewer, focused agents each check one dimension:

- **slide-auditor** — visual layout
- **proofreader** — grammar/typos
- **pedagogy-reviewer** — teaching quality
- **r-reviewer** / **julia-reviewer** / **stata-reviewer** — language-specific code quality
- **domain-reviewer** — field-specific correctness (template — customize for your field)
- **tikz-reviewer** — TikZ diagram critique
- **verifier** — end-to-end task verification

---

## Skills Quick Reference

| Skill | What It Does |
|-------|-------------|
| `/compile-latex` | 3-pass XeLaTeX compilation with bibtex |
| `/review-r` / `/review-julia` / `/review-stata` | Language-specific code review |
| `/slide-excellence` | Combined multi-agent slide review |
| `/create-lecture` | Full Beamer lecture creation workflow |
| `/data-analysis` | End-to-end analysis (auto-detects R/Julia/Stata) |
| `/lit-review` | Literature search + synthesis |
| `/research-ideation` | Generate research questions + strategies |
| `/review-paper` | Manuscript review with simulated referee objections |
| `/commit` | Stage, commit, create PR, and merge |
| `/deep-audit` | Repository-wide consistency audit |

See `CLAUDE.md` for the full list.

---

## Prerequisites

| Tool | Required For | Install |
|------|-------------|---------|
| [Claude Code](https://code.claude.com/docs/en/overview) | Everything | `npm install -g @anthropic-ai/claude-code` |
| XeLaTeX | Slides | [TeX Live](https://tug.org/texlive/) or [MiKTeX](https://miktex.org/) |
| Julia | Quantitative modeling | [julialang.org](https://julialang.org/) |
| Stata | Empirical analysis | [stata.com](https://www.stata.com/) |
| R | Figures & analysis | [r-project.org](https://www.r-project.org/) |

Not all tools are needed — install only what your project uses. Claude Code is the only hard requirement.

---

## Credits

Original workflow by [Pedro Sant'Anna](https://github.com/pedrohcgs/claude-code-my-workflow) (Emory University), extracted from Econ 730: Causal Panel Data. See also the community extensions: [clo-author](https://github.com/hsantanna88/clo-author), [claudeblattman](https://github.com/chrisblattman/claudeblattman), [MixtapeTools](https://github.com/scunning1975/MixtapeTools).

---

## License

MIT License. See [LICENSE](LICENSE).
