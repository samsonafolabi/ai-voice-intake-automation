# AI Voice Intake & Dispatch Automation (HVAC Use Case)

**An end-to-end automation that turns a voice note into a triaged, logged, and (if urgent) instantly escalated service request — built on n8n, Telegram, and Groq.**

---

## The Problem

Service businesses like HVAC, plumbing, and electrical contractors lose real revenue every time an urgent call comes in after hours or during a busy shift. A missed "no heat" or "water leak" call doesn't just cost one job — it costs a customer relationship, and sometimes a safety issue goes unaddressed for hours.

Meanwhile, dispatchers and office staff spend hours a week manually listening to voicemails, typing up job notes, and deciding what's urgent versus what can wait — all before a technician is even assigned.

## The Solution

This workflow automates that entire intake-to-triage process:

1. A customer or field technician sends a **voice note** (via Telegram — easily swappable for SMS/WhatsApp/phone in a production deployment)
2. The audio is **transcribed** automatically
3. An LLM **classifies urgency** (emergency vs. routine) and extracts structured details — issue type, equipment involved, and a one-line summary
4. If it's an **emergency**, the on-call dispatcher gets an **instant alert** with all the relevant details
5. Every request — urgent or not — is logged to a **searchable job history**, so nothing gets lost and past issues can be looked up by customer or keyword later

No call gets missed. No manual note-taking. Every job is triaged and documented automatically, in seconds.

## How It Works (Architecture)

```
Voice Note (Telegram)
        |
Filter: is it actually a voice message?
        |
Download audio file
        |
Transcribe (Groq Whisper API)
        |
Classify urgency + extract details (Groq LLM)
        |
Parse structured response
        |
   IF emergency?
   /          \
 YES           NO
  |             |
Alert          (skip)
dispatcher       |
  \             /
   Log to Google Sheets (job history)
```

## Stack

| Component | Tool |
|---|---|
| Workflow orchestration | [n8n](https://n8n.io) |
| Voice capture | Telegram Bot API |
| Transcription | Groq Whisper API (`whisper-large-v3`) |
| Classification | Groq LLM API (`openai/gpt-oss-20b`) |
| Alerting | Telegram Bot API |
| Job history / CRM log | Google Sheets |

This is built with lightweight, easily swappable components — the same pipeline works with Twilio or WhatsApp for voice capture, Airtable or a proper database for job history, or Slack/SMS for alerting, depending on what a business already uses.

## Why This Pattern Matters

The same architecture — **capture → transcribe → classify → route → log** — isn't specific to HVAC. It's a general-purpose intake pattern that adapts to:

- Plumbing / electrical / general contracting dispatch
- Legal intake (new client call triage)
- Real estate lead qualification
- Medical/dental appointment triage
- Any business where "someone calls or messages and a human has to decide what happens next" is currently a manual bottleneck

Swapping industries is mostly a matter of rewriting one system prompt (the classification instructions) — the rest of the pipeline stays the same. That makes this less a single tool and more a template for automating triage across any service-based business.

## Demo

📹 [Watch the demo video](#) *https://drive.google.com/file/d/1bV5vgbgYqEZBQic9zRV-sZHbUffvlmty/view?usp=sharing*

The demo shows:
- A live emergency voice note triggering an instant dispatcher alert
- A live routine voice note logging silently with no alert
- The resulting job history in Google Sheets

## Workflow File

The full n8n workflow (importable JSON) is included in this repo: [`HVAC Inbound Voicenotes_Calls`](HVAC Inbound Voicenotes_Calls)

## Built By

*[Afolabi Samson / afolabisamson20@gmail.com]*
