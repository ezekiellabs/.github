<!--
Organization-wide default. No Ezekiel Labs repository currently ships its own
SUPPORT.md, so this is what every repository shows under "Support".

Links here must be absolute — relative links in a default community health file
resolve against ezekiellabs/.github, not the repository the reader is in.
-->

# Support

Where to take a given question, across every
[Ezekiel Labs](https://github.com/ezekiellabs) repository:

| You have | Go to |
|---|---|
| A question, an idea, or something to show off | That repository's **Discussions** |
| A bug — wrong output, a crash, a broken install | That repository's **Issues**, using the bug template |
| A gap in detection coverage | That repository's **Issues**, using the coverage-request template if it has one |
| A security vulnerability | The repository's **Security** tab → *Report a vulnerability*. Never a public issue — see the [security policy](https://github.com/ezekiellabs/.github/blob/main/SECURITY.md) |

## Before you file

- Check whether the repository's `CHANGELOG.md` already mentions it. These
  projects move quickly and the fix may be unreleased.
- Include the version (`--version` on a CLI) or the commit SHA. "Latest" is not
  a version — it changes.
- For a wrong result, include the input that produced it. A tool that maps
  actions to detections is very hard to debug from a description alone.

## What we do not support

These are open-source tools maintained in the open by one person. There is no
SLA, no paid tier, and no private support channel other than the security
reporting path above. Response times are best effort.

A knowledge base or rule reference being wrong is a **correctness bug** and we
want to hear about it — that is the most valuable kind of report we get.
