# Contributing to ecurl

We welcome contributions to improve functionality, stability, and packaging quality.

---

## Development Setup

### Clone and Prepare
```bash
git clone https://github.com/jaketcooper/ecurl.git
cd ecurl
````

### Dependencies

Ensure the following are installed:

```bash
sudo apt install curl perl jq liburi-perl shellcheck
```

---

## Build and Test

### Manual Run

```bash
bash src/usr/bin/ecurl --help
```

### Local Install

```bash
sudo make install
```

### Debian Package Build

```bash
cd src
dpkg-buildpackage -us -uc
```

---

## Code Style

* Use **POSIX-compatible Bash** (`#!/bin/bash`)
* Avoid `eval` and unquoted variables
* Follow [Google Shell Style Guide](https://google.github.io/styleguide/shellguide.html)
* Use `set -o pipefail` and `local` for all functions

### Linting

```bash
shellcheck src/usr/bin/ecurl
```

---

## Commit Guidelines

* Use clear, atomic commits.
* Prefix messages by type:

  * `feat:` new feature
  * `fix:` bug or patch
  * `docs:` documentation
  * `refactor:` internal code cleanup
  * `test:` test improvements

Example:

```
feat(batch): added --threads for parallel execution
```

---

## Branch Workflow

| Branch           | Purpose            |
| ---------------- | ------------------ |
| `main`           | stable release     |
| `dev`            | active development |
| `feature/<name>` | feature branches   |
| `hotfix/<name>`  | urgent fixes       |

Pull requests must merge cleanly into `dev`.

---

## Testing Guidelines

Use realistic URLs and sanitized payloads:

```bash
ecurl -i "1' OR '1'='1" -u "https://example.com?id="
```

Batch tests:

```bash
ecurl --payload-file tests/payloads.txt --delay 1
```

All PRs should include test evidence or minimal reproduction cases.

---

## Documentation

Update or extend:

* `README.md`
* `TECHNICAL.md`
* `CHANGELOG.md`

Keep docs synced with implemented flags.

---

## Security Reporting

Do **not** open public issues for vulnerabilities.
Use the private contact in [`SECURITY.md`](SECURITY.md).

---

## Contributor Recognition

All significant contributors are listed in project metadata.
Maintainers reserve the right to reformat PRs for style consistency.

---

*Maintainer: Jake Cooper*
*License: MIT*
