# cp-cli

`cp-cli` is a lightweight toolkit for **Python competitive programming** workflows.

## License And Disclaimer

This project is licensed under the Apache License 2.0.
See `LICENSE` for the full text.

This software is provided on an **"AS IS"** basis, without warranties or conditions of any kind, either express or implied.

## What It Does

It helps you:

- scaffold contest/problem files quickly,
- manage reusable code templates,
- evaluate and plot time-complexity expressions,
- generate and manage problem write-ups,
- keep your local scripts up to date.

## Features

- `contest`: create contest folders or instantly touch files with templates.
- `template`: add/list/set/remove templates with a default template symlink.
- `complexity`: evaluate and plot expressions like `n^2 + log(n)` in terminal.
- `writeup`: scaffold and sync a local `README.md` for problem notes and complexity analysis.
- `cp-cli`: show toolkit info and perform upgrades.

## Requirements

- Linux/macOS shell
- Python 3.8+ (required)
- `curl`, `tar`
- Internet access (for install/upgrade)

## Installation

Run from your project root:

```bash
python3 -m venv .cp-cli && \
mkdir -p .cp-cli/{data,templates} && \
curl -fsSL https://github.com/theamankumarsingh/cp-cli/archive/refs/heads/main.tar.gz \
| tar -xz \
    -C .cp-cli \
    --strip-components=1 \
    cp-cli-main/scripts \
    cp-cli-main/LICENSE \
    cp-cli-main/README.md && \
chmod +x .cp-cli/scripts/* && \
./.cp-cli/bin/python3 ./.cp-cli/scripts/cp-cli force-upgrade && \
echo 'export PATH="$VIRTUAL_ENV/scripts:$PATH"' >> .cp-cli/bin/activate && \
{ grep -qxF '.cp-cli/' .gitignore 2>/dev/null || echo '.cp-cli/' >> .gitignore; } && \
echo -e '\n\033[92m✔ cp-cli installed successfully!\033[0m' && \
echo -e 'Run: \033[94msource .cp-cli/bin/activate\033[0m to start.'
```

Activate the environment whenever you want to use the commands:

```bash
source .cp-cli/bin/activate
```

## Quick Start

```bash
source .cp-cli/bin/activate
cp-cli
contest ABC123 5
writeup
template add fastio
template set fastio
complexity "n^2 + log(n)" 100000
complexity "n*log(n)" safeline
```

## Commands

### `cp-cli`

Toolkit manager command:

- `cp-cli` shows version, root, available scripts, and maintenance commands.
- `cp-cli upgrade` or `cp-cli update` checks remote version and performs a **clean sync** (adds new scripts, updates existing ones, and removes deprecated residuals).
- `cp-cli force-upgrade` or `cp-cli force-update` forces a full script sync and refreshes Python dependencies (e.g., `plotext`) even if the version hasn't changed.

### `contest`

Create contest folder and Python solution files.

Usage:

```bash
contest <contest_name>
contest <contest_name> <count>
contest <contest_name> <file1> <file2> ...
contest touch <file1> [file2...]
```

Examples:

```bash
contest ABC123
# Creates: ABC123/A.py

contest ABC123 5
# Creates: A.py, B.py, C.py, D.py, E.py

contest ABC123 x y z
# Creates: x.py, y.py, z.py

contest touch helper logic
# Instantly creates helper.py and logic.py in the current directory
```

Template behavior:
- If `templates/default` or `templates/default.py` exists, it is used.
- Otherwise, a fallback Python starter file is generated.

### `writeup`

Generate or update a local README.md in your contest folder to store problem approaches and time/space complexities.

Usage:

```bash
writeup
```

Behavior:
- Scans the current directory for .py solution files.
- Creates a README.md if it doesn't exist, initializing it with a problem count and a template for each .py file.
- Adds a clean Markdown template (### Approach, ### Complexity) for newly created problems.
- Safely preserves any existing notes you've already written.
- Automatically cleans up (removes) sections for .py files that have been deleted from the directory.

### `template`

Manage reusable templates in `.cp-cli/templates`.

Usage:

```bash
template list
template add <name>
template rm <name>
template set <name>
```

Notes:
- `template add <name>` opens `$EDITOR` (defaults to `nano`) to edit/create `<name>.py`.
- The first valid template added is auto-set as default.
- Default template is stored as symlink: `.cp-cli/templates/default.py`.

### `complexity`

Evaluate or plot complexity expressions.

Usage:

```bash
complexity "<expression>" <value>
complexity "<expression>" [safeline|sl]
```

Examples:

```bash
complexity "n^2 + log(n)" 100
complexity "sqrt(n) * log(n)"
complexity "n*log(n)" sl
```

Expression notes:
- Use quotes around expressions.
- `^` is treated as exponent (`**`).
- Supports: `sqrt`, `log` (base 2), `log10`, `log2`, `exp`, `pow`, `ceil`, `floor`, `abs`, `pi`, `e`.
- Supports custom log-base shorthand like `log3(n)`.

Result classification (eval mode):
- order <= 6: Safe
- order == 7: Risky
- order > 7: TLE

## Updating

```bash
source .cp-cli/bin/activate
cp-cli upgrade
```

Use `cp-cli force-upgrade` to re-sync scripts and dependencies even if versions match.

## Uninstall

From your project root:

```bash
deactivate 2>/dev/null || true
rm -rf .cp-cli
```

Optional: remove the `.cp-cli/` line from `.gitignore` if you do not need it anymore.

## Community

We welcome meaningful improvements to this repository, including bug fixes, better documentation, performance enhancements, and new ideas that help competitive programmers.

If you want to contribute, open an issue to discuss your idea or submit a pull request directly. Open-source collaboration is encouraged and appreciated.

## Contributors

[![Contributors](https://contrib.rocks/image?repo=theamankumarsingh/cp-cli)](https://github.com/theamankumarsingh/cp-cli/graphs/contributors)
