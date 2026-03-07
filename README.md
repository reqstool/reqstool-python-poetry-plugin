[![Commit Activity](https://img.shields.io/github/commit-activity/m/reqstool/reqstool-python-poetry-plugin?label=commits&style=for-the-badge)](https://github.com/reqstool/reqstool-python-poetry-plugin/pulse)
[![GitHub Issues](https://img.shields.io/github/issues/reqstool/reqstool-python-poetry-plugin?style=for-the-badge&logo=github)](https://github.com/reqstool/reqstool-python-poetry-plugin/issues)
[![License](https://img.shields.io/github/license/reqstool/reqstool-python-poetry-plugin?style=for-the-badge&logo=opensourceinitiative)](https://opensource.org/license/mit/)
[![Build](https://img.shields.io/github/actions/workflow/status/reqstool/reqstool-python-poetry-plugin/build.yml?style=for-the-badge&logo=github)](https://github.com/reqstool/reqstool-python-poetry-plugin/actions/workflows/build.yml)
[![Documentation](https://img.shields.io/badge/Documentation-blue?style=for-the-badge&link=docs)](https://reqstool.github.io)

# Reqstool Python Poetry Plugin

> [!WARNING]
> Poetry plugin support is currently untested. Functionality may be broken. Contributions welcome.

Poetry build plugin for [reqstool](https://github.com/reqstool/reqstool-client) that collects decorated code and generates `annotations.yml` during `poetry build`.

## Installation

```bash
poetry add reqstool-python-poetry-plugin
```

## Usage

Configure the plugin in `pyproject.toml`:

```toml
[tool.reqstool]
sources = ["src", "tests"]
test_results = ["build/**/junit.xml"]
dataset_directory = "docs/reqstool"
output_directory = "build/reqstool"
```

The plugin uses [reqstool-python-decorators](https://github.com/reqstool/reqstool-python-decorators) for processing.

## Documentation

Full documentation can be found [here](https://reqstool.github.io).

## Contributing

See the organization-wide [CONTRIBUTING.md](https://github.com/reqstool/.github/blob/main/CONTRIBUTING.md).

## License

MIT License.
