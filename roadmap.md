# PLGen Roadmap

Last updated: 2026-03-08

---

## Completed

### Infrastructure
- [x] Node.js + Express + SQLite registry server
- [x] DigitalOcean droplet — Ubuntu 22.04, $6/mo
- [x] pm2 process manager with auto-restart on reboot
- [x] nginx reverse proxy + certbot SSL
- [x] Daily SQLite backup cron job
- [x] GitHub private repo (`provenancelabel/registry`) with deploy key

### Registry API
- [x] `POST /api/labels/register` — free and registered tiers
- [x] `GET /api/labels/:plId` — public label JSON
- [x] `GET /api/labels` — member label history
- [x] `POST /api/members/signup` — admin-only member creation
- [x] `GET /api/members/me` — member profile
- [x] `GET /health` — uptime check
- [x] `GET /:plId` — public HTML viewer page
- [x] `GET /dashboard` — member dashboard (API key auth)
- [x] `GET /new` — label registration form
- [x] `POST /webhooks/stripe` — Stripe checkout webhook

### Payments & Onboarding
- [x] Stripe Payment Links — $4/mo and $40/yr
- [x] Webhook auto-creates member on successful payment
- [x] Resend welcome email with API key + Claude Project link

### Main Site (provenancelabel.org)
- [x] Homepage
- [x] Spec page (ported to shared.css)
- [x] Join page with pricing
- [x] Universal nav across all pages
- [x] GitHub Actions deploy workflow
- [x] shared.css design system

### AI Tooling
- [x] Claude Project with pre-loaded session prompt
- [x] Members paste key once, say "PLGen this" to register

---

## In Progress

- [ ] Founding tier — 200 slots, $99 one-time

---

## Next Up

- [ ] Cancellation handling — Stripe `customer.subscription.deleted` deactivates member
- [ ] Add Claude Project link to `/join` page
- [ ] ChatGPT Custom GPT — equivalent of Claude Project for GPT users

---

## Future

- [ ] Proper auth — email + password replacing API key paste
- [ ] Member email for lost API key self-service
- [ ] Public label count / stats page
- [ ] `.plgen` file format spec for repos
- [ ] WordPress / Ghost plugin
