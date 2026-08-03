# Roadmap

Ezekiel Labs is building a **toolkit** for security teams — practical tools in
the space between offense and defense that make a team faster, smarter, and
better informed about what their stack can actually see.

Not a single product with features bolted on. Separate tools that share a
knowledge base, a design language, and one commitment: they tell you the truth
about coverage, including when the answer is "you can't see this."

This file is the queue. It is deliberately ordered, and deliberately honest
about what has not started.

## Shipped

### [opseclint](https://github.com/ezekiellabs/opseclint) — detection-coverage analyzer

Point it at a command, script, or playbook: it resolves each action to the
MITRE ATT&CK technique(s) it implements, the host telemetry it emits, and the
detections that would fire, with a 0–100 detectability score. Linux/auditd,
Windows/Sysmon, macOS/Endpoint Security. Ingests recorded telemetry to check
prediction against what a sensor really logged.

Remaining work is tracked in [its issues](https://github.com/ezekiellabs/opseclint/issues).

## Next

### `opseclint-core` — the knowledge base and evaluator, as a library

Publish the knowledge base, the `match` engine, and the Sigma rule evaluator as
a consumable crate, plus an MCP server over the same API.

Today all of it is locked inside one binary. As a library it becomes something
other tools build on — a SIEM enrichment step, a notebook, an agent that can
answer "what detects T1059.001 on Windows, and what would that command emit?"
It is also the substrate every tool below needs, which is why it comes first:
building the second tool without it means forking the knowledge base, and two
copies of a knowledge base is how a toolkit dies.

Scope: a stable public API over `kb`, `matcher`, `sigma_eval`, and the platform
knowledge bases; opseclint's binary becomes the first consumer rather than the
owner. An MCP server ships alongside so the same data is reachable from
LLM-assisted workflows without a shell.

## Queued

Ordered by conviction, not by size. None started.

### `sigmalint` — static analysis for detection content

opseclint analyzes *commands*. This analyzes *rules*: selections referencing
fields their declared logsource never emits, selections broad enough to be
meaningless, missing ATT&CK tags, duplicated logic across a ruleset,
deprecated modifiers.

The motivating evidence is measured, not assumed. opseclint's own verification
found that on Windows, 69 of 84 knowledge-base entries cite rules keyed on
fields a process-creation event cannot carry — `ScriptBlockText`, `EventID`,
`Payload`, PE metadata. Those rules are not wrong exactly, but nothing tells a
detection engineer they can never fire against the source they think they are
covering. This is the blue-team counterpart to opseclint's red lean, and it is
what makes "purple by default" structural rather than a slogan.

### Detection feasibility — what your stack can never see

Given a rule set and a description of what you actually collect (a Sysmon
config, an auditd ruleset, an ESF subscription list), report which rules are
dead on arrival because your sensors never populate the fields they key on.

Counting rules is the coverage metric everyone uses and it is close to
meaningless. This replaces it with a harder question: given what you collect,
what can you *not* see, regardless of how many rules you own? Add time and it
becomes drift detection — "an exclusion added to sysmonconfig.xml took 14
techniques dark."

Sysmon's include/exclude precedence is the hard part; auditd and ESF are much
simpler and should ship first.

### Engagement reporter — the deliverable, not the terminal

Consume opseclint's predicted output plus the telemetry captured during a
purple-team engagement, emit a shareable report: what was executed, what fired,
what did not, ranked blind spots, an ATT&CK layer.

A different reader than everything above — the client and the CISO, not the
operator. Purple teams bill real hours assembling these by hand.

### Detection dojo — the teaching artifact

A static site generated from the knowledge base: here is a command, guess what
fires, reveal the techniques, telemetry, detections, and score.

233 modeled actions is already a curriculum. Cheapest thing on this list and
the only one whose value is reach rather than revenue.

### Cloud control plane

The same thesis pointed at CloudTrail, Entra, Okta, Workspace: given an API
call, what does it log and what detects it.

The largest audience and the least crowded, but it shares almost no code with
anything above — a new knowledge base, a new event model, new rule sources.
Listed last because it is closer to a second company than a second tool.

## How this list changes

Anything here can be argued down or reordered; open a
[discussion](https://github.com/ezekiellabs/opseclint/discussions). What does
not change is the bar: a tool ships when it is honest about its own limits.
Absence of a finding is never proof of stealth, and that applies to the toolkit
as much as to any command it analyzes.
