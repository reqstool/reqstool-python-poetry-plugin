
[![Commit Activity](https://img.shields.io/github/commit-activity/m/reqstool/reqstool-python-poetry-plugin?label=commits&style=for-the-badge)](https://github.com/reqstool/reqstool-python-poetry-plugin/pulse)
[![GitHub Issues](https://img.shields.io/github/issues/reqstool/reqstool-python-poetry-plugin?style=for-the-badge&logo=github)](https://github.com/reqstool/reqstool-python-poetry-plugin/issues)
[![License](https://img.shields.io/github/license/reqstool/reqstool-python-poetry-plugin?style=for-the-badge&logo=opensourceinitiative)](https://opensource.org/license/mit/)
[![Build](https://img.shields.io/github/actions/workflow/status/reqstool/reqstool-python-poetry-plugin/build.yml?style=for-the-badge&logo=github)](https://github.com/reqstool/reqstool-python-poetry-plugin/actions/workflows/build.yml)
[![Static Badge](https://img.shields.io/badge/Documentation-blue?style=for-the-badge&link=docs)](https://reqstool.github.io/reqstool-python-poetry-plugin/reqstool-python-poetry-plugin/0.0.2/index.html)

# Reqstool Python Poetry Plugin

> [!WARNING]
> Poetry plugin support is currently untested. Functionality may be broken. Contributions welcome.

## Description

This provides an application plugin for Poetry that runs during `poetry build` and `poetry install`.

The plugin collects decorated code, formatting it and writing it to a annotations.yml file saved to the `build/reqstool/` folder, utilizing the `reqstool-python-decorators` package for the processing.


## Installation

### Plugin

The package name is `reqstool-python-poetry-plugin`.

* Using poetry:

```
$poetry add reqstool-python-poetry-plugin
```

* pip install (unsure if working as intended):

```
$pip install reqstool-python-poetry-plugin
```

### Dependencies

#### reqstool-decorators

The plugin reads decorators available in the `reqstool-python-decorators` package.

```
$pip install reqstool-python-decorators
```

pyproject.toml

```
[tool.poetry.dependencies]
reqstool-python-decorators = "<version>"
```

### Configuration

The plugin is configured in the `pyproject.toml` file.

```toml
[tool.reqstool]
sources = ["src", "tests"]
test_results = ["build/**/junit.xml"]
dataset_directory = "docs/reqstool"
output_directory = "build/reqstool"
```

This specifies where the plugin should be applied: `sources`, where test reports are located: `test_results`, where reqstool files are located: `dataset_directory` and output directory: `output_directory`.


## Usage

### Decorators

Used to decorate your code as seen in the examples below, the decorator processing that runs during the build process collects data from the decorated code.

Import decorators:

```
from reqstool-python-decorators.decorators.decorators import Requirements, SVCs
```

Example usage of the decorators:

```
@Requirements("REQ_111", "REQ_222")
def somefunction():
```

```
@SVCs("SVC_111", "SVC_222")
def test_somefunction():
```

### Poetry build

When running `$poetry build` the plugin will generate an `annotations.yml` file in the `build/reqstool/` folder and a `reqstool_config.yml` in the project root (which is bundled into the sdist and then removed after the build completes).

When running `$poetry install` the plugin will update `pyproject.toml` with the necessary `include` entries for reqstool files.



## License

This project is licensed under the MIT License - see the LICENSE.md file for details.
