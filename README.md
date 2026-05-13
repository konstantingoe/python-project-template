# my_package: A Python project template for scientific paper writing

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Documentation](https://img.shields.io/badge/docs-mkdocs-blue)](https://github.com/your-org/your-repo)
[![Code style: ruff](https://img.shields.io/badge/code%20style-ruff-000000.svg)](https://github.com/astral-sh/ruff)
[![Tests](https://img.shields.io/badge/tests-pytest-brightgreen)](https://github.com/your-org/your-repo/actions)

> **This is a Python project template.** See the checklist below before developing.

---

## After cloning — checklist

Complete these steps before writing any code.

- [ ] **Rename the package directory** — rename `graph_models/` to your package name (e.g. `my_package/`)
- [ ] **Update `pyproject.toml`** — change `name`, `description`, `authors`, `maintainers`, `keywords`, `classifiers`, and the `[project.urls]` section
- [ ] **Update `[tool.hatch.version]`** — change `path` to point to your new package's `__init__.py`
- [ ] **Update `[tool.hatch.build.targets.wheel]`** — change `packages` to your new package name
- [ ] **Update `[tool.hatch.build.targets.sdist]`** — remove the `venv_graph_models/` entry and replace with your venv name if needed
- [ ] **Reset the version** — set `__version__` in `your_package/__init__.py` to `"0.1.0"` (or your starting version)
- [ ] **Update `mkdocs.yml`** — set `site_name`, `repo_url`, and `site_url`
- [ ] **Update `docs/index.md`** — replace the placeholder content with your package's overview and quick-start guide
- [ ] **Update `docs/reference.md`** — point the `:::` autodoc directives at your modules
- [ ] **Update this README** — replace all placeholder text, links, and the badges at the top
- [ ] **Update `.pre-commit-config.yaml`** — review hook versions and add/remove hooks as needed
- [ ] **Set up the remote** — `git remote set-url origin https://github.com/your-org/your-repo.git`
- [ ] **Create and push the initial commit** — `git add -A && git commit -m "chore: init from template" && git push -u origin main`

---

## Table of Contents

- [Overview](#overview)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Project Structure](#project-structure)
- [Testing](#testing)
- [Development](#development)
- [Recommended VS Code Extensions](#recommended-vs-code-extensions)
- [License](#license)

## Overview

<!-- TODO: replace with a description of your project -->

This repository ships a ready-to-use Python project skeleton with:

- **Hatchling** build backend and `pyproject.toml`-only configuration
- **uv** for fast, reproducible dependency management
- **ruff** for linting and formatting (Google-style docstrings enforced)
- **mypy** for static type checking (strict mode)
- **pytest + pytest-cov** for testing with coverage
- **pre-commit** hooks (ruff, mypy, bandit security scan, uv-based pip-compile on push)
- **MkDocs + Material** for documentation, auto-deployed to GitHub Pages via GitHub Actions

The example package (`graph_models`) provides an undirected-graph data structure
(`UGRAPH`) to illustrate the intended layout.  Replace it with your own code.

## Installation

### From source

```bash
git clone https://github.com/your-org/your-repo.git
cd your-repo
pip install -e .
```

### Development installation

```bash
git clone https://github.com/your-org/your-repo.git
cd your-repo
make sync-venv
```

`make sync-venv` creates a virtual environment, installs all pinned dev
dependencies via `uv pip sync`, and installs the package in editable mode.

## Quick Start

```python
from graph_models import UGRAPH  # replace with your package

g = UGRAPH(nodes=["A", "B", "C"], edges=[("A", "B"), ("A", "C")])
print(g.num_nodes)  # 3
print(g.num_edges)  # 2
print(g.adjacency_matrix)
```

## Project Structure

```
.
├── graph_models/          # → rename to your package
│   ├── __init__.py
│   └── graphs.py
├── tests/
│   └── test_graphs.py
├── docs/
│   ├── index.md
│   └── reference.md
├── notebooks/             # generate figures/tables for paper/
│   ├── figure_creator.ipynb
│   └── table_creater.ipynb
├── paper/                 # LaTeX source — swap template for your venue
│   ├── main.tex
│   ├── uai2026.cls        # → replace with your venue's class file
│   ├── library.bib        # BibTeX references
│   ├── sections/          # \input'd by main.tex
│   ├── figures/           # written by notebooks/
│   └── tables/            # written by notebooks/
├── pyproject.toml         # build, deps, tool config
├── Makefile               # sync-venv, requirements, test, precommit
├── requirements.txt       # pinned runtime deps (auto-generated)
├── requirements_dev.txt   # pinned dev deps (auto-generated)
├── mkdocs.yml
└── .pre-commit-config.yaml
```

### `paper/` and `notebooks/`

The `paper/` folder holds the LaTeX source for an accompanying manuscript, and
`notebooks/` holds the Jupyter notebooks that produce its figures and tables —
each notebook writes its output directly into `paper/figures/` or
`paper/tables/`, which are then pulled into the document via
`\includegraphics` and `\input`. Re-running a notebook is the canonical way to
refresh a result; the `.tex` skeleton stays put.

The shipped template targets **UAI 2026** (`paper/uai2026.cls`) purely as an
example. Replace it with whatever class file your publishing outlet requires
(NeurIPS, ICML, ACL, IEEEtran, Springer LNCS, …) — the figure/table workflow
in `notebooks/` is venue-agnostic and stays the same.

## Testing

```bash
pytest tests/ -v
```

With coverage report:

```bash
pytest tests/ --cov=graph_models --cov-report=html
```

Or via Make:

```bash
make test
```

## Development

### Managing dependencies

Add a dependency to `pyproject.toml`, then regenerate lock files:

```bash
make requirements        # re-pin without upgrading existing deps
make update-requirements # re-pin and upgrade all deps
make sync-venv           # apply the lock files to the venv
```

### Pre-commit hooks

```bash
pre-commit install          # enable hooks (run automatically on git commit/push)
pre-commit run --all-files  # run all hooks manually
```

### Building the documentation locally

```bash
mkdocs serve   # live preview at http://127.0.0.1:8000
mkdocs build   # static site in site/
```

### Deploying docs to GitHub Pages

The `.github/workflows/docs.yml` workflow builds the site with
`mkdocs build --strict` and publishes it to GitHub Pages on every push to
`main` (and on manual `workflow_dispatch`). No local steps are needed.

> One-time setup: in the repo, go to **Settings → Pages** and set
> **Source: GitHub Actions**. The first push to `main` after that will
> deploy the site.

### Linting and type checking

```bash
ruff check graph_models/     # linting
mypy graph_models/           # static type checking
```

## Recommended VS Code Extensions

The `.vscode/settings.json` shipped with this template is wired up for Ruff
formatting, Pylance type checking, and Jupyter notebooks. To get the full
experience, install the extensions below.

### Python core

| Extension | ID | Purpose |
| --- | --- | --- |
| Python | `ms-python.python` | Language support, interpreter selection, debugging |
| Pylance | `ms-python.vscode-pylance` | Fast type checker / language server (required by `python.analysis.*` settings) |
| Python Debugger | `ms-python.debugpy` | `debugpy`-based debugging |
| Mypy Type Checker | `ms-python.mypy-type-checker` | Inline `mypy` diagnostics (matches the `mypy --strict` config in `pyproject.toml`) |
| Python Environments | `ms-python.vscode-python-envs` | Manage the `venv_*` virtualenv created by `make sync-venv` |
| Ruff | `charliermarsh.ruff` | Lint + format on save (set as `defaultFormatter` for `[python]` and notebooks) |
| autoDocstring | `njpwerner.autodocstring` | Generate Google-style docstrings (matches the Ruff `pydocstyle` convention) |
| Python Type Hint | `njqdev.vscode-python-typehint` | Autocomplete for type annotations |

### Jupyter (for `notebooks/`)

| Extension | ID |
| --- | --- |
| Jupyter | `ms-toolsai.jupyter` |
| Jupyter Keymap | `ms-toolsai.jupyter-keymap` |
| Jupyter Notebook Renderers | `ms-toolsai.jupyter-renderers` |
| Jupyter Cell Tags | `ms-toolsai.vscode-jupyter-cell-tags` |
| Jupyter Slide Show | `ms-toolsai.vscode-jupyter-slideshow` |

### LaTeX (for `paper/`)

| Extension | ID | Purpose |
| --- | --- | --- |
| LaTeX Workshop | `james-yu.latex-workshop` | Build, preview, and SyncTeX |
| LaTeX Utilities | `tecosaur.latex-utilities` | Companion to LaTeX Workshop |
| LaTeX Snippets | `jeffersonqin.latex-snippets-jeff` | Math / environment snippets |
| LaTeX Support | `torn4dom4n.latex-support` | Extra syntax helpers |
| Unicode Latex | `oijaz.unicode-latex` | Type Unicode via LaTeX commands |
| LTeX | `valentjn.vscode-ltex` | Grammar / spell check for `.tex` and Markdown |

### Config files & tooling

| Extension | ID | Purpose |
| --- | --- | --- |
| Even Better TOML | `tamasfe.even-better-toml` | `pyproject.toml` editing |
| YAML | `redhat.vscode-yaml` | `mkdocs.yml`, `.pre-commit-config.yaml`, GitHub Actions |
| Markdown All in One | `yzhang.markdown-all-in-one` | Authoring `docs/` and this README |
| markdownlint | `davidanson.vscode-markdownlint` | Markdown linting |
| Makefile Tools | `ms-vscode.makefile-tools` | Run targets from the `Makefile` |
| Rainbow CSV | `mechatroner.rainbow-csv` | Inspect tabular data |
| Dependi | `fill-labs.dependi` | Surface outdated versions in `requirements*.txt` |
| GitHub Actions | `github.vscode-github-actions` | Workflow editing and run status |
| GitHub Pull Requests | `github.vscode-pull-request-github` | Review PRs in-editor |

### Quality-of-life

| Extension | ID |
| --- | --- |
| GitLens | `eamodio.gitlens` |
| Error Lens | `usernamehw.errorlens` |
| indent-rainbow | `oderwat.indent-rainbow` |
| Trailing Spaces | `shardulm94.trailing-spaces` |

> Tip: add the IDs above to `.vscode/extensions.json` under `"recommendations"`
> so VS Code prompts new contributors to install them on first open.

## License

MIT License — see [LICENSE](LICENSE) for details.
