# ecurl Packaging Guide (Debian / Kali Linux)

This document describes how to build, lint, and submit **ecurl** for inclusion in Debian and Kali repositories.

---

## 📦 1. Package Layout

````

ecurl/
├── src/
│   ├── usr/bin/ecurl
│   ├── debian/
│   │   ├── control
│   │   ├── rules
│   │   ├── changelog
│   │   ├── copyright
│   │   ├── ecurl.manpages
│   │   ├── install
│   │   └── source/format
│   └── usr/share/man/man1/ecurl.1

````

---

## ⚙️ 2. Building the Package

### Dependencies
```bash
sudo apt install devscripts debhelper lintian build-essential
````

### Build Command

```bash
cd src
dpkg-buildpackage -us -uc
```

Artifacts are generated in the parent directory (`../ecurl_<version>_all.deb`).

---

## 🧩 3. Debian Control File Example

```text
Source: ecurl
Section: utils
Priority: optional
Maintainer: Jake Cooper <jaketcooper@users.noreply.github.com>
Build-Depends: debhelper (>= 12)
Standards-Version: 4.6.0
Homepage: https://github.com/jaketcooper/ecurl

Package: ecurl
Architecture: all
Depends: ${shlibs:Depends}, ${misc:Depends}, bash, curl, perl, jq, liburi-perl
Description: Encoded cURL wrapper for penetration testing
 Ecurl is a Bash-based utility that simplifies encoded payload generation,
 request management, and batch penetration testing automation.
 Intended for authorized security research only.
```

---

## 🧱 4. Debian Rules (dh-based)

```makefile
#!/usr/bin/make -f
%:
	dh $@
```

---

## 🧾 5. Changelog Template

```text
ecurl (1.0.0-1) unstable; urgency=medium

  * Initial release for Debian and Kali.
  * Added man page and Lintian-clean packaging.

 -- Jake Cooper <jaketcooper@users.noreply.github.com>  Wed, 12 Nov 2025 12:00:00 -0500
```

---

## 🧠 6. Lintian & Policy Compliance

Run checks before submission:

```bash
lintian ../ecurl_1.0.0-1_all.deb
```

Ensure:

* No `bashism` warnings (use `shellcheck`).
* License file included (`/usr/share/doc/ecurl/copyright`).
* Manpage installed in `usr/share/man/man1/`.
* `README.Debian` added if behavior differs from upstream.

---

## 🧰 7. Kali / Debian Submission Steps

### Kali

1. Fork [`https://gitlab.com/kalilinux/packages`](https://gitlab.com/kalilinux/packages)
2. Add your repository under `utils/`
3. Submit a merge request with:

   * Source tarball
   * Signed `.dsc` and `.changes`
   * Lintian output
   * Verification build logs

### Debian Mentors Upload (Optional)

```bash
dput mentors ../ecurl_1.0.0-1_source.changes
```

---

## 🔐 8. Recommended Tests

| Test                | Command                                       |       |
| ------------------- | --------------------------------------------- | ----- |
| Dependency check    | `apt-cache depends ecurl`                     |       |
| Encoding regression | `ecurl -i "test" -c 3 -s`                     |       |
| Batch stability     | `ecurl --payload-file payloads.txt --delay 1` |       |
| JSON validation     | `ecurl -i test --json                         | jq .` |
| Replay test         | `ecurl --save-req --replay request.req`       |       |

---

## 📚 9. References

* [Debian Policy Manual](https://www.debian.org/doc/debian-policy/)
* [Debhelper Compatibility Levels](https://manpages.debian.org/debhelper)
* [Kali Packaging Guide](https://www.kali.org/docs/development/packaging/)
* [Keep a Changelog format](https://keepachangelog.com/)

---

**Maintainer:** Jake Cooper
**License:** MIT
**Version:** 1.0.0
**Status:** Lintian clean ✅

```

---

Would you like me to go ahead and draft the next core doc (`TECHNICAL.md`) next — or the `CHANGELOG.md` and `CONTRIBUTING.md` to round out the public-facing ones first?
```
