---
name: email-digest
description: Process Gmail inbox to identify urgent and high-risk emails, analyze sentiment and urgency, generate strategic recommendations, create draft responses, and deliver an executive briefing. Trigger with "process my emails", "email digest", "check my inbox", "triage my email", or "what's urgent in my inbox".
---

# Email Digest

Automated inbox processing: fetch, analyze, brief, respond.

## Process

### Step 1: Fetch Emails

```bash
python3 ${CLAUDE_PLUGIN_ROOT}/skills/email-digest/scripts/fetch_emails.py --hours 24
```

**Output**: JSON array of emails with sender, subject, body, timestamp, thread_id, labels.
**Options**: `--hours 24` (default), `--unread-only`, `--label INBOX`

### Step 2: Analyze Emails

For each email, determine:
- **Category**: urgent / respond / delegate / archive / irate
- **Sentiment**: positive / neutral / negative / irate
- **Urgency**: high / medium / low
- **Summary**: 1-sentence summary
- **Recommendation**: What to do and why
- **Draft response**: Suggested reply (if category is urgent or respond)

**Irate client detection**: If sentiment analysis detects anger, frustration, or escalation, flag as highest priority, generate immediate de-escalation response, and recommend same-day personal follow-up.

### Step 3: Post Executive Briefing

Format the briefing as:

```
Email Digest -- [Date]

URGENT (count)
- [Sender] -- [Subject] -- [Recommendation]

RESPOND (count)
- [Sender] -- [Subject] -- [Summary]

LOW PRIORITY (count)
- [count] emails archived
- [count] newsletters skipped
```

If Slack is configured, post via:

```bash
python3 ${CLAUDE_PLUGIN_ROOT}/skills/email-digest/scripts/post_to_slack.py --input analysis.json
```

### Step 4: Create Draft Responses (Optional)

```bash
python3 ${CLAUDE_PLUGIN_ROOT}/skills/email-digest/scripts/create_drafts.py --input analysis.json
```

Creates Gmail drafts for urgent and respond-category emails.

## Automation

Schedule with headless mode:

```bash
0 7 * * * claude -p "/email-digest" --output-format json >> logs/email-digest.log
```

## Edge Cases

- **No urgent emails**: Still deliver briefing with summary stats
- **Gmail API rate limit**: Built-in retry with exponential backoff
- **Very long threads**: Summarize the thread, don't analyze each message separately
- **Missing credentials**: Fail gracefully with setup instructions

## Environment Variables

```
GOOGLE_CLIENT_ID       # Gmail API
GOOGLE_CLIENT_SECRET   # Gmail API
SLACK_BOT_TOKEN        # Slack posting (optional)
SLACK_CHANNEL_ID       # Where to post briefings (optional)
```
