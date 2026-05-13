# Task
The xFusionCorp Industries ML team enforces code quality on every commit via pre-commit. A draft .pre-commit-config.yaml exists in the git repository at /root/code/fraud-detection/, but it does not match the team's standard and pre-commit run --all-files fails against it. Correct the configuration.

# Fix
```
# Before fix
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v2.3.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check_yaml

  - repo: https://github.com/charliermarsh/ruff-pre-commit
    rev: v0.1.0
    hooks:
      - id: ruff-lint

  - repo: https://github.com/psf/black-pre-commit-mirror
    hooks:
      - id: black

```
```
# After Fix
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.5.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml

  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.3.0
    hooks:
      - id: ruff

  - repo: https://github.com/psf/black-pre-commit-mirror
    rev: 24.2.0
    hooks:
      - id: black
```

```
pre-commit install
pre-commit run --all-files
```

# What is pre-commit
- pre-commit is a Python-based framework used to manage Git Hooks. Git Hooks are scripts that Git is designed to run automatically when certain actions happen (like commit, push, or merge). Without this tool, you’d have to manually write shell scripts and put them in a hidden folder (.git/hooks/). pre-commit makes this much easier by letting you use a simple configuration file.
- We need to install this tool.

```
brew install pre-commit

# or
pip install pre-commit
```
- Create the configuration file `.pre-commit-config.yaml` file.

```
# After Fix
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.5.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml

  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.3.0
    hooks:
      - id: ruff

  - repo: https://github.com/psf/black-pre-commit-mirror
    rev: 24.2.0
    hooks:
      - id: black
```
- Register the tool with git. Inside your project folder we need to run. This command "hooks" the tool into your local Git settings.
```
pre-commit install
```
- How it works?
1.  **You stage files:** `git add src/data/process_data.py`
2.  **You try to commit:** `git commit -m "Add data processing"`
3.  **The Trigger:** Before the commit is actually saved, Git pauses and calls the `pre-commit` tool.
4.  **The Inspection:** `pre-commit` reads your `.yaml` file, downloads the necessary tools (Ruff, Black, etc.), and runs them against your **staged files only**.
5.  **The Result:**
    *   **Pass:** If everything is clean, the commit finishes normally.
    *   **Fail:** If Black finds a formatting error or Ruff finds a bug, the commit is **blocked**. The tool will often fix the file for you, and then you just `git add` the fix and try the commit again.