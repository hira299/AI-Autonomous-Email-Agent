# 📧 AI Autonomous Email Agent

> **Stack:** n8n · Gmail API · Google Sheets · Groq · Cloudflare Tunnel  
> **Status:** Production — Live & Running  
> **Loom Walkthrough:** https://www.loom.com/share/9fbd2526672348f2b8c4284933817e39

---

## 🧩 The Problem

High operational overhead and latency in manual email screening and Tier-1 inquiry resolution. Responding to every recruiter or client email manually is time-consuming and inconsistent — especially across time zones.

---

## ⚙️ The Solution

A stateful, event-driven AI email agent that autonomously handles incoming email conversations on my behalf. The system maintains full conversation memory per thread, generates context-aware replies using a Groq-powered LLM, and responds directly to the sender — all with zero manual input.

**Total infrastructure cost: $0**

---

## 🏗️ Architecture

```
Gmail Trigger (every 60s)
  → Extract Email Fields
  → Load Chat History (Google Sheets — by thread ID)
  → Build Messages Array (system prompt + history + new message)
  → AI Response Generator
      → LLM: Groq / Llama 3
  → Send Gmail Reply          → Save AI Reply (Google Sheets)
  → Mark Message as Read      → Save User Message (Google Sheets)
```

---

## 🔑 Key Features

- **Stateful memory** — loads full conversation history by thread ID from Google Sheets, so the agent never loses context across multiple emails
- **Context-aware replies** — system prompt is injected with full background context before every LLM call
- **Parallel persistence** — both the user message and AI reply are saved to Google Sheets simultaneously after each exchange
- **Auto inbox management** — marks processed emails as read automatically
- **Zero server cost** — runs on self-hosted n8n exposed via Cloudflare Tunnel

---

## 🗄️ Memory Layer (Google Sheets)

Each conversation is stored with:

| Column | Description |
|---|---|
| `thread_id` | Gmail thread ID — used to group conversations |
| `role` | `user` or `assistant` |
| `content` | Full message body |
| `timestamp` | ISO timestamp |
| `sender` | Sender email address |

---

## 🚀 Setup Instructions

### Prerequisites
- n8n instance (self-hosted via Docker recommended)
- Gmail account with OAuth2 credentials configured in n8n
- Google Sheets with the memory table schema above
- Groq API key (free tier works)
- Cloudflare Tunnel (for public webhook access — optional)

### Steps
1. Import `AI_Interview_Bot.json` into your n8n instance
2. Connect credentials: Gmail OAuth2, Google Sheets, Groq API
3. Update the Google Sheets node to point to your sheet
4. Customize the system prompt inside `Build Messages Array` with your own context
5. Activate the workflow

### System Prompt
The system prompt lives inside the `Build Messages Array` code node. Replace it with your own background, experience, and instructions to make the agent respond as you.

