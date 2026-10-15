## Python

### Ruff

#### Workflow

```sh
# Local dev / pre-commit
ruff check src --fix
ruff format src

# CI (no writes, fail on issues)
ruff check src
ruff format src --check
```

#### Global Install

```sh
pixi global install ruff
```

#### Check Code

Lint only — reports issues without changing anything

```sh
ruff check src
```

Lint + auto-fix — applies safe fixes for issues Ruff can resolve automatically

```sh
ruff check src --fix
```

#### Format Code

Format — like Black, reformats code to a consistent style

```sh
ruff format src
```
