# PLGen — Claude Code Project Context

This file is automatically loaded at the start of every Claude Code session in this directory.

---

## Project

**PLGen (Provenance Label Generator)** — an open standard and registry for transparent disclosure of AI collaboration in creative work.

- **Owner:** Shelton Davis — shelton@empathylab.io (Empathy Lab)
- **Standard:** provenancelabel.org
- **Registry:** registry.provenancelabel.org
- **Registry source:** `/Users/shelton/github/plgen-registry/` (private)
- **Main site source:** `/Users/shelton/github/provenancelabel/` (GitHub Pages)
- **This repo:** `/Users/shelton/github/plgen-build/` — public build log

---

## Stack

| Layer | Tech |
|---|---|
| Registry server | Node.js + Express + SQLite (better-sqlite3) |
| Hosting | DigitalOcean droplet — 67.205.169.17 |
| Process manager | pm2 |
| Reverse proxy | nginx + certbot SSL |
| Payments | Stripe Payment Links + webhooks |
| Email | Resend |
| Main site | Static HTML/CSS — GitHub Pages |
| AI tooling | Claude Sonnet 4.6 via Claude Project |

---

## Key conventions

- **shared.css** is the single design system — all pages on both domains use it. No new fonts, no new CSS variables.
- **Navigation:**
  - provenancelabel.org: Logo · Spec · Join · Member Login ↗
  - registry.provenancelabel.org: Logo · New Label · My Labels · provenancelabel.org ↗
- **Auth:** `x-plgen-key` header (API key). Admin routes use `x-admin-key`.
- **Deploy:** push to GitHub → SSH into droplet → `cd /var/www/registry && git pull && pm2 restart registry`
- **SSH alias:** `ssh plgen` (configured in ~/.ssh/config)
- **Always use `nano`** for multiline content in SSH terminal — never heredoc.
- **Always use `bash /tmp/script.sh`** for long curl commands in terminal.

---

## Roadmap — next up

1. Founding tier — 200 slots, $99 one-time, Stripe payment link
2. Cancellation handling — `customer.subscription.deleted` webhook deactivates member
3. Add Claude Project link to `/join` page
4. ChatGPT Custom GPT — equivalent of Claude Project for GPT users

---

## "Log this session" workflow

When the user says **"log this session"** (or similar):

1. Generate a session log using the format below
2. Write it to `log/YYYY-MM-DD.md` (use today's date)
3. Commit and push to GitHub

### Log format

```
# Session Log — YYYY-MM-DD

**Focus:** [one-line summary of session theme]

---

## What was built

[Bullet list of concrete things added, changed, or fixed]

---

## Decisions made

[Key choices made this session, with brief reasoning]

---

## Issues encountered

[Any blockers, bugs, or unexpected problems — and how they were resolved]

---

## End state

[Where things stand at close of session — what's working, what's next]

---

*PL v1.0 | Shelton Davis + Claude Sonnet 4.6 | YYYY-MM-DD | Human X% · AI X%*
```

The provenance split at the bottom should be an honest estimate based on the session.

---

## "PLGen this" — label registration

When the user says **"PLGen this"**, follow the Claude Project instructions to calculate the split, collect author/title/URL, and POST to the registry.

Registry endpoint: `POST https://registry.provenancelabel.org/api/labels/register`
Header: `x-plgen-key: [key provided by user]`
