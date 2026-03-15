---
paths:
  - "**/*.py"
  - "scripts/python/**"
---

# Python Code Standards (Lightweight)

Python is used occasionally for utility scripts, data processing, and API integrations.

---

## Conventions

- **PEP 8** style, enforced by formatter (black/ruff)
- **Type hints** for function signatures
- **`pathlib.Path`** for all file paths (not `os.path`)
- **`requirements.txt`** or `pyproject.toml` for dependencies
- **`if __name__ == "__main__":`** guard in all scripts
- **Relative paths** from project root
- **`random.seed()`** at top if stochastic

## Script Template

```python
#!/usr/bin/env python3
"""[Brief description of what this script does]."""

from pathlib import Path
import argparse

def main():
    """Entry point."""
    # ...

if __name__ == "__main__":
    main()
```

## Quality Checklist

```
[ ] Type hints on function signatures
[ ] Paths via pathlib.Path, relative
[ ] requirements.txt lists dependencies
[ ] __main__ guard present
[ ] No hardcoded absolute paths
```
