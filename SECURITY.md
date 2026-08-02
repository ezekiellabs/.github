<!--
Organization-wide default. A repository that ships its own SECURITY.md
overrides this — opseclint does, because it carries a scope note specific to
static analysis that must not be generalized across the org.

Links here must be absolute — relative links in a default community health file
resolve against ezekiellabs/.github, not the repository the reader is in.
-->

# Security Policy

This policy applies to every repository in the
[Ezekiel Labs](https://github.com/ezekiellabs) organization that does not
publish one of its own.

## Reporting a vulnerability

Please **do not** open a public issue for security problems.

Use GitHub's private vulnerability reporting: on the affected repository, open
the **Security** tab → **Report a vulnerability**. That creates a private
advisory visible only to the maintainer.

Please include:

- the affected version or commit,
- a description of the issue,
- steps to reproduce, and
- the impact you foresee.

You can expect an initial response within a few days.

## Supported versions

Ezekiel Labs projects follow [Semantic Versioning](https://semver.org/). Fixes
land on the default branch and in the next release on the current major. Older
majors are not backported — check the individual repository for its release
history.

## A note on what our tools are

Ezekiel Labs builds detection and coverage tooling. Our tools describe what a
defender would observe; they do not recommend evasions, and they do not execute
the commands, scripts, or playbooks they analyze. Their knowledge bases and rule
references are informational and should be validated against your own detection
stack before you rely on them. A knowledge base being wrong about a detection is
a correctness bug — please report it as a normal issue, not a vulnerability.
