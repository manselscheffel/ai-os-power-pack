---
name: telegram
description: Route Telegram messages to Claude Code with persistent memory. Polls for messages, validates users, loads semantic context, invokes Claude, and captures conversations. Trigger with "start telegram bot", "set up telegram", "telegram daemon", or "connect telegram".
---

# Telegram Bot

Route Telegram messages to Claude Code with persistent memory (mem0 + Pinecone).

## How It Works

1. **Poll** -- Long-polls Telegram Bot API for incoming messages
2. **Validate** -- Whitelist check, rate limiting, blocked content patterns
3. **Load memory** -- Semantic search against Pinecone for relevant memories
4. **Build context** -- Memory + last 20 conversation messages + user's request
5. **Invoke Claude** -- Runs Claude Code CLI with streaming JSON output + progress updates
6. **Respond** -- Sends formatted response back via Telegram
7. **Capture** -- Feeds the exchange into mem0 for future recall

## Scripts

| Script | Purpose |
|--------|---------|
| `scripts/telegram_handler.py` | Core daemon -- orchestrates the full loop |
| `scripts/telegram_bot.py` | Polling + security validation |
| `scripts/telegram_send.py` | Telegram Bot API wrapper (send messages, files, photos) |
| `scripts/message_db.py` | SQLite message history |

## Quick Start

```bash
# Check Claude CLI is available
python3 ${CLAUDE_PLUGIN_ROOT}/skills/telegram/scripts/telegram_handler.py --check

# Test Claude invocation + memory search (no Telegram needed)
python3 ${CLAUDE_PLUGIN_ROOT}/skills/telegram/scripts/telegram_handler.py --test "What do you know about me?"

# Dry run -- polls Telegram but doesn't invoke Claude
python3 ${CLAUDE_PLUGIN_ROOT}/skills/telegram/scripts/telegram_handler.py --dry-run

# Start the daemon
python3 ${CLAUDE_PLUGIN_ROOT}/skills/telegram/scripts/telegram_handler.py
```

## Configuration

Config file: `references/messaging.yaml`

- **Whitelist**: `telegram.allowed_user_ids` -- only these Telegram user IDs can trigger Claude
- **Rate limits**: 30 messages/minute, 200/hour (configurable)
- **Blocked patterns**: `rm -rf`, `sudo`, `DROP TABLE`, `DELETE FROM`
- **Confirmation**: Dangerous operations (file deletion, git push) require explicit confirmation

To find your Telegram user ID: message `@userinfobot` on Telegram.

## Security

- Reject-by-default: no whitelist = no access
- Rate limiting prevents abuse (per-minute and per-hour)
- Blocked patterns catch dangerous commands before they reach Claude
- Secret sanitization strips API keys, tokens, JWTs from memory capture
- Claude runs with `--dangerously-skip-permissions` (required for unattended bot execution)

## Environment Variables

```
TELEGRAM_BOT_TOKEN     # From @BotFather on Telegram
ANTHROPIC_API_KEY      # For Claude Code CLI
OPENAI_API_KEY         # For mem0 fact extraction
PINECONE_API_KEY       # For vector memory storage
```

## Data

- Message history: `data/messages.db` (SQLite, auto-created)
- Logs: `logs/messaging.log`
- Memory: Pinecone vector store (shared across sessions)
