# Architecture Decisions

---

## Database: SQLite over Postgres

**Decision:** Use SQLite via better-sqlite3.

**Reasoning:** Registry labels and members are write-light, read-heavy. SQLite in WAL mode handles concurrent reads well. Zero ops overhead — no separate database server, no connection pooling, no managed database cost. The entire database is a single file that backs up trivially. Revisit when concurrent writes become a bottleneck.

---

## Auth: API key over OAuth/session

**Decision:** Member identity is an API key (`plg_live_...`) sent as `x-plgen-key` header.

**Reasoning:** Simple to implement, simple to explain. Members receive it in email, paste it into Claude once per session. The key IS the identity — no login flow needed for MVP. Designed to be replaced by proper email + password auth when membership grows to a point where lost-key support becomes painful.

---

## Payments: Stripe Payment Links over embedded checkout

**Decision:** Use Stripe-hosted Payment Links rather than embedded Stripe Elements.

**Reasoning:** No PCI scope, no frontend payment code. Links are created in the Stripe dashboard — no code needed. Webhook handles the rest. Fast to ship, works forever.

---

## Deployment: DigitalOcean over Vercel/Railway

**Decision:** Single $6/mo DigitalOcean droplet with pm2 + nginx.

**Reasoning:** SQLite requires a persistent filesystem — serverless platforms don't support this. A simple VPS gives full control, persistent storage, and predictable cost. pm2 handles restarts, nginx handles SSL termination via certbot.

---

## Two-tier label system: free vs registered

**Decision:** Free labels get a PL ID and plain-text label only. Registered (member) labels get a permanent URL, public viewer page, and embeddable badge.

**Reasoning:** The standard itself is free forever — anyone can use PLGen. The registry adds verifiability and permanence, which is the member value proposition. Free tier drives adoption; registered tier drives revenue.

---

## AI tooling: Claude Project over per-session prompt

**Decision:** Create a Claude Project with session instructions pre-loaded rather than asking users to paste a prompt each time.

**Reasoning:** Target audience is writers and journalists, not developers. Asking them to paste a system prompt creates friction and failure points. A Project is a one-time setup that persists across every conversation — open it, work, say "PLGen this."
