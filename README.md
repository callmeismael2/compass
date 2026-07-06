# Compass

A Claude Code slash command that runs a full competitive UX analysis for any digital pattern and renders the results directly into Figma.

You name a pattern and a platform. Compass pulls real screens from Mobbin, researches competitors, and builds a structured analysis in your Figma file — no manual copying, no tab switching.

## What it produces

A new Figma page named `Compass — [Pattern] — [Platform] — [Date]`, containing:

| Section | What it covers |
|---|---|
| S0 | Methodology & scope |
| S0.5 | Market landscape — 2×2 competitive positioning map |
| S1 | 48 Mobbin screenshots across 8 pattern sub-categories + your product's current state |
| S1b | User flow walkthroughs — 8 storyboard strips |
| S2 | Personas & jobs-to-be-done |
| S3 | Journey / flow maps |
| S3.5 | Ideal pattern wireframe — annotated schematic happy path |
| S4 | Competitor teardowns with strategic hypothesis + trajectory |
| S4.5 | User sentiment — App Store ratings and review themes |
| S5 | Feature comparison matrix with your product as the baseline |
| S6 | Pattern synthesis with opportunity tier badges |
| S7 | Heuristic scorecard |
| S8 | Accessibility audit |
| S9 | Strategic implications — gap map, P0/P1/P2 recommendations, executive one-liner |

## Prerequisites

### 1. Claude Code

Install from [claude.ai/code](https://claude.ai/code). The desktop app is recommended — it includes the Figma MCP built in.

### 2. Figma MCP

The Figma MCP is bundled with the Claude Code desktop app. Enable it in **Settings → Integrations → Figma**.

You'll need a Figma file to write to. Create one and note the file key from the URL:
```
figma.com/design/[FILE-KEY]/your-file-name
```

### 3. Mobbin MCP

Compass pulls screens from [Mobbin](https://mobbin.com). **A paid Mobbin account is required.**

**Step 1 — Add the MCP** (run in your terminal):
```bash
claude mcp add mobbin --scope user --transport http https://api.mobbin.com/mcp
```

**Step 2 — Authenticate** (must run in an interactive terminal, not inside Claude):
```bash
claude mcp login mobbin
```
Open the URL it prints in your browser and complete the OAuth flow. Keep the terminal process alive until you see a confirmation message.

> **Can't find the `claude` command?** Use the full path to the binary. Find it in Claude Code → Settings → About, or locate it at:
> ```
> ~/Library/Application Support/Claude/claude-code/[version]/claude.app/Contents/MacOS/claude
> ```
> Wrap the path in quotes if it contains spaces.

## Installation

Copy `compass.md` into your Claude Code commands directory:

```bash
cp compass.md ~/.claude/commands/compass.md
```

Restart Claude Code if it's already running.

## Usage

In any Claude Code session, run:

```
/compass
```

Compass asks four questions one at a time:

1. **Pattern** — what to analyze (e.g. `upsell and cross-sell`, `paywall`, `onboarding`, `churn save`)
2. **Platform** — where to look (e.g. `iOS mobile`, `web desktop`, `Android mobile`)
3. **Your product name** — used as the baseline row in all comparison tables (e.g. `Peacock`, `Spotify`)
4. **Figma file key** — the key from your Figma file URL

Confirm the inputs and Compass runs end-to-end. The Figma file is the deliverable — output is written directly to a new page, not to chat.

## Platform support

Mobbin's API supports `ios` and `web` only. Compass maps your input accordingly:

| Your input | Mobbin searches as |
|---|---|
| iOS mobile | `ios` |
| Web desktop | `web` |
| Android mobile | `web` — results annotated as web reference |
| Connected TV | `web` — results annotated as web reference |
| Multiple platforms | searches run per platform; best results selected |

## Your product baseline

Compass searches Mobbin for your product's current implementation of the pattern and places it as the first row in S1 — above all competitor categories. This gives you a direct visual reference before the competitive landscape.

- **Known products** (major apps listed on Mobbin) — real screens are pulled automatically
- **Unknown or internal products** — a labeled placeholder strip is placed instead, with a note to add your own screens manually

Your product also appears as the first row in S5 (feature matrix) and S7 (heuristic scorecard), scored against every competitor.

## Animated screens

Mobbin's MCP returns static images only — video recordings and animations are not accessible through the API. Each screen in S1 is sourced from Mobbin and links back to the original listing, where you can view any available animations manually.

## Known limitations

**Run time and cost.** A full run spans 13 sections, ~15 plugin calls, 48+ image downloads and uploads, and multiple web searches. Expect 20–45 minutes depending on Mobbin response times and Figma write speed. Token consumption is significant — typically 80,000–150,000 tokens per run. Check your usage tier before triggering.

**Partial completion.** Long agentic runs have failure points. If a run stops mid-way, note the last completed section and its end-Y coordinate from chat. In a new session, navigate to the existing Figma page and resume from the next section — each section is independent once its start-Y is known. Do not create a new page on resume.

**AI-inferred sections.** Two sections are synthesized from training knowledge, not retrieved from live sources:
- **S2 — Personas**: derived from pattern knowledge and screenshot analysis, not primary user research. Marked "AI-INFERRED" in the Figma output.
- **S4.5 — Sentiment themes**: review themes are synthesized, not scraped from live App Store reviews. Ratings are web-sourced and treated as factual; theme bullets are prefixed with "~" to signal inference.

**Screenshot-based evaluation.** S7 (heuristic scoring) and S8 (visual accessibility flags) are based on static screenshots only. They surface directional signals — not substitutes for live heuristic evaluation or WCAG testing with assistive technology. S8 is explicitly labeled "Visual Accessibility Flags" for this reason.

**Android and CTV.** Mobbin's API supports `ios` and `web` only. Android and CTV searches fall back to web results, labeled accordingly in the output.

**Mobbin content in shared files.** Compass output files contain screenshots sourced from Mobbin using your paid account. Review [Mobbin's terms of service](https://mobbin.com/terms) before sharing output Figma files externally or publishing screenshots from them.

## Output example

*Screenshot coming soon — contributions welcome.*

## License

MIT
