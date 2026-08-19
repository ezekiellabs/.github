<div align="center">
  <img src="assets/owl.svg" alt="Ezekiel Labs" width="112" height="112">
  <h1>Ezekiel Labs</h1>
  <p><strong>Open-source red · blue · purple team tooling.</strong></p>

[![opseclint](https://img.shields.io/crates/v/opseclint?style=plastic&logo=rust&logoColor=white&label=opseclint)](https://crates.io/crates/opseclint)
[![Rust](https://img.shields.io/badge/Rust-000000?style=plastic&logo=rust&logoColor=white)](https://www.rust-lang.org)
[![MITRE ATT&CK](https://img.shields.io/badge/MITRE_ATT%26CK-C21D24?style=plastic)](https://attack.mitre.org)
[![Sigma](https://img.shields.io/badge/Sigma-1F6FEB?style=plastic)](https://sigmahq.io)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue?style=plastic)](https://github.com/ezekiellabs/.github/blob/main/LICENSE)

</div>

Ezekiel Labs builds practical security tools that live in the space between
offense and defense. Our tools tell you which actions are loud, which
detections fire, and exactly where the blind spots are.

Everything here is open source, self-contained where we can manage it, and
designed to drop into a CI pipeline or a purple-team engagement without ceremony.

## Tools

### <img src="assets/eye-mark.svg" alt="" width="22" height="22" align="top"> [opseclint](https://github.com/ezekiellabs/opseclint)

A detection-coverage analyzer for the command line. Point it at a command,
script, or playbook and it resolves each action to the MITRE ATT&CK technique(s)
it implements, the host telemetry it emits, and the detections that would fire
with a 0–100 detectability score. _"What would a defender see?"_

```sh
cargo install opseclint
```

_More on the way — see the
[roadmap](https://github.com/ezekiellabs/.github/blob/main/ROADMAP.md)._

## What we're about

- **Honesty over everything.** Absence of a finding is never proof of stealth.
- **Purple by default.** Red and blue are two readings of the same event.
- **Open and self-contained.** MIT-licensed.

Written down at greater length in
[GOVERNANCE.md](https://github.com/ezekiellabs/.github/blob/main/GOVERNANCE.md).

## Contributing

[Contributing](https://github.com/ezekiellabs/.github/blob/main/CONTRIBUTING.md)
·
[Code of Conduct](https://github.com/ezekiellabs/.github/blob/main/CODE_OF_CONDUCT.md)
·
[Security](https://github.com/ezekiellabs/.github/blob/main/SECURITY.md)
·
[Support](https://github.com/ezekiellabs/.github/blob/main/SUPPORT.md)

These are organization-wide defaults; a repository with its own copy supersedes
them.

---

<sub>Named after a kid with sharp eyes. Developed in Omaha.</sub>
