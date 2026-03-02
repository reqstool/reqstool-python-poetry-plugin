# mypackage

Minimal test project for manual validation of `reqstool-python-poetry-plugin`.

## Manual testing

Run all commands from the **repository root** (`reqstool-python-poetry-plugin/`).

### 1 — Set up the venv (first time only)

```bash
python3.13 -m venv .venv
.venv/bin/pip install -e .
.venv/bin/pip install poetry
```

### 2 — Point Poetry at Python 3.13

```bash
cd tests/fixtures/test_project
../../../.venv/bin/poetry env use python3.13
```

### 3 — Run install (populates `pyproject.toml` includes)

```bash
../../../.venv/bin/poetry install
```

Check that `pyproject.toml` now has an `include` entry under `[tool.poetry]`.

### 4 — Run build

```bash
../../../.venv/bin/poetry build
```

**What to verify:**
- `build/reqstool/annotations.yml` exists and contains `REQ_001`
- `reqstool_config.yml` is **not** present in the project root (cleaned up after build)
- `dist/mypackage-0.1.0.tar.gz` contains `reqstool_config.yml`, `build/reqstool/annotations.yml`, and `docs/reqstool/requirements.yml`

```bash
tar -tzf dist/mypackage-0.1.0.tar.gz | sort
```

## AI agent instructions

See [AGENTS.md](AGENTS.md) for step-by-step instructions suitable for AI agents.
