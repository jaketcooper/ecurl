# ecurl — Encoded cURL for Penetration Testing

![Version](https://img.shields.io/badge/version-1.0.0-blue)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
![Shell](https://img.shields.io/badge/language-bash-blue.svg)
![Status](https://img.shields.io/badge/build-passing-brightgreen.svg)
![Platform](https://img.shields.io/badge/platform-linux%20%7C%20unix-lightgrey.svg)
![Kali Ready](https://img.shields.io/badge/kali--ready-yes-critical.svg)

---

## 🔍 Overview
**ecurl** is an encoded wrapper around [`curl`](https://curl.se/) for **authorized penetration testing** and **security automation**.  
It simplifies payload encoding, session persistence, batch payload testing, and JSON-based chaining — all while keeping full `curl` flexibility. _(just add an 'e'!)_

The tool is lightweight, self-contained (Bash), and tested under **Debian**, **Ubuntu**, and **Kali Linux**.

> ⚠️ Use responsibly. Only perform security testing on systems you have explicit permission to test.

---

## 🚀 Key Features

| Category | Description |
|-----------|-------------|
| **Encoding** | URL, double, HTML, Base64, Unicode |
| **Session Management** | Persistent cookies, replay, export/import |
| **Batch Testing** | Run payloads from file with delays/threads |
| **JSON Output** | Base64-safe structured output for pipelines |
| **Proxy & TLS** | Full proxy, client cert, and SSL control |
| **Error Handling** | Granular exit codes, robust curl wrappers |
| **Colorized Output** | Clear status display with ANSI detection |
| **Forensics Ready** | History logging to `~/.ecurl_history` |

---

## ⚙️ Installation

### From Source
```bash
git clone https://github.com/jaketcooper/ecurl.git
cd ecurl
sudo make install
````

### From Debian/Kali Package

*(Recommended once released to Kali repos)*

```bash
sudo apt install ecurl
```

This installs:

```
/usr/bin/ecurl
/usr/share/man/man1/ecurl.1.gz
/usr/share/doc/ecurl/*
```

Dependencies are automatically resolved (`curl`, `perl`, `jq`, `liburi-perl`).

---

## 🧠 Usage

```bash
ecurl [OPTIONS]

  -i, --injection <TEXT>    Payload to encode and send
  -t, --target <URL>        Persistent target
  -c, --count <N>           Number of encoding passes
  --encode-type <TYPE>      url, html, base64, unicode
  --json                    Output as JSON (for chaining)
  --payload-file <FILE>     Batch payload testing
  --session <NAME>          Named cookie persistence
  --replay <FILE>           Replay a saved request
  -s, --show                Show encoded payload only
  --version                 Display version
```

### Example 1: Basic Injection

```bash
ecurl -t "https://target/api?id=" 
ecurl -i "' OR '1'='1"
```

### Example 2: Batch Test Payloads

```bash
ecurl --payload-file payloads.txt -c 2 --delay 1
```

### Example 3: JSON Chaining

```bash
ecurl -i test --json | jq '.response.status'
```

---

## 🧩 Encoding Example

| Type    | Input        | Output               |
| ------- | ------------ | -------------------- |
| url     | `' OR 1=1--` | `%27%20OR%201%3D1--` |
| html    | `<script>`   | `&lt;script&gt;`     |
| base64  | `abc123`     | `YWJjMTIz`           |
| unicode | `A`          | `\u0041`             |

---

## 🪄 Advanced Features

* **Replay Mode** — export and rerun full requests
* **Tamper Scripts** — custom transforms before sending
* **Grep & Match Filters** — highlight or extract response fragments
* **Threaded Batch Mode** — queue large payload sets efficiently

---

## 🧰 Developer Notes

* Written in **pure Bash**, portable across most POSIX shells
* Error-resistant: no unguarded `eval`, no silent failures
* Fully compatible with Kali's **Debhelper 12+** build system

For deeper architectural details, see:

* [`TECHNICAL.md`](TECHNICAL.md)
* [`PACKAGING.md`](PACKAGING.md)

---

## 🤝 Contributing

Contributions, patches, and packaging improvements are welcome.
Please see [`CONTRIBUTING.md`](CONTRIBUTING.md) and adhere to the [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md).

---

## 🛡️ Security Policy

See [`SECURITY.md`](SECURITY.md) for responsible disclosure and vulnerability reporting.

---

## 🧾 License

Licensed under the [MIT License](LICENSE).
© 2025 Jake Cooper

---

## 📦 Debian / Kali Packaging Status

`ecurl` is Debian Policy-compliant and structured for inclusion in **Kali Rolling**:

* `debian/control` defines dependencies (`curl`, `perl`, `jq`, `liburi-perl`)
* `debian/rules` uses standard `dh` build helper
* Man page: `/usr/share/man/man1/ecurl.1.gz`
* Lintian clean

For detailed packaging steps, refer to [`PACKAGING.md`](PACKAGING.md).
