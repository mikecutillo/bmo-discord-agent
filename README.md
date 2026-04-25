# BMO — Family AI Companion

A 24/7 Discord-based AI companion for the Cutillo household. Runs as a supervised TypeScript service that lives inside the family server and acts as a shared household brain — a single "voice" across 21 Discord channels that handles check-ins, curfew tracking, chore reminders, and family-wide announcements.

> *Built inside AIOS (my personal AI Operating System). This repo is a public overview — the running code is private because it's wired into family infrastructure, Discord IDs, and Notion configuration.*

---

## What it does

- **Answers kids on demand.** DMs to BMO trigger intent-routed replies — curfew status, Pulse survey questions, homework reminders, "is dinner ready" style questions.
- **Runs scheduled family rituals.** Morning briefings, evening Pulse check-ins, weekend plans, reminders — all fired by a scheduler and delivered to the right channel.
- **Watches for state changes.** Device online/offline events, curfew boundaries, calendar changes — surfaced to the relevant channel with the right tone.
- **One voice across channels.** Tone adapts per channel and per person, but the underlying personality stays consistent — BMO sounds like BMO whether it's talking to a 12-year-old about chores or posting a weekly family digest.

## How it's built

| Layer | Tech |
|---|---|
| Runtime | Node.js + TypeScript, `discord.js` |
| LLM | Anthropic Claude (natural replies, intent classification) |
| Config store | Notion (6 databases — Capabilities, Channels, People, Personality, Pulse Questions, Incidents) |
| Supervision | macOS `launchd` with auto-restart |
| Control surface | `/bmo` slash command + a live ops page in AIOS |

## The capability registry

BMO doesn't ship with hard-coded behaviors. Every thing it does is a **capability row in Notion**, executed by one of six executor types:

1. **`scheduled-summary`** — cron-style, composes and posts a digest
2. **`intent-response`** — matches a DM to an intent, replies
3. **`state-watcher`** — reacts to external state changes (devices, calendar, curfew)
4. **`survey`** — runs a multi-step Pulse check-in with stored results
5. **`external-api`** — calls a third-party service and formats the result
6. **`static-response`** — deterministic fallback for known prompts

**Why this matters:** I add new behaviors by writing a Notion row — no code change, no redeploy. Today BMO has **20 active capabilities** across those six types. Same registry drives admin dashboards inside AIOS.

## What this demonstrates

- **Agent-style LLM orchestration** without reaching for a framework — explicit executor types, deterministic fallback paths, clear separation of intent vs. side effect
- **Config-as-data architecture** — Notion as the editable source of truth for a production system
- **Real-world durability** — runs unattended under `launchd`, recovers from network blips, gets daily use by three kids
- **Voice consistency at scale** — one personality model, 21 channel contexts, guided by a central Personality database

## Stack

![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat&logo=nodedotjs&logoColor=white)
![Discord.js](https://img.shields.io/badge/Discord.js-5865F2?style=flat&logo=discord&logoColor=white)
![Anthropic Claude](https://img.shields.io/badge/Anthropic_Claude-CC785C?style=flat&logoColor=white)
![Notion API](https://img.shields.io/badge/Notion_API-000000?style=flat&logo=notion&logoColor=white)
![launchd](https://img.shields.io/badge/launchd-000000?style=flat&logo=apple&logoColor=white)

## Related in the AIOS Portfolio

- **[AIOS](https://github.com/mikecutillo/aios)** — Personal AI OS host; Next.js dashboard orchestrating 16+ household and business modules
- **[Household Voice Control](https://github.com/mikecutillo/household-voice-control)** — Voice-interface layer bridging Alexa to a local AI OS via custom skill + Lambda
- **[Household Digest](https://github.com/mikecutillo/household-digest)** — AI-composed daily digest pipeline; Gmail, Calendar, M365 to Discord every morning

---

Part of [AIOS](https://github.com/mikecutillo) — my personal AI Operating System. See the [profile README](https://github.com/mikecutillo) for the full system map.
