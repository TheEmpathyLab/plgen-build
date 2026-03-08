# PLGen Build Log

This repository documents the design, development, and deployment of the PLGen (Provenance Label Generator) system — built in collaboration with Claude Sonnet 4.6.

Every session is logged. Every major decision is recorded. The process is as transparent as the standard we're building.

---

## What is PLGen?

PLGen is an open standard for transparent disclosure of AI collaboration in creative work. A Provenance Label is a short declaration: who made this, when, and how much came from a human versus an AI.

- **Standard:** [provenancelabel.org](https://provenancelabel.org)
- **Registry:** [registry.provenancelabel.org](https://registry.provenancelabel.org)
- **Spec:** [provenancelabel.org/spec](https://provenancelabel.org/spec)

---

## This repo

| Directory | Contents |
|---|---|
| `log/` | Dated session entries — what was built, what was decided, what's next |
| `decisions/` | Architectural and product decisions with reasoning |
| `prompts/` | AI session prompts, Claude Project instructions |
| `roadmap.md` | Current state and next steps |

---

## Stack

| Layer | Tech |
|---|---|
| Registry server | Node.js + Express + SQLite (better-sqlite3) |
| Hosting | DigitalOcean ($6/mo droplet) |
| Process manager | pm2 |
| Reverse proxy | nginx + certbot (SSL) |
| Payments | Stripe (webhooks → auto member creation) |
| Email | Resend |
| Main site | Static HTML/CSS on GitHub Pages |
| AI tooling | Claude Sonnet 4.6 via Claude Project |

---

## Built with

This project is built in active collaboration with Claude Sonnet 4.6. Each session log includes a Provenance Label.

---

*Maintained by Shelton Davis — [shelton@empathylab.io](mailto:shelton@empathylab.io)*
