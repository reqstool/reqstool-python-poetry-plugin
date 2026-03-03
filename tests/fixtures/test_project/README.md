# mypackage

Minimal test project for manual validation of `reqstool-python-poetry-plugin`.

## Prerequisites

A `.venv` must exist in the repository root with both the plugin and Poetry installed.
If it is missing, recreate it from the repository root (`reqstool-python-poetry-plugin/`):

```bash
python3.13 -m venv .venv
.venv/bin/pip install -e .
.venv/bin/pip install poetry reqstool
```

Point Poetry at Python 3.13 (once per machine):

```bash
cd tests/fixtures/test_project
../../../.venv/bin/poetry env use python3.13
```

## Validation

Run all commands from `tests/fixtures/test_project/`.

### 1 — Install (adds `include` entries to pyproject.toml)

```bash
../../../.venv/bin/poetry install
```

Expected: plugin prints its version, then adds three entries to `pyproject.toml` under `[tool.poetry]`:

```toml
include = ["reqstool_config.yml", "docs/reqstool/**/*", "build/reqstool/**/*"]
```

### 2 — Run tests

```bash
../../../.venv/bin/pytest tests/ --junit-xml=build/test-results/junit.xml -v
```

Expected: `test_hello` passes.

### 3 — Build

```bash
../../../.venv/bin/poetry build
```

Expected output (in order):
1. `[reqstool] plugin version <x.y.z>`
2. `[reqstool] added to reqstool_config.yml: docs/reqstool/requirements.yml`
3. `[reqstool] added to reqstool_config.yml: docs/reqstool/software_verification_cases.yml`
4. `[reqstool] added to reqstool_config.yml: build/reqstool/annotations.yml`
5. `[reqstool] Created reqstool_config.yml in project root`
6. Poetry builds sdist + wheel
7. `[reqstool] Removed reqstool_config.yml from project root`

### 4 — Check artefacts

```bash
# annotations.yml must exist
test -f build/reqstool/annotations.yml && echo "OK: annotations.yml"

# reqstool_config.yml must be gone after build
test ! -f reqstool_config.yml && echo "OK: reqstool_config.yml cleaned up"

# sdist must contain all reqstool files
tar -tzf dist/mypackage-0.1.0.tar.gz | sort
```

Expected entries in the sdist:
- `mypackage-0.1.0/reqstool_config.yml`
- `mypackage-0.1.0/build/reqstool/annotations.yml`
- `mypackage-0.1.0/docs/reqstool/requirements.yml`
- `mypackage-0.1.0/docs/reqstool/software_verification_cases.yml`

### 5 — Run reqstool status

Extract the sdist and merge in the local test results, then run `reqstool status`:

```bash
mkdir -p /tmp/mypackage-reqstool
tar -xzf dist/mypackage-0.1.0.tar.gz -C /tmp/mypackage-reqstool
cp -r build/test-results /tmp/mypackage-reqstool/mypackage-0.1.0/build/
../../../.venv/bin/reqstool status local -p /tmp/mypackage-reqstool/mypackage-0.1.0
```

Expected: all green — `REQ_001` implemented, `T1 P1`, no missing tests or SVCs.
