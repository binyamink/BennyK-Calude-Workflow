---
name: feedback_stata_execution
description: CRITICAL — Stata on Windows requires PowerShell wrapper; direct bash calls fail silently
type: feedback
---

Stata on Windows MUST be executed via PowerShell wrapper, not direct bash calls.

**Why:** Direct bash calls to Stata on Windows return immediately without blocking. The script appears to succeed but hasn't actually run. This causes silent failures that waste entire debugging sessions — it has happened multiple times across projects.

**How to apply:** Whenever executing a `.do` file on Windows:
1. Use: `powershell -Command "& '<stata_path>' /e do '<do_file>'"`
2. NEVER use: `stata /e do file.do` directly in bash
3. After execution, ALWAYS check the `.log` file for `r()` error patterns
4. See `.claude/rules/stata-execution.md` for the full protocol
