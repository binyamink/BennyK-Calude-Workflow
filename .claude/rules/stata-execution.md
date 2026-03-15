---
paths:
  - "**/*.do"
  - "scripts/stata/**"
---

# Stata Execution Protocol

**CRITICAL: Stata on Windows requires special handling. Direct bash calls fail silently.**

---

## Windows Execution (MANDATORY)

Stata on Windows **MUST** be executed via PowerShell wrapper:

```bash
powershell -Command "& 'C:/Program Files/Stata19/StataNow-MP.exe' /e do 'scripts/stata/analysis.do'"
```

### Why This Matters

- Direct `bash` calls to Stata on Windows return immediately without blocking
- The script appears to succeed but may not have run at all
- This causes **silent failures** — no error, no output, wasted debugging time
- The PowerShell wrapper ensures the process blocks until Stata finishes

### Stata Path Auto-Detection

If CLAUDE.local.md does not specify a path, check in this order:

1. `C:/Program Files/Stata19/StataNow-MP.exe` (StataNow 19)
2. `C:/Program Files/Stata18/StataMP-64.exe` (Stata 18)
3. `C:/Program Files/Stata17/StataMP-64.exe` (Stata 17)
4. `C:/Program Files (x86)/Stata*/Stata*.exe` (32-bit fallback)

Use `powershell -Command "Test-Path '<path>'"` to check existence.

### Full Execution Pattern

```bash
# 1. Detect Stata path
STATA_PATH="C:/Program Files/Stata19/StataNow-MP.exe"

# 2. Execute via PowerShell (BLOCKS until complete)
powershell -Command "& '$STATA_PATH' /e do 'scripts/stata/analysis.do'"

# 3. ALWAYS check log file for errors after execution
grep -E "^r\([0-9]+\)" scripts/stata/analysis.log && echo "STATA ERROR DETECTED" || echo "No errors in log"
```

## macOS / Linux Execution

```bash
stata-mp -b do "scripts/stata/analysis.do"
```

- The `-b` flag runs in batch mode (blocks until complete)
- Check `.log` file for errors after execution

## Post-Execution Verification (ALL PLATFORMS)

After every Stata execution, **ALWAYS**:

1. **Check the `.log` file** for error codes:
   ```bash
   grep -E "^r\([0-9]+\)" scripts/stata/analysis.log
   ```
   - `r(111)` = observation not found
   - `r(198)` = syntax error
   - `r(601)` = file not found
   - `r(2000)` = no observations
   - `r(2001)` = insufficient observations

2. **Verify output files** were created:
   ```bash
   ls -la output/tables/*.tex output/figures/*.pdf 2>/dev/null
   ```

3. **Check log for warnings:**
   ```bash
   grep -i "note:" scripts/stata/analysis.log
   grep -i "dropped" scripts/stata/analysis.log
   ```

## Known Issues

- **pystata / `sfi` module:** Do NOT use pystata for execution — the `sfi` module produces errors in many environments. Use the PowerShell/batch approach instead.
- **Working directory:** Stata sets `cd` to the do-file's directory. Use globals for project-relative paths.
- **Long paths:** Windows paths with spaces MUST be quoted in the PowerShell command.
