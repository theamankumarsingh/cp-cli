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
- automated testing against AtCoder and Codeforces samples,
- keep your local scripts up to date.

## Features

- `contest`: create contest folders (and automatically navigating into them) or instantly touch files with templates.
- `runat` / `runcf`: standardized testing with automatic sample downloads.
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
python3 -c "f='.cp-cli/bin/activate'; c=open(f).read(); open(f,'w').write(c.replace('unset -f deactivate', 'unset -f contest\n        unset -f deactivate'))" && \
echo '
contest() {
    command contest "$@"
    if [ -n "$1" ] && [ "$1" != "touch" ] && [ -d "$1" ]; then
        cd "$1"
    fi
}' >> .cp-cli/bin/activate && \
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
# Activate environment
source .cp-cli/bin/activate

# Scaffold 6 files and auto-cd
contest abc340 6

# (Solve A.py)

# Download samples & test
runat A

# Generate write-up
writeup
```

## Commands

### `cp-cli`
Toolkit manager command:
- `cp-cli` shows version, root, available scripts, and maintenance commands.
- `cp-cli upgrade` checks remote version and performs a **clean sync** (adds new scripts and removes deprecated residuals).

### `contest`
Create contest folder and Python solution files.
```bash
contest <contest_name> [count|files...]
contest touch <file1> [file2...]
```
- **Auto-cd:** Automatically switches your terminal to the new contest folder.
- **Templates:** New files are populated with your default template.
- **Safety:** `touch` will skip existing files to avoid overwriting your code.

### `runat` / `runcf`
Automated testing using `online-judge-tools`.
- **Automatic Context:** Infers the contest ID from your current directory name.
    - `runat`: Uses the full folder name (e.g., `abc340`).
    - `runcf`: Extracts the numeric ID from the folder name (e.g., `CF1927` -> `1927`).
- **Download & Test:** Downloads samples to `Tests/<filename_stem>` if missing.
- **Fast Mode:** If the `Tests/` folder for that problem exists, it skips downloading.
- **Force Mode:** Use `runat force A` to delete local tests and re-download.
- **URL Mode:** Provide a full URL as the first argument to test exactly one file.

```bash
runat A B                # Tests A.py and B.py (Context inferred from folder)
runcf A                  # Tests A.py (Context inferred from folder)
runat force A            # Wipes Tests/A and re-downloads samples
runat <URL> A            # Tests A.py against a specific problem URL
```
*Note: Since the toolkit environment includes `online-judge-tools`, you can also use the `oj` command directly for advanced tasks (e.g., `oj s` to submit).*

### `writeup`
Generate or update a local `README.md` in your contest folder to store problem approaches and complexities.
- Scans for `.py` files, preserves existing notes, and removes sections for deleted files.

### `template`
Manage reusable templates in `.cp-cli/templates`.
- `template add <name>`: opens `$EDITOR` to create a template.
- `template set <name>`: symlinks the template as the default for `contest`.

### `complexity`
Evaluate or plot complexity expressions.
```bash
complexity "n^2 + log(n)" 100000
complexity "n*log(n)" sl
```

## Updating

```bash
source .cp-cli/bin/activate
cp-cli upgrade
```

## Uninstall

From your project root:
```bash
deactivate 2>/dev/null || true
rm -rf .cp-cli
```

## Community

We welcome meaningful improvements! If you want to contribute, open an issue or submit a pull request.

## Contributors

[![Contributors](https://contrib.rocks/image?repo=theamankumarsingh/cp-cli)](https://github.com/theamankumarsingh/cp-cli/graphs/contributors)
