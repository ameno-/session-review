---
name: session-review-visual-explainer
description: Audit Codex, Claude, or PI session logs, calculate autonomy and tool-usage metrics, and generate a markdown highlights file plus a shareable HTML visual explainer. Use when the user asks for a session review, autonomy audit, token/tool analysis, or post-session report.
license: MIT
---

# Session Review Visual Explainer

Purpose-built for community-shareable session audits.

This skill combines:
- rigorous session-log analysis
- explicit autonomy methodology
- visual HTML reporting

Inspiration and workflow guide:
- `https://github.com/nicobailon/visual-explainer`

Use that project as the aesthetic and packaging reference for the visual side of the workflow:
- rich typography
- self-contained HTML
- charts instead of ASCII tables
- animated KPI cards, bars, and flow diagrams

## When to use

Use this skill when the user asks for:
- a session review
- a post-session audit
- token or tool-call counts
- autonomy or “how long did you code without waiting?” analysis
- a shareable report for the community

## Inputs

Accept any of:
- one explicit session log path
- a session id
- “this session”
- a cluster of linked session ids

Typical sources:
- Codex rollout logs: `~/.codex/sessions/**/*.jsonl`
- Codex history hints: `~/.codex/history.jsonl`
- Claude project logs: `~/.claude/projects/**/*.jsonl`
- local hook logs or factory artifacts when they help with attribution

## Workflow

### 1. Resolve the authoritative log set

Prefer the most authoritative source available:
- Codex rollout JSONL for Codex sessions
- Claude/PI JSONL when the session lives there

If the user says “this session”:
- find the active or most recent rollout/session log
- if the thread references earlier linked sessions, include them only if they are part of the same workstream

If multiple logs are involved:
- state which one is primary
- state which ones are secondary/contextual
- keep the headline metrics scoped consistently

### 2. Define the audit window

Default behavior for current-session reviews:
- compute `pre-audit` metrics that stop at the user’s audit request
- optionally compute `live` metrics that include the audit turn itself

This avoids polluting the main numbers with the review work.

### 3. Compute mandatory metrics

Always compute:

1. Timeline and session shape
- start/end timestamps
- wall-clock span
- meaningful work window
- first user time
- first assistant response time
- last work event before audit cutoff
- largest idle gaps

2. Message and turn mix
- user message count
- assistant message count
- counts by event type
- counts by response item type
- tool calls per user turn

3. Tool orchestration
- total tool calls
- tool result mapping rate
- top tool frequencies
- sub-agent spawn/wait/send counts
- failures, timeouts, and unresolved calls

4. Edit and artifact activity
- `apply_patch` count
- patch lines/chars
- unique patched files
- mutating shell command count
- conservative artifact count

5. Validation activity
- test/build/lint/e2e commands observed
- whether claimed completion is backed by evidence

6. Autonomy
- strict activity-based autonomous execution time
- strict activity-based autonomous coding time
- longest uninterrupted coding turn
- merged autonomous coding blocks

Read `references/metrics-methodology.md` before calculating autonomy.

### 4. Produce two deliverables

Always generate:

1. Markdown highlights file
- concise
- numbers first
- caveats explicit

2. Self-contained HTML visual explainer
- no ASCII tables
- cards, bars, and charts
- include methodology/caveats
- include both strict and merged autonomy views when relevant

Output paths:
- use a user-requested destination if given
- otherwise prefer `~/.agent/diagrams/` for HTML
- store markdown beside the HTML or in the requested artifact directory

### 5. Visual treatment

Follow the visual-explainer style philosophy:
- use a deliberate palette
- use real typography
- make the autonomy metric dominant
- visualize long-run blocks and tool mix
- use tables only for dense comparisons

Recommended sections:
- executive summary
- raw KPIs
- autonomy breakdown
- merged-run chart
- token composition
- tool orchestration
- swarm/sub-agent outcomes
- caveats and evidence notes

### 6. Optional Remotion path

If a local Remotion skill or toolchain is available:
- generate an animated companion artifact for charts or timeline sequences
- keep the HTML report as the canonical review document

If Remotion is not available:
- say so briefly
- fall back to self-contained HTML animation with CSS/SVG/DOM

Do not pretend Remotion support exists if it is not installed.

## Output contract

Your final response should include:
- primary log path used
- whether metrics are `pre-audit`, `live`, or both
- the autonomous coding number in plain text
- output file paths for the HTML and markdown deliverables
- any caveat that materially changes interpretation

## Constraints

- Do not guess token counts if the log provides them
- Do not use wall-clock span as the autonomy headline
- Do not hide idle gaps
- Do not merge across large silent periods
- Do not overclaim artifact counts when filesystem mtime is polluted by unrelated work

## References

- Read `references/metrics-methodology.md` for formulas and thresholds
- Use `prompts/session-audit.md` as the reusable command/prompt skeleton
