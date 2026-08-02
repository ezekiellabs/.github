<!--
Organization-wide default.

Links here must be absolute — relative links in a default community health file
resolve against ezekiellabs/.github, not the repository the reader is in.
-->

# Governance

[Ezekiel Labs](https://github.com/ezekiellabs) is maintained by Garrett Allen
([@Gerrrt](https://github.com/Gerrrt)). Decisions are made by the maintainer,
in the open, on the relevant issue or pull request.

## How changes land

Default branches are protected. Everything — including the maintainer's own work
— goes through a pull request with CI green before it merges. Releases are cut
from the default branch and tagged; each repository documents its own release
procedure.

## What belongs here

A project belongs in Ezekiel Labs if it is practical tooling in the space
between offense and defense: something that tells you which actions are loud,
which detections fire, and where the blind spots are. Open source, MIT, and
self-contained enough to drop into a CI pipeline or a purple-team engagement
without ceremony.

Three commitments shape what gets accepted:

- **Honesty over everything.** Absence of a finding is never proof of stealth. A
  tool that abstains when it cannot know something beats one that guesses, and
  any change that makes a tool more confident has to bring the evidence that
  earns it.
- **Purple by default.** Red and blue are two readings of the same event. Our
  tools describe *detectability* — what a defender would observe. They do not
  recommend evasions, and contributions that add "how to be quieter" guidance
  are out of scope. This is a scope boundary, applied uniformly.
- **Open and self-contained.** MIT-licensed, minimal dependencies, no phoning
  home, no required accounts.

## Becoming a maintainer

There is no formal process yet, because there is one maintainer. Sustained,
high-quality contribution to a repository is how it would start; ask on a
discussion if you are interested.
