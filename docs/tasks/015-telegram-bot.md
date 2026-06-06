---
id: "015"
title: "entrevista-telegram-bot: TelegramGateway, SessionRouter, MessageFormatter"
milestone: M4
priority: P0
estimate: 4h
blockedBy: ["014"]
blocks: ["018"]
parent: null
---

## Summary

Implement the thin Telegram protocol gateway in TypeScript/Node.js. No business logic — receives webhooks, proxies to conversation-lambda, formats text replies.

## Acceptance Criteria

- [ ] `/start {campaign_id}` deep link triggers `POST /conversations/start` on conversation-lambda.
- [ ] Incoming text messages proxy to `POST /conversations/{id}/message`.
- [ ] `MessageFormatter` escapes Telegram MarkdownV2 special characters.
- [ ] Lambda handler handles Telegram webhook signature verification (SECURITY-01 compliance).
- [ ] `npm test` passes.

## Context

| Document | Relevance |
|----------|-----------|
| `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/user-stories/stories.md` | US-01 |
| `AGENTS.md` | Telegraf 4.x, Node.js 20, TypeScript |

## Definition of Ready

- [ ] Task 014 complete (conversation-lambda API stable).
- [ ] Telegram Bot token and webhook URL pattern confirmed.
