# PLGen Dashboard
Last updated: 2026-06-15

---

## CLI Command Suite

| Command | Status | Description |
|---|---|---|
| `plgen init` | Live | Opens a tracked session |
| `plgen generate` | Live | Produces the label (PLGen v1.2 JSON) |
| `plgen coach` | Built — pending deploy | Evaluates the split, delivers feedback |
| `plgen register` | Live | Posts label to registry |
| `plgen validate` | Live | Checks label integrity |
| `plgen status` | Live | Reports current session state |
| `plgen help` | Live | Lists commands |
| `plgen cancel` | Live | Ends session without registering |

---

## Registry API Endpoints

| Endpoint | Status | Notes |
|---|---|---|
| `POST /api/labels/register` | Live | Free and registered tiers |
| `GET /api/labels/:plId` | Live | Public label JSON |
| `GET /api/labels` | Live | Member label history |
| `POST /api/members/signup` | Live | Admin-only member creation |
| `GET /api/members/me` | Live | Member profile |
| `GET /api/members/average` | **Needed** | Member baseline for `plgen coach` — returns `{ human_avg, session_count }` |
| `GET /health` | Live | Uptime check |
| `GET /:plId` | Live | Public HTML viewer |
| `GET /dashboard` | Live | Member dashboard (API key auth) |
| `GET /new` | Live | Label registration form |
| `POST /webhooks/stripe` | Live | Stripe checkout webhook |

---

## AI Prompt Files

| File | Version | Status |
|---|---|---|
| `ai_instructions/plgen-prompt-v3.3-claude.md` | v3.3 | **Current** — PLGen v1.2 schema, `plgen init/generate/register` |
| `ai_instructions/plgen-coach-v1.md` | v1.0 | Built — pending deploy |
| `superseded/plgen-prompt-v3.md` | v3.1 | Superseded by v3.3 |
| `superseded/plgen-prompt-v1.md` | v1 | Superseded |
| `superseded/plgen-prompt-v2.md` | v2 | Superseded |
| `superseded/prompts-skill.md` | v1.1 | Superseded — javascript_tool arch replaced |
| `superseded/prompts-claude-project-instructions.md` | — | Superseded — earlier project instructions approach |

---

## Infrastructure

| Component | Status |
|---|---|
| Node.js + Express + SQLite registry | Live |
| DigitalOcean droplet (Ubuntu 22.04, $6/mo) | Live |
| pm2 process manager | Live |
| nginx + certbot SSL | Live |
| Daily SQLite backup cron | Live |
| GitHub private registry repo | Live |
| Stripe payments ($4/mo, $40/yr) | Live |
| Stripe webhook → auto member creation | Live |
| Resend welcome email | Live |

---

## Open Items

- [ ] `GET /api/members/average` endpoint — required for `plgen coach` member baseline
- [ ] Founding tier — 200 slots, $99 one-time
- [ ] Cancellation handling — `customer.subscription.deleted` deactivates member
- [ ] Add Claude Project link to `/join` page
