# Session Audit Metrics Methodology

This file defines the metrics used by `session-review-visual-explainer`.

## Primary principle

The autonomy headline must answer:
- “How much time was spent coding, not waiting for instructions?”

That means wall-clock span is never the headline metric.

## Windows

### Pre-audit

Default for current-session reviews.

Stop metrics at the user message that requested the audit.

### Live

Optional.

Include the audit turn itself. Use this only as a secondary number.

## Time metrics

### Session wall span

`last_event_before_cutoff - first_event`

Use only as context.

### Meaningful work window

`last_work_event_before_cutoff - first_user_message`

Useful when the session remained open long after real work ended.

### Strict activity-based active execution time

For each user-turn window:
- collect execution events after the user message
- ignore passive token-count events
- split intervals on long silent gaps
- sum only the contiguous active intervals

Recommended silent-gap threshold:
- `20 minutes`

### Strict activity-based coding time

Start from strict active execution time.

Count a turn or interval as coding-bearing when it includes one or more of:
- `apply_patch`
- mutating shell commands
- code/test file creation
- explicit build/test commands directly tied to implementation verification

Typical mutating shell signals:
- `cat >`
- `cat >>`
- `node <<`
- `mv`
- `cp`
- `mkdir -p`
- `touch`
- `git push`
- `git merge`
- `committer`

Adjust for local workflows when needed.

### Longest uninterrupted coding turn

The longest single strict coding interval without merging across user interruptions.

### Merged autonomous coding blocks

This is the “felt autonomy” view.

Merge adjacent strict coding intervals when:
- the gap between them is small
- the interruption was only a brief approval/check-in

Recommended merge threshold:
- `5 minutes`

Never merge across:
- long silent gaps
- overnight gaps
- obvious context resets

## Tool metrics

Always compute:
- tool calls
- tool outputs
- custom tool calls
- mapping rate between calls and results
- top tool frequencies

## Token metrics

Prefer log-native token counters if present.

Typical categories:
- `input_tokens`
- `cached_input_tokens`
- `output_tokens`
- `reasoning_output_tokens`
- `total_tokens`

If both pre-audit and live are available:
- report both
- compute the audit delta if useful

## Artifact metrics

Use conservative counting.

Preferred order:
1. command-targeted output files observed in-session
2. patch target files
3. filesystem modification-time counts only when the directory is known to be isolated

If mtime-based counts may be polluted by unrelated work:
- say so
- fall back to the conservative command-targeted count

## Sub-agent metrics

Always compute:
- spawn count
- wait count
- timed-out waits
- completed sub-agent results
- invalid routing or unsupported-agent failures

Do not treat a timed-out wait as a failed agent unless the content proves failure.

## Recommended HTML sections

1. Executive summary
2. Raw KPIs
3. Autonomy breakdown
4. Merged coding blocks
5. Timeline / idle gaps
6. Tool orchestration
7. Sub-agent outcomes
8. Caveats
