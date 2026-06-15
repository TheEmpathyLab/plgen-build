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

### PLGen Skills System
A community skills directory — the Obsidian model for AI collaboration transparency.

PLGen owns the standard and the registry, not the prompts. Anyone who understands
the PLGen skill format can write and publish a skill. provenancelabel.org/skills
becomes a community directory, not a page Shelton maintains alone.

**Core infrastructure:**
- [ ] `/skills` page on provenancelabel.org — community directory, searchable, rated
- [ ] Skills are platform-agnostic prompt files (Claude, GPT, others)
- [ ] Open submission — anyone can contribute a skill
- [ ] Users load only what they need — `plgen generate` to start, `plgen coach` when ready
- [ ] Versioned skill files — users can pin to a version or pull latest
- [ ] Paid path stays narrow: registry sync, personalization, verified skills — not the ecosystem

**Community skill examples (we can't predict what gets built):**
- [ ] `plgen coach --academic` — discipline-aware coaching for students
- [ ] `plgen coach --journalism` — source attribution and AI disclosure for reporters
- [ ] Domain variants, language adaptations, workflow integrations

**Institutional use case — universities and professors:**
The most powerful surface we can't fully design ourselves. A professor or university
could publish a PLGen skill scoped to their course, department, or field — defining
what responsible AI use looks like for their students specifically. A biology lab and
a creative writing program have completely different standards. PLGen provides the
framework; the institution provides the context.

- [ ] Skill format spec — documented standard for third-party skill authors
- [ ] Institutional skill namespace (e.g. `plgen coach --mit-sloan`, `plgen coach --uw-journalism`)
- [ ] Skill submission and review process for the community directory

### Other
- [ ] Proper auth — email + password replacing API key paste
- [ ] Member email for lost API key self-service
- [ ] Public label count / stats page
- [ ] `.plgen` file format spec for repos
- [ ] WordPress / Ghost plugin
