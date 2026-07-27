# Ezekiel Labs

**Open-source red · blue · purple team tooling. Built to show you what a defender would actually see.**

Ezekiel Labs builds practical security tools that live in the space
between offense and defense. Details which actions are loud, which 
detections fire, and exactly where the blind spots are.

Everything here is open source, self-contained where we can manage it, and
designed to drop into a CI pipeline or a purple-team engagement without ceremony.

## Tools

### 🛡️ [opseclint](https://github.com/ezekiellabs/opseclint)
A detection-coverage analyzer for the command line. Point it at a command,
script, or playbook and it resolves each action to the MITRE ATT&CK technique(s)
it implements, the host telemetry it emits, and the detections that would fire
with a 0–100 detectability score. *"What would a defender see?"*

```sh
cargo install opseclint
```

*More on the way.*

## What we're about

- **Honesty over theater.** A tool that says "you're covered" had better be right. Absence of a finding is never proof of stealth.
- **Purple by default.** Red and blue are two readings of the same event. Our tools speak both.
- **Open and self-contained.** MIT-licensed.

---

<sub>Named after a kid with sharp eyes. Developed in Omaha.</sub>