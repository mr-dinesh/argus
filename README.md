# Argus — Argument Intelligence

**Stress-test any claim, URL, or article from every angle — steelman, strawman, synthesis.**

→ [Live](https://argus-proxy.mrdinesh.workers.dev/) · Part of the [Vibe Coding series](https://mrdee.in/vibecoding/vibecoding-016-argus/)

---

## What it does

Paste a claim, a URL, or an article. Argus auto-detects the input type and produces three panels:

- **Steelman** — The strongest possible version of the argument
- **Strawman** — The weakest, most attackable version
- **Synthesis** — What a rigorous, disinterested thinker concludes, plus the real crux and one actionable implication

## Deploy

```bash
npm install -g wrangler
wrangler secret put GROQ_API_KEY   # free tier at console.groq.com
wrangler deploy
```

Local dev:

```bash
wrangler dev   # http://localhost:8787
```

## Architecture

Single Cloudflare Worker (`worker.js`):

```
GET  /         → full HTML/CSS/JS app (self-contained)
POST /fetch    → proxies URL fetch server-side (solves CORS)
POST /analyse  → calls Groq API (llama-3.3-70b-versatile)
```

Rate limiting via KV: 20 req/90s on `/fetch`, 10 req/90s on `/analyse` per IP.

## Stack

- Cloudflare Workers
- Groq API — `llama-3.3-70b-versatile` (free tier)
- KV namespace for rate limiting
