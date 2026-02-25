---
name: memory-manager
description: Manage persistent memory across sessions using daily logs, MEMORY.md, and optional mem0 vector memory. Trigger with "what do you remember", "save this to memory", "search memory for", "show today's log", "weekly review of memory", or "what happened yesterday".
---

# Memory Manager

Three-tier persistent memory system for Claude sessions.

## Architecture

```
Tier 1: MEMORY.md        -- Always in prompt. Curated facts. ~200 lines max.
Tier 2: Daily logs        -- Session history. Append-only. Read at session start.
Tier 3: mem0 + Pinecone   -- Optional. Cloud vectors. Semantic search. Auto-dedup.
```

## Session Start Protocol

1. Read `memory/MEMORY.md` for curated facts and preferences
2. Read today's log: `memory/logs/YYYY-MM-DD.md`
3. Read yesterday's log for continuity (if exists)
4. Optionally search mem0 for relevant context

## During Session

Append notable events, decisions, and completed tasks to today's log:

```
- [HH:MM] Completed lead research for [person]
- [HH:MM] User prefers direct tone in LinkedIn posts
- [HH:MM] Fixed Gmail API rate limit issue in fetch_emails.py
```

## Memory Operations

### Save a fact

Add to `memory/MEMORY.md` under the appropriate section. Keep it under 200 lines. If using mem0:

```bash
python3 tools/memory/mem0_add.py --content "User prefers Pinecone over Supabase for vectors"
```

### Search memory

```bash
python3 tools/memory/mem0_search.py --query "what tools does user prefer" --limit 10
```

### Review memory

```bash
python3 tools/memory/mem0_list.py --limit 50
```

### Sync mem0 to MEMORY.md

```bash
python3 tools/memory/mem0_sync_md.py           # Regenerate MEMORY.md from mem0
python3 tools/memory/mem0_sync_md.py --dry-run  # Preview changes
```

### Delete a memory

```bash
python3 tools/memory/mem0_delete.py --memory-id "abc123"
```

## Daily Log Format

```markdown
# Daily Log: YYYY-MM-DD

> Session log for Day, Month DD, YYYY

---

## Events & Notes

- [09:15] Session started, reviewed yesterday's progress
- [09:30] Researched competitor pricing -- found 3 alternatives
- [10:45] Drafted LinkedIn post about AI automation
- [11:00] User confirmed preference for informal tone
```

## Tier 3 Setup (Optional)

For semantic search and automatic fact extraction, set up mem0 + Pinecone:

1. Create free Pinecone account (100K vectors free)
2. Set environment variables: `OPENAI_API_KEY`, `PINECONE_API_KEY`
3. Configure `args/mem0_config.yaml`
4. Cost: approximately $0.04/month

## Edge Cases

- **MEMORY.md exceeds 200 lines**: Summarize older entries, archive to daily log
- **Conflicting memories**: mem0 auto-deduplicates (ADD/UPDATE/DELETE/NOOP)
- **No mem0 configured**: Tier 1+2 work standalone with zero dependencies
