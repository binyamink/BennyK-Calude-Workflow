---
paths:
  - "data/**"
  - "scripts/**"
---

# Data Management Convention

## Directory Structure

```
data/
├── README.md          # Data dictionary + provenance (REQUIRED)
├── raw/               # Original, untouched source files
├── processed/         # Cleaned/merged datasets ready for analysis
└── codebooks/         # Variable documentation, survey instruments
```

- **`raw/`** is read-only after initial download — never modify originals
- **`processed/`** is reproducible from `raw/` via scripts
- Scripts that create processed data live in `scripts/` (not `data/`)

## Provenance

Every dataset in `data/raw/` must be documented in `data/README.md`:

```markdown
## [dataset_name].dta
- **Source:** [URL, paper citation, or "provided by co-author"]
- **Downloaded:** [YYYY-MM-DD]
- **Coverage:** [years, geography, unit of observation]
- **N:** [approximate row count]
- **Key variables:** [list the 5-10 most important]
- **License/access:** [public / restricted / proprietary]
```

## Git & Large Files

Data files are **gitignored by default** (see `.gitignore`).

- **Small reference files** (<1 MB): `git add -f data/raw/small_file.csv`
- **Large files**: Keep locally, document download instructions in `data/README.md`
- **Shared with co-authors**: Use Dropbox/Google Drive/Box link in README
- **Replication packages**: Include download script or `Makefile` target

## Naming Conventions

- Lowercase, underscores: `county_trade_flows_2010_2020.dta`
- Include coverage in name: years, geography, version
- Processed files mirror raw names with suffix: `county_trade_flows_2010_2020_clean.dta`

## In Scripts

```stata
* Stata — always reference via globals
use "$data/raw/county_trade_flows.dta", clear
```

```julia
# Julia — always use joinpath with relative paths
df = CSV.read(joinpath("data", "raw", "county_trade_flows.csv"), DataFrame)
```

```r
# R — always use relative paths
df <- read_csv("data/raw/county_trade_flows.csv")
```

## Validation

Before analysis, scripts should verify:
- Expected number of observations
- Key variables exist and have expected types
- No unexpected missingness in identifiers
- Panel balance (if panel data)
