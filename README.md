# Session Review Visual Explainer

`session-review-visual-explainer` is a shareable Codex skill for auditing session logs and turning them into community-friendly reports.

![Session Review Visual Explainer](./assets/banner.png)

It combines:
- session-log parsing
- autonomy and tool-usage metrics
- markdown highlights output
- self-contained HTML visual explainers

This workflow is explicitly inspired by:
- `https://github.com/nicobailon/visual-explainer`

That project is the guide for the visual side of the skill:
- self-contained HTML
- strong typography
- charts instead of ASCII tables
- rich recap pages that can be shared directly

## What this skill is for

Use it when you want to answer questions like:
- how many tokens were used?
- how many tool calls happened?
- how much time was spent coding autonomously?
- what actually got accomplished?
- can we turn this into a visual report for the community?

## Outputs

The skill is designed to produce:
- a markdown highlights file
- a self-contained HTML review report

If a local Remotion toolchain exists, the skill can optionally add an animated companion asset. If it does not, the skill falls back to HTML/CSS/SVG/DOM animation and says so explicitly.

## Package contents

- `SKILL.md`: runtime instructions for the agent
- `agents/openai.yaml`: UI/discovery metadata
- `references/metrics-methodology.md`: autonomy and counting rules
- `prompts/session-audit.md`: reusable prompt skeleton
- `assets/`: icons for registries and UI

## Install

Copy this folder into a Codex skills directory, for example:

```bash
cp -R session-review-visual-explainer ~/.codex/skills/
```

Then invoke it explicitly with a prompt such as:

```text
Use $session-review-visual-explainer to audit this session log and generate a markdown highlights file plus an HTML report.
```
