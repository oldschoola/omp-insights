<!-- SPDX-FileCopyrightText: 2026 Hari Srinivasan <harisrini21@gmail.com> -->
<!-- SPDX-License-Identifier: AGPL-3.0-only -->

# Pi Insights

Personal usage analytics for the [Pi coding agent](https://github.com/earendil-works/pi). Scans your session history, extracts deterministic stats and LLM-powered facets, then generates a self-contained HTML report covering your workflows, friction points, and suggestions for improvement.

## Install

**From npm** (recommended):

```bash
pi install npm:@observal/pi-insights
```

**From source:**

```bash
git clone https://github.com/BlazeUp-AI/pi-insights.git
pi install ./pi-insights
```

**Try without installing:**

```bash
pi -e npm:@observal/pi-insights
```

## Usage

Run the command inside any Pi session:

```
/insights
```

The report opens in your browser automatically. It covers:

- **At a Glance** synthesis of what is working, what is hindering you, quick wins, and ambitious workflows
- **Stats Overview** with tokens, cost, lines changed, tool usage, and response time distributions
- **Project Areas** grouped by where you spent time
- **Interaction Style** analysis of how you work with Pi
- **What's Working** with your most impressive workflows
- **Friction Analysis** categorized by type with concrete examples
- **Suggestions** including config additions, features to try, and usage patterns with copyable prompts
- **On the Horizon** for ambitious future workflows as models improve

### Flags

| Flag | Description |
|------|-------------|
| `--refresh` / `-r` | Invalidate all cached LLM facet extractions and re-run them |
| `--no-open` | Generate the report without opening it in the browser |

### Examples

```bash
# Normal run (uses caches, fast on re-runs)
/insights

# Force re-extraction of all session facets
/insights --refresh

# Generate without auto-opening
/insights --no-open
```

## How It Works

The pipeline runs in five phases:

1. **Scan** all Pi session log files
2. **Extract stats** deterministically from each session (tool counts, tokens, languages, git activity, response times)
3. **LLM facet extraction** per session using a cheap model (Haiku) to classify goals, outcomes, satisfaction, and friction
4. **Aggregate and generate insights** using 7 parallel insight prompts plus a synthesis prompt on your active model
5. **Render** a self-contained HTML report with charts, cards, and copyable suggestions

Results are cached in `~/.pi/agent/usage-data/`:

| Path | Contents |
|------|----------|
| `session-meta/<id>.json` | Deterministic stats, cached permanently |
| `facets/<id>.json` | LLM-extracted facets, cached permanently (clear with `--refresh`) |
| `report.html` | Last generated report |

## Requirements

- [Pi](https://github.com/earendil-works/pi) v0.74.0 or later
- An active model configured in Pi (used for insight generation)
- Optionally, access to Claude Haiku (via Bedrock or Anthropic API) for cheaper facet extraction. Falls back to your active model if unavailable.

## License

AGPL-3.0-only
