# ⚖️ Lawgic AI — Indian Legal Assistant
 
Lawgic AI is an AI-powered legal chatbot specializing in Indian law. It answers questions about the Constitution of India, landmark Supreme Court judgements, central and state legislation, and the latest legal developments — in clear, accessible language.
 
---
 
## ✨ Features
 
- 🏛️ Deep knowledge of the **Constitution of India** — all Articles, Schedules, and Amendments
- ⚖️ Coverage of landmark **Supreme Court & High Court** judgements
- 📜 Familiar with major legislation — IPC, CrPC, POCSO, UAPA, RTI, CAA, GST, and more
- 🔍 Cites exact **Article and Section references** in every response
- 💬 Multi-turn **conversation memory** within a session
- 📱 Clean, responsive UI that works on desktop and mobile
- ⚠️ Built-in disclaimer reminding users to consult a qualified advocate
 
---
 
## 🛠️ Tech Stack
 
| Layer | Technology |
|---|---|
| **Frontend** | Vanilla HTML, CSS, JavaScript — single file, zero dependencies |
| **LLM** | [Groq API](https://console.groq.com) — `llama-3.3-70b-versatile` (free tier) |
| **API Proxy** | [Cloudflare Workers](https://workers.cloudflare.com) — serverless edge function that hides the API key |
| **Hosting** | [GitHub Pages](https://pages.github.com) — free static hosting |
| **Rate Limiting** | Cloudflare Workers KV — IP-based counter, 10 requests/min per user |
| **Fonts** | Playfair Display · DM Sans · DM Mono (Google Fonts) |
 
---
 
## 🏗️ Architecture
 
```
User (Browser)
     │
     │  5s cooldown enforced client-side
     ▼
GitHub Pages          ← hosts the static HTML/CSS/JS frontend
     │
     │  POST /messages
     ▼
Cloudflare Worker     ← serverless proxy, keeps API key hidden & encrypted
     │    │
     │    └── Cloudflare KV ← checks IP request count (max 10/min)
     │         if limit hit → 429 Too Many Requests
     │
     │  Groq API call (only if allowed)
     ▼
Groq (LLaMA 3.3 70B)  ← LLM inference, returns legal response
```
 
The API key is stored as an **encrypted secret** inside Cloudflare Workers and is never exposed to the browser or the frontend code.
 
### Rate Limiting — Two Layers
 
| Layer | Where | How |
|---|---|---|
| **Client-side cooldown** | Browser (HTML) | 5 second delay enforced between messages per session |
| **Server-side IP throttling** | Cloudflare Worker + KV | Max 10 requests per minute per IP — returns `429` if exceeded |
 
This dual-layer approach prevents both accidental spam from legitimate users and deliberate abuse from bad actors trying to exhaust the Groq API quota.
 
