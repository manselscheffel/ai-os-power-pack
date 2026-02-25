# AI OS Power Pack

> Email digest, lead research, Telegram bot, slide generation, and memory management for Claude.

A plugin for Claude Code and Cowork that adds five production-grade business automation skills. Each skill follows the separation of concerns principle: AI reasons about WHAT to do, deterministic scripts handle HOW.

## Install

### Claude Code

```bash
/plugin marketplace add manselscheffel/ai-os-power-pack
/plugin install ai-os-power-pack@manselscheffel/ai-os-power-pack
```

### Cowork

Upload the `.plugin` file from [Releases](https://github.com/manselscheffel/ai-os-power-pack/releases), or browse plugins in the Customize sidebar.

## Skills

| Skill | What It Does | Required API Keys |
|-------|-------------|-------------------|
| **email-digest** | Gmail inbox to prioritized briefing with sentiment analysis | Gmail OAuth |
| **research-lead** | LinkedIn URL to research package + personalized outreach | Relevance AI, Perplexity, OpenAI |
| **telegram** | Route Telegram messages to Claude with persistent memory | Telegram Bot, Anthropic, OpenAI, Pinecone |
| **gamma-slides** | Markdown to professional slide decks via Gamma | Gamma |
| **memory-manager** | 3-tier persistent memory (files + logs + vectors) | None (Tier 3: OpenAI + Pinecone) |

## Commands

| Command | What It Does |
|---------|-------------|
| `/email-digest` | Process inbox, analyze, prioritize, brief |
| `/research-lead <url>` | Research a lead from LinkedIn URL |
| `/telegram [start\|test\|check]` | Start or manage the Telegram bot |
| `/slides <topic>` | Generate a presentation |
| `/memory [search\|save\|review]` | Manage persistent memory |

## How It Works

Each skill is a self-contained workflow. The AI reads the skill instructions, decides the approach, then delegates execution to Python scripts. This separation means:

- Scripts handle API calls, data processing, and file operations deterministically
- Claude handles reasoning, analysis, personalization, and judgment calls
- A 7-step pipeline produces consistent results instead of probabilistic guessing

## Requirements

- Claude Code or Cowork subscription
- Python 3.9+ for scripts
- API keys per skill (see table above)

## License

MIT
