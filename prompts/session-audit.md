Create a rigorous session audit and visual review.

Requirements:
- resolve the authoritative session log or log set
- compute `pre-audit` metrics by default when the audit targets the current session
- compute `live` metrics only as a secondary view when useful
- make autonomous coding time the headline number
- use the methodology in `references/metrics-methodology.md`
- generate:
  - one markdown highlights file
  - one self-contained HTML visual explainer

Visual direction:
- use `https://github.com/nicobailon/visual-explainer` as the inspiration and workflow guide
- avoid ASCII tables
- use charts, bars, KPI cards, and a flow/timeline section

Must include:
- token counts
- tool-call counts
- user/assistant message counts
- sub-agent metrics
- artifact/edit metrics
- caveats and measurement methodology

If Remotion is available locally:
- optionally render an animated companion asset
- keep the HTML report as the canonical deliverable

If Remotion is not available:
- state that clearly
- fall back to HTML/CSS/SVG/DOM animation
