# AI Agent Instructions — mypackage test fixture

This minimal Poetry project exists to validate the `reqstool-python-poetry-plugin` end-to-end.
Run the steps below from the **repository root** (`reqstool-python-poetry-plugin/`).

## Prerequisites

A `.venv` must exist in the repository root with both the plugin and Poetry installed.
If it is missing, recreate it:

```bash
python3.13 -m venv .venv
.venv/bin/pip install -e .   # install plugin in editable mode
.venv/bin/pip install poetry  # install Poetry into the same venv
```

Also ensure Poetry can find Python 3.13 in the test project:

```bash
cd tests/fixtures/test_project
echo "3.13.11" > .python-version          # already committed; recreate if missing
../../../.venv/bin/poetry env use python3.13
cd ../../..
```

## Validation steps

Run all commands from `tests/fixtures/test_project/`.

### 1 — Install (updates `pyproject.toml` includes)

```bash
cd tests/fixtures/test_project
../../../.venv/bin/poetry install
```

**Expected:** plugin prints version, then adds three include entries to
`pyproject.toml` under `[tool.poetry]`:

```toml
include = ["reqstool_config.yml", "docs/reqstool/**/*", "build/reqstool/**/*"]
```

### 2 — Build

```bash
../../../.venv/bin/poetry build
```

**Expected output (in order):**
1. `[reqstool] plugin version <x.y.z>`
2. `[reqstool] added to reqstool_config.yml: docs/reqstool/requirements.yml`
3. `[reqstool] added to reqstool_config.yml: build/reqstool/annotations.yml`
4. `[reqstool] Created reqstool_config.yml in project root`
5. Poetry builds sdist + wheel
6. `[reqstool] Removed reqstool_config.yml from project root`

### 3 — Check generated files

```bash
# annotations.yml must exist
test -f build/reqstool/annotations.yml && echo "OK: annotations.yml"

# reqstool_config.yml must be gone from project root
test ! -f reqstool_config.yml && echo "OK: reqstool_config.yml cleaned up"
```

### 4 — Check sdist contents

```bash
tar -tzf dist/mypackage-0.1.0.tar.gz | sort
```

**Expected entries (among others):**
- `mypackage-0.1.0/reqstool_config.yml`
- `mypackage-0.1.0/build/reqstool/annotations.yml`
- `mypackage-0.1.0/docs/reqstool/requirements.yml`

### 5 — Verify `annotations.yml` content

```bash
cat build/reqstool/annotations.yml
```

Must contain `REQ_001` mapped to `src.mypackage.main.hello`.

## Pass criteria

All five checks above must succeed without errors.
If any step fails, report the full output and the contents of
`build/reqstool/annotations.yml` and `dist/mypackage-0.1.0.tar.gz` listing.
