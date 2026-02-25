# Connectors

This plugin works with the following external services. Connect them to unlock full functionality.

## Gmail

Used by: **email-digest**

Provides inbox access for fetching, analyzing, and drafting email responses.

### Setup

1. Create a Google Cloud project at [console.cloud.google.com](https://console.cloud.google.com)
2. Enable the Gmail API
3. Create OAuth 2.0 credentials (Desktop app type)
4. Download `credentials.json` to your workspace root or `auth/` directory
5. Run the auth flow to generate `token.json`

### Environment Variables

```
GOOGLE_CLIENT_ID
GOOGLE_CLIENT_SECRET
```

## Slack

Used by: **email-digest**, **research-lead**

Delivers briefings and lead review cards to Slack channels.

### Setup

1. Create a Slack app at [api.slack.com/apps](https://api.slack.com/apps)
2. Add the `chat:write` scope
3. Install to your workspace
4. Copy the Bot User OAuth Token

### Environment Variables

```
SLACK_BOT_TOKEN
SLACK_CHANNEL_ID
```

## Telegram

Used by: **telegram**

Routes messages between Telegram and Claude Code.

### Setup

1. Message [@BotFather](https://t.me/BotFather) on Telegram
2. Create a new bot with `/newbot`
3. Copy the bot token
4. Find your user ID by messaging [@userinfobot](https://t.me/userinfobot)
5. Add your user ID to `skills/telegram/references/messaging.yaml`

### Environment Variables

```
TELEGRAM_BOT_TOKEN
```

## Gamma

Used by: **gamma-slides**

Generates professional presentations from markdown content.

### Setup

1. Sign up at [gamma.app](https://gamma.app)
2. Get your API key from account settings

### Environment Variables

```
GAMMA_API_KEY
```

## Perplexity

Used by: **research-lead**

Deep company and market research with source citations.

### Environment Variables

```
PERPLEXITY_API_KEY
```

## OpenAI

Used by: **research-lead**, **telegram** (for mem0 fact extraction)

AI analysis for lead profiling and memory management.

### Environment Variables

```
OPENAI_API_KEY
```

## Pinecone

Used by: **telegram**, **memory-manager** (Tier 3)

Vector storage for semantic memory search. Free tier supports 100K vectors.

### Setup

1. Create a free account at [pinecone.io](https://pinecone.io)
2. Create an index (dimension: 1536, metric: cosine)
3. Copy the API key

### Environment Variables

```
PINECONE_API_KEY
```
