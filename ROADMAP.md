# Roadmap

Ezekiel Labs is building a **toolkit** for security teams — practical tools in
the space between offense and defense that make a team faster, smarter, and
better informed about what their stack can actually see.

Not a single product with features bolted on. Separate tools that share a
knowledge base, a design language, and one commitment: they tell you the truth
about coverage, including when the answer is "you can't see this."

This file is the queue. It is deliberately ordered, deliberately honest about
what has not started, and it keeps the ideas we ranked *low* along with the
reason — a roadmap that only lists winners is a marketing page.

Five sections, and the differences matter:

- **Shipped** — released and in use.
- **Next** — committed, and the thing being worked on.
- **Queued** — scoped, and buildable with what exists today.
- **Horizon** — larger bets that would define the org rather than extend it.
  Each is gated on something earlier on this page landing first. Listing them is
  not the same as committing to them, and they are deliberately kept apart so
  the near-term work is not read as vaguer than it is.
- **Considered, ranked low** — declined, with the reasoning kept.

Each entry says what it is, why it sits where it does, what it reuses, and what
the hard part is.

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
An MCP server puts the same data in front of everyone doing LLM-assisted
security work.

**Why first:** it is the substrate every tool below needs. Building the second
tool without it means forking the knowledge base, and two copies of a knowledge
base is how a toolkit dies.

**Scope:** a stable public API over `kb`, `matcher`, `sigma_eval`, and the
platform knowledge bases; opseclint's binary becomes the first consumer rather
than the owner.

## Queued

Ordered by what unblocks the most, then by conviction. Detection feasibility leads
because four of the six Horizon bets and one queued entry are gated on it — it is
the hinge for most of this page. None started.

### 1. Detection feasibility — what your stack can never see

Two inputs: a ruleset (SigmaHQ or your own), and a description of what you
actually collect (a Sysmon config XML, an auditd rules file, an ESF
subscription list, an index-field dump). Output: which rules are dead on
arrival because your sensors never populate the fields they key on.

> 412 of 2,216 rules can never fire here. 180 need script-block logging.
> 96 need file-create events you exclude in `sysmonconfig.xml:214`.

**Why first:** it unblocks more than anything else here. Entry 5 is this plus a
scheduler, and four of the six Horizon bets — negative-space, incident replay,
the collection compiler, and the Ezekiel Index — are all gated on it. Nothing
else on this page has that fan-out.

On its own merits it also inverts opseclint. opseclint asks "what would a
defender see?" — this asks "given what you collect, what can't you see, no
matter how many rules you buy?" Same purple thesis, one layer down. Counting
rules is the coverage metric everyone uses and it is close to meaningless; this
replaces it with a harder question.

**Reuses:** the Sigma parser, `sigma_eval`'s field extraction
(`referenced_fields` already does most of the work), the platform triple, the
render layer, `--navigator`.

**Evidence it's real:** opseclint's own verification numbers are exactly this
measurement, arrived at accidentally, against its own knowledge base.

**Risk:** Sysmon config semantics are genuinely nasty — include/exclude
precedence, rule groups, `onmatch` inversion. auditd and ESF are far simpler.
Ship Linux first.

### 2. `sigmalint` — static analysis for detection content

opseclint analyzes *commands*. This analyzes *rules*: selections referencing
fields their declared logsource never emits, selections broad enough to be
meaningless, missing ATT&CK tags, duplicated logic across a ruleset,
deprecated modifiers.

**Why here:** the motivating evidence is measured, not assumed.
[opseclint#61](https://github.com/ezekiellabs/opseclint/pull/61) found that on
Windows, 69 of 84 knowledge-base entries cite rules keyed on fields a
process-creation event cannot carry — `ScriptBlockText`, `EventID`, `Payload`,
PE metadata. Those rules are not wrong exactly, but nothing tells a detection
engineer they can never fire against the source they think they are covering.
This is the blue-team counterpart to opseclint's red lean, which makes "purple
by default" structural rather than a slogan.

**Reuses:** `sigma_eval`'s parser and field extraction, wholesale.

### 3. Detection regression harness — CI for detection content

A ruleset plus a labeled corpus of true-positive and known-benign command
lines. Run every rule; report which stopped firing after an edit, and which
started firing on benign input.

**Why here:** detection engineers edit rules constantly with no test suite.
Both hard pieces already exist — an evaluator, and the known-benign corpus
opseclint's own tests use to assert zero false positives. The most defensible
product on this list, because it turns existing assets into something with
recurring use rather than a one-off report.

**Risk:** the corpus is the product. A thin corpus makes a harness that passes
everything.

### 4. "Explain this rule" — trigger shapes and false-positive surface

Point it at one Sigma rule: what command shapes would trigger it, and what it
hits in the benign corpus.

**Why here:** by far the cheapest thing on this list — probably a weekend,
mostly wiring parts that exist. Solves a real triage annoyance ("why did this
fire?"). Small enough that it should ship as an **opseclint flag first**, and
only become its own tool if people actually reach for it.

### 5. Visibility drift monitor

Snapshot sensor config and ruleset; diff over time; alert when a change
silently reduces coverage — "an exclusion added to `sysmonconfig.xml` took 14
techniques dark."

**Why here:** this is entry 1 with time added, and it is the version with
budget attached. "Prove our detection coverage didn't regress this quarter" is
a compliance sentence. Harder to sell to an individual, much easier to a team.
It should not be built before 1, because it is 1 plus a scheduler.

### 6. Engagement reporter — the deliverable, not the terminal

Consume opseclint's predicted output plus the telemetry captured during a
purple-team engagement; emit a shareable HTML report: what was executed, what
fired, what did not, ranked blind spots, an ATT&CK layer.

**Why here:** a different reader than everything above — the client and the
CISO, not the operator. Purple teams bill real hours assembling these by hand.
It also proves the org can produce a *deliverable*, not only a terminal tool.

### 7. Detection dojo — the teaching artifact

A static site generated from the knowledge base: here is a command, guess what
fires, reveal the techniques, telemetry, detections, and score.

**Why here:** 233 modeled actions is already a curriculum. Cheapest build on
the list and the only one whose value is reach rather than revenue — it makes
the org legible to people who will never install a Rust CLI.

### 8. Cloud / SaaS control plane

The same thesis pointed at CloudTrail, Entra, Okta, Workspace: given an API
call, what does it log and what detects it.

**Why last:** the largest audience and the least crowded, but it shares almost
no code with anything above — a new knowledge base, a new event model, new rule
sources. Closer to a second company than a second tool.

## Horizon

Bigger bets. Each is gated on something earlier on this page landing first —
`opseclint-core` under Next, or an entry from the queue — and each is sketched
rather than specified. That is the honest state of them. Roughly ordered by how
soon the dependency clears.

### Agent substrate — ground truth for LLM agents doing security work

`opseclint-core` ships an MCP server over the knowledge base. This is the
larger version: a stable, versioned service that answers *what does this action
emit, what detects it, and could **you** see it* across every tool in the
toolkit, not just opseclint.

**Why it matters:** agents are being pointed at security work right now and
they have no ground truth. They hallucinate detections and confidently misjudge
what is observable. An agent grounded in a real knowledge base and a real
evaluator is materially less wrong, and that makes this infrastructure rather
than a tool.

**Depends on:** `opseclint-core`, then breadth from the queue.

**The hard part, and it is the whole thing:** agents amplify whatever they are
given. Our abstain-honestly property stops being a nice trait and becomes
load-bearing — an `INDETERMINATE` that an agent silently rounds to "not
detected" is worse than no answer at all. The API has to make uncertainty
impossible to discard, which is an interface design problem more than a
technical one.

**Timing:** the most time-sensitive item on this page. The window where being
the obvious grounding source is winnable is now, not in three years.

### Negative-space engine — enumerate what is invisible

Every security tool enumerates what you *can* see. Invert it: given a sensor
config and a ruleset, produce the techniques that are invisible in this
environment, ranked by how much an adversary would want them. Not "here are
your alerts" but "here are the 340 things that would leave no trace here."

**Why it matters:** no one can produce this today. It is the single most useful
artifact a defender could hold, and it is the purest expression of the thesis —
absence of a finding, made explicit and enumerable instead of assumed.

**Depends on:** detection feasibility (queue 1), plus real knowledge-base
breadth.

**The hard part is an honesty trap.** "Invisible" is only meaningful relative
to a technique universe we have modeled, and 233 entries is a floor, not a map.
An incomplete blind-spot list presented as complete is exactly the failure this
org exists to argue against — it would be our own tool telling the lie we
built the org to expose. Any version of this has to lead with what it does not
model, and that framing has to survive contact with marketing.

### Counterfactual incident replay — "would we have seen this?"

Feed in a breach report, an intel writeup, or an emulation plan; get back a
step-by-step verdict with the specific collection gap at each step.

**Why it matters:** every team does this by hand, badly, in a meeting. It turns
threat intel from reading material into a ranked remediation list.

**Depends on:** detection feasibility (queue 1).

**The hard part:** parsing prose intel into a technique sequence is the
unsolved half. The tractable version takes structured input — an ATT&CK
Navigator layer, a TTP list, an emulation plan — and leaves free-text ingestion
to a later, clearly-labeled, best-effort mode. Whatever assists the parsing,
the *verdict* must stay mechanically derived and checkable; a plausible-sounding
answer here is worse than none.

### Collection compiler — coverage-driven sensor configuration

Today Sysmon configs are hand-written XML copied from a GitHub repo and rarely
audited. Invert the direction: declare the coverage you want and the event
volume you can afford, emit an optimized config.

**Why it matters:** it reframes sensor configuration from folklore into
engineering, and it is the natural inverse of the feasibility checker — same
model, run backwards.

**Depends on:** detection feasibility (queue 1), whose config parsing this
reuses in reverse.

**The hard part, and it is a different kind of hard:** this is the first tool
whose output people *deploy*. Everything else on this page reports; a bug here
degrades someone's actual security posture rather than misdescribing it. It
also needs an event-volume cost model, and nobody has good public data for
that. Blast radius argues for shipping it as a proposer — emit a diff and an
explanation, never write the live config.

### Detection package manager — dependency resolution for detection content

Signed, versioned detection bundles that **declare their telemetry
dependencies**. Install a technique bundle and it tells you what collection it
requires, and refuses to claim coverage the environment cannot support.

**Why it matters:** detection content today ships the way software did in 1998
— copy a YAML file from a repo, hope. Declared dependencies plus a feasibility
check is the piece that makes "detection-as-code" mean something.

**Depends on:** `sigmalint` (queue 2), the regression harness (queue 3), and
feasibility (queue 1). Dependency resolution is meaningless without all three.

**The hard part:** package managers are won by distribution and content, not by
design. This is worthless without publishers and adopters, which makes it an
ecosystem play with a long fuse — the right move is probably to make the
*format* excellent and let someone else run a registry.

### The Ezekiel Index — a public, reproducible measurement

A recurring published measurement: across the public SigmaHQ ruleset and the
sensor configs people actually deploy, what percentage of ATT&CK is *provably*
detectable? Methodology open, results reproducible, run on a schedule.

**Why it matters:** the DBIR and the OWASP Top 10 are enormously influential
and largely qualitative. An empirical, reproducible benchmark would be cited
constantly — and it is close to free, because it is existing tooling pointed at
public data.

**Depends on:** detection feasibility (queue 1). Cheapest item in this section
by a wide margin once that exists.

**The hard part:** publishing a number that grades other people's ecosystems
invites scrutiny, which is fine — provided the methodology is genuinely open
and the number moves when reality does. The failure mode is an index that
becomes marketing, and the guard against it is publishing the code and the
inputs alongside the result, every time.

## Considered, ranked low

Kept here so they do not get re-proposed without the reasoning.

### Ruleset coverage mapper

Point it at a whole detection library, get an ATT&CK Navigator layer of real
coverage plus duplicates and orphans.

Cheap — `--navigator` and the technique index already exist, so maybe two
weeks. But DeTT&CT and Sigma's own tooling occupy this space, and it measures
rule *presence*, which is precisely the metric entry 1 exists to debunk.
Building it would put us slightly at odds with our own thesis.

### Sensor-config auditor

The config half of entry 1 on its own: given a Sysmon config, which of
opseclint's 233 modeled actions go dark? No ruleset involved.

Smallest scope and ships fastest, but it is a **feature, not a product** — it
belongs as an opseclint flag. It is also the right way to prototype entry 1:
add config parsing behind a flag, see whether the output is compelling, then
split it out.

## How this list changes

Anything here can be argued down or reordered; open a
[discussion](https://github.com/ezekiellabs/opseclint/discussions). What does
not change is the bar: a tool ships when it is honest about its own limits.
Absence of a finding is never proof of stealth, and that applies to the toolkit
as much as to any command it analyzes.
