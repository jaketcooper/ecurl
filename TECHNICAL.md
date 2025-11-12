# ecurl Technical Documentation

Version: 1.0.0  
Author: Jake Cooper  
License: MIT  

---

## Overview

`ecurl` is a self-contained Bash utility that wraps around `curl` to provide advanced encoding, payload injection, and automation features for security testing.

Its architecture emphasizes:
- **Safety** (no unguarded `eval`)
- **Transparency** (verbose, colorized logs)
- **Extensibility** (batching, tamper scripts, replay)

---

## Architecture Diagram (Conceptual)

```

[ User Input ]
↓
[ Argument Parser ]
↓
[ Encoding Engine ]
↓
[ Request Builder ] ———→ [ cURL Wrapper ]
↓                            ↓
[ JSON Formatter ] ←—————— [ Response Parser ]
↓
[ History / Session Manager ]

```

---

## Internal Module Map

| Subsystem | Function | Purpose | Lines (approx.) |
|------------|-----------|----------|----------------|
| **Core Entrypoint** | `main()` | Central argument dispatch and mode control | L1000–1600 |
| **Dependency Check** | `check_dependencies()` | Ensures required binaries and Perl modules are present | L200–250 |
| **Encoding Engine** | `apply_encoding()`, `url_encode()`, `html_encode()`, `base64_encode()`, `unicode_encode()` | Handles all supported encoding types | L260–370 |
| **Command Builder** | `build_curl_command()` | Constructs `curl` array with flags, headers, and options | L380–550 |
| **Execution Layer** | `execute_curl()` | Runs prepared command with error traps and JSON output | L560–770 |
| **Batch System** | `batch_test_payloads()` | Iterates payloads from file with threading/delay options | L780–890 |
| **Persistence Layer** | `save_request()`, `replay_request()` | Saves and replays JSON-based request templates | L900–970 |
| **Config Management** | `export_config()`, `import_config()` | JSON export/import of current state | L980–1030 |
| **History System** | `save_history()`, `show_history()`, `clear_history()` | Audit trail of executed commands | L1040–1100 |
| **Error Handling** | `error_exit()` | Unified exit codes with message coloring | L190–220 |
| **Output Layer** | `log_message()` | Structured, colorized output | L150–190 |

---

## Encoding Pipeline

Each payload is transformed via:
1. **Normalization** — clean trailing whitespace and line breaks  
2. **Encoding Passes** — apply one or multiple encoders  
3. **Output Transformation** — substitute HTML or Unicode if configured  
4. **Dry-run Mode** — display encoded payload with `--show`  

Encoders:
- `url_encode`: `perl -MURI::Escape`
- `html_encode`: `sed` entity replacements
- `base64_encode`: native `base64`
- `unicode_encode`: Perl ordinal conversion

All encoders preserve payload length tracking for logging.

---

## JSON Output Structure

When `--json` is enabled, each request yields:

```json
{
  "timestamp": "2025-11-12T17:20:41Z",
  "request": {
    "payload": "1' OR '1'='1",
    "url": "https://target.com/api?id="
  },
  "response": {
    "status": 200,
    "time": 0.31,
    "size": 4162,
    "body": "<HTML...>"
  }
}
````

* Response body is Base64-safe
* Metadata validated with `jq` before emission

---

## Error Model

| Code | Constant        | Meaning                  |
| ---- | --------------- | ------------------------ |
| 0    | `E_SUCCESS`     | Success                  |
| 1    | `E_GENERAL`     | Generic failure          |
| 2    | `E_MISSING_ARG` | Missing argument         |
| 3    | `E_INVALID_ARG` | Invalid argument         |
| 4    | `E_MISSING_DEP` | Required command missing |
| 5    | `E_CURL_ERROR`  | cURL failure             |
| 6    | `E_FILE_ERROR`  | File missing/unreadable  |

All handled centrally by `error_exit()`.

---

## File Persistence

| File                 | Description                   |
| -------------------- | ----------------------------- |
| `~/.ecurl_target`    | Stored persistent target      |
| `~/.ecurl_cookies`   | Default cookie jar            |
| `~/.ecurl_session.*` | Session-specific cookie files |
| `~/.ecurl_history`   | Command history               |

---

## Design Principles

1. **Security-first** — no unescaped user inputs executed directly.
2. **Predictable output** — identical output formats for automation.
3. **User-responsibility emphasis** — includes clear warning banners.
4. **Self-contained** — no external libs, minimal Bash assumptions.

---

## Future Extensions

* Asynchronous batch queue with GNU parallel
* Interactive payload generator
* Tamper scripting framework in Python/Perl
* REST API bridge for integration with CI/CD
