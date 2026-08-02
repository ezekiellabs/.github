<!--
Organization-wide default. A repository that ships its own CONTRIBUTING.md
overrides this — opseclint does, because its guide documents a knowledge-base
entry schema that has no org-level equivalent.

Links here must be absolute — relative links in a default community health file
resolve against ezekiellabs/.github, not the repository the reader is in.
-->

# Contributing to Ezekiel Labs

Thanks for your interest. This is the organization-wide default; a repository
with its own `CONTRIBUTING.md` supersedes it, and that one is the guide to
follow.

## The shape of a contribution

1. Fork the repository and branch from the default branch.
2. Make the change. Match the surrounding code — its naming, its comment
   density, its idiom.
3. Run whatever gates that repository's CI enforces, locally, before opening the
   pull request. Every repository documents them in its README or its own
   contributing guide.
4. Open a pull request. Default branches are protected; CI has to go green
   before anything merges.
5. Note user-facing changes in `CHANGELOG.md` under `## [Unreleased]` if the
   repository keeps one.

## Scope

Ezekiel Labs tools describe **detectability** — what a defender would see. They
do not recommend evasions. Pull requests that add "how to be quieter" or
"how to defeat this detection" guidance are out of scope and will not be merged.
This is a project boundary, not a judgment about the contribution.

Being honest about limits is part of the product: a tool that abstains when it
cannot know something is more useful than one that guesses. Contributions that
make a tool *more* confident should come with the evidence that earns it.

## Reporting bugs, asking questions

- **Bugs and feature requests** — use the repository's issue templates.
- **Questions, ideas, show-and-tell** — use that repository's Discussions.
- **Security issues** — never in a public issue. See the
  [security policy](https://github.com/ezekiellabs/.github/blob/main/SECURITY.md).

## Code of Conduct

Participation is governed by our
[Code of Conduct](https://github.com/ezekiellabs/.github/blob/main/CODE_OF_CONDUCT.md).
