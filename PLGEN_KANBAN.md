# PLGen Kanban
Last updated: 2026-06-12

---

## Done

- [x] Node.js + Express + SQLite registry server
- [x] DigitalOcean droplet, pm2, nginx, SSL
- [x] Daily SQLite backup cron
- [x] `POST /api/labels/register` — free and registered tiers
- [x] `GET /api/labels/:plId` — public label JSON
- [x] `GET /api/labels` — member label history
- [x] `POST /api/members/signup` — admin-only
- [x] `GET /api/members/me` — member profile
- [x] `GET /health`, `GET /:plId`, `GET /dashboard`, `GET /new`
- [x] `POST /webhooks/stripe` — auto member creation on payment
- [x] Stripe Payment Links ($4/mo, $40/yr)
- [x] Resend welcome email with API key + Claude Project link
- [x] Main site — homepage, spec, join, nav, shared.css
- [x] GitHub Actions deploy workflow
- [x] Claude Project with pre-loaded session prompt (plgen-prompt-v3.md)
- [x] `plgen generate` — label + PLGT1 token
- [x] Anti-sycophancy enforcement in split estimation (v3.1)

---

## In Progress

- [ ] `plgen coach` — `ai_instructions/plgen-coach-v1.md` prompt built (2026-06-11)
  - Pending: `GET /api/members/average` backend endpoint
  - Pending: deploy to Claude Project

---

## Up Next

- [ ] `GET /api/members/average` — member baseline endpoint for coach
- [ ] Deploy `plgen coach` to Claude Project alongside generate
- [ ] Founding tier — 200 slots, $99 one-time

---

## Backlog

- [ ] Cancellation handling — `customer.subscription.deleted` deactivates member
- [ ] Add Claude Project link to `/join` page
- [ ] ChatGPT Custom GPT — equivalent of Claude Project for GPT users
- [ ] Proper auth — email + password replacing API key paste
- [ ] Member email for lost API key self-service
- [ ] Public label count / stats page
- [ ] `.plgen` file format spec for repos
- [ ] WordPress / Ghost plugin

---

## v1.1 Considerations (plgen coach)

- Coaching receipt attached to label at registration
- State 2 rewrite loop — human revises, reruns generate, coach re-evaluates
- `plgen check` as standalone command (vs. embedded Signal A trigger)
