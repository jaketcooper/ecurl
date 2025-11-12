# Changelog

All notable changes to this project will be documented here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/).

---

## [1.0.0] - 2025-11-12
### Added
- Initial public release
- Full Bash implementation with modular encoding engine
- Support for URL, HTML, Base64, and Unicode encodings
- Persistent target and session storage
- Batch testing mode with delay and threading
- JSON output for integration with `jq`
- Replay and export/import functionality
- Colorized CLI with verbosity and quiet options
- Debian/Kali packaging (lintian clean)
- Comprehensive man page and docs

### Security
- Added strict argument validation and exit codes
- No unsafe `eval` usage
- Sanitized filename and session name handling

---

## [Unreleased]
### Planned
- Parallel batch execution
- Plugin-based tamper modules
- Richer output formatting for pipelines
- Kali upstream merge request
