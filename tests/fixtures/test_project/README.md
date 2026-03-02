# mypackage

Minimal test project for manual validation of `reqstool-python-poetry-plugin`.

## Prerequisites

A `.venv` must exist in the repository root with both the plugin and Poetry installed.
If it is missing, recreate it from the repository root:

```bash
python3.13 -m venv .venv
.venv/bin/pip install -e .
.venv/bin/pip install poetry
```

Point Poetry at Python 3.13 (once per machine):

```bash
cd tests/fixtures/test_project
../../../.venv/bin/poetry env use python3.13
```

## Validation

Run all commands from `tests/fixtures/test_project/`.

### 1 — Install

```bash
../../../.venv/bin/poetry install
```

Expected: plugin prints its version, then adds three entries to `pyproject.toml` under `[tool.poetry]`:

```toml
include = ["reqstool_config.yml", "docs/reqstool/**/*", "build/reqstool/**/*"]
```

### 2 — Build

```bash
../../../.venv/bin/poetry build
```

Expected output (in order):
1. `[reqstool] plugin version <x.y.z>`
2. `[reqstool] added to reqstool_config.yml: docs/reqstool/requirements.yml`
3. `[reqstool] added to reqstool_config.yml: build/reqstool/annotations.yml`
4. `[reqstool] Created reqstool_config.yml in project root`
5. Poetry builds sdist + wheel
6. `[reqstool] Removed reqstool_config.yml from project root`

### 3 — Verify

```bash
# annotations.yml must exist
test -f build/reqstool/annotations.yml && echo "OK: annotations.yml"

# reqstool_config.yml must be gone from project root after build
test ! -f reqstool_config.yml && echo "OK: reqstool_config.yml cleaned up"

# sdist must contain all reqstool files
tar -tzf dist/mypackage-0.1.0.tar.gz | sort
```

Expected entries in the sdist:
- `mypackage-0.1.0/reqstool_config.yml`
- `mypackage-0.1.0/build/reqstool/annotations.yml`
- `mypackage-0.1.0/docs/reqstool/requirements.yml`

`annotations.yml` must contain `REQ_001` mapped to `src.mypackage.main.hello`.
