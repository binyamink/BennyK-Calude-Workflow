# Data Directory

## Overview

| Dataset | Source | Unit | Coverage | N (approx) |
|---------|--------|------|----------|-------------|
| | | | | |

---

## Raw Data (`raw/`)

### [dataset_name.dta]
- **Source:** [URL, citation, or provider]
- **Downloaded:** [YYYY-MM-DD]
- **Coverage:** [years, geography]
- **Unit of observation:** [firm, county, country-pair-year, etc.]
- **N:** [approximate]
- **Key variables:** [list 5-10]
- **License/access:** [public / restricted / proprietary]
- **Download instructions:** [if not included in repo]

---

## Processed Data (`processed/`)

### [processed_dataset_name.dta]
- **Created by:** `scripts/[language]/[script_name]`
- **Input(s):** `raw/[source_file(s)]`
- **Description:** [what cleaning/merging was done]
- **Key transformations:** [log transforms, deflation, sample restrictions]

---

## Codebooks (`codebooks/`)

[List any variable documentation, survey instruments, or data dictionaries]

---

## Reproduction

To recreate all processed data from raw sources:

```bash
# [Add commands here, e.g.:]
# Rscript scripts/R/01_clean_data.R
# julia scripts/julia/02_merge_trade.jl
# powershell -Command "& '<stata_path>' /e do 'scripts/stata/03_build_panel.do'"
```
