---
paths:
  - "**/*.jl"
  - "scripts/julia/**"
---

# Julia Code Standards

**Standard:** Senior computational economist + research software engineer quality

---

## 1. Module Structure & Header

Every script starts with:

```julia
# ============================================================
# [Descriptive Title]
# Author: [from project context]
# Purpose: [What this script does]
# Inputs: [Data files, parameters]
# Outputs: [Figures, JLD2 files, CSV]
# ============================================================

using Random, LinearAlgebra, Statistics
using DataFrames, CSV
using Plots  # or CairoMakie

Random.seed!(20260315)  # YYYYMMDD format
```

- All `using` statements at top, grouped: stdlib → data → plotting → project-specific
- `Random.seed!()` called ONCE at top (never inside loops/functions)

## 2. Naming & Style

- `snake_case` for functions and variables
- `PascalCase` for types/structs only
- Verb-noun pattern for functions: `solve_model`, `compute_welfare`, `generate_data`
- Docstrings for all public functions:

```julia
"""
    solve_model(params; tol=1e-10, maxiter=1000)

Solve the Aiyagari model via value function iteration.

# Arguments
- `params::ModelParams`: calibrated parameters
- `tol::Float64`: convergence tolerance
- `maxiter::Int`: maximum iterations
"""
```

## 3. Type Stability & Performance

- **Type-annotate function signatures** for public functions
- **Avoid global variables** — pass as arguments or use `const`
- **Pre-allocate arrays** in hot loops: `similar()`, `zeros()`, `Vector{Float64}(undef, N)`
- **Use `@views`** for array slices in performance-critical code
- **Profile before optimizing:** `@btime` from BenchmarkTools, `@profview`
- **Common trap:** Type instability from untyped containers — use `Vector{Float64}` not `[]`

## 4. Figures & Visualization

```julia
# --- Institutional palette ---
const PRIMARY_BLUE   = "#012169"
const PRIMARY_GOLD   = "#f2a900"
const ACCENT_GRAY    = "#525252"
const POSITIVE_GREEN = "#15803d"
const NEGATIVE_RED   = "#b91c1c"
```

### Plots.jl Convention

```julia
function dualsave(plt, name; dir="output/figures", width=800, height=400)
    mkpath(dir)
    savefig(plt, joinpath(dir, "$name.pdf"))
    savefig(plt, joinpath(dir, "$name.png"))
end
```

- Explicit size in every `savefig` or `dualsave` call
- Transparent background: `background_color=:transparent`
- Font sizes readable at projection: `guidefontsize=12, tickfontsize=10`
- Legend position: `:bottomright` or `:outerright`

## 5. Serialization (JLD2 Pattern)

**Heavy computations saved as JLD2; downstream scripts load pre-computed data.**

```julia
using JLD2

# Save
@save joinpath(out_dir, "model_results.jld2") equilibrium welfare_gains policy_functions

# Load
@load joinpath(out_dir, "model_results.jld2") equilibrium welfare_gains policy_functions
```

- Every computed object has a corresponding `@save` call
- Descriptive filenames matching the content
- Both raw results AND summary tables saved
- Use `joinpath()` for cross-platform paths

## 6. Common Pitfalls

| Pitfall | Impact | Prevention |
|---------|--------|------------|
| Global scope in scripts | 100x slower | Wrap in `function main()` or `let` block |
| Untyped containers `[]` | Type instability | Use `Float64[]` or `Vector{Float64}()` |
| String interpolation in hot loop | Allocations | Pre-format or use `@sprintf` |
| Missing `Random.seed!()` | Non-reproducible | Always at top of script |
| `push!` in tight loop | Slow growth | Pre-allocate with `sizehint!` or fixed array |
| Hardcoded absolute paths | Breaks portability | Use `joinpath()` with relative paths |

## 7. Line Length & Mathematical Exceptions

**Standard:** Keep lines <= 92 characters.

**Exception: Mathematical Formulas** — lines may exceed limit **if and only if:**

1. Breaking the line would harm readability of the math (Bellman equations, FOCs, matrix operations)
2. An inline comment explains the mathematical operation
3. The line is in a numerically intensive section

## 8. Error Handling

```julia
# Convergence check
iter > maxiter && @warn "VFI did not converge after $maxiter iterations"

# Input validation for public functions
@assert length(grid) > 0 "Grid must be non-empty"
```

## 9. Code Quality Checklist

```
[ ] using statements at top, grouped
[ ] Random.seed!() once at top
[ ] All paths via joinpath(), relative
[ ] Public functions have docstrings + type annotations
[ ] Figures: transparent bg, explicit dimensions, dualsave()
[ ] JLD2: every computed object saved
[ ] Comments explain WHY not WHAT
[ ] No global mutable state in hot paths
[ ] Performance-critical code benchmarked
```
