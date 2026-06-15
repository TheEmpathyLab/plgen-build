# PLGen Cleanup & Consolidation — Findings
## Produced: 2026-06-15
## Mode: INVESTIGATE AND PROPOSE — no changes made, no files altered

---

## Step 1 — Ground Truth

### The "v3.3" mystery

Coach was written "assuming v3.3 exists." Here is what was actually found:

**The live Claude system prompt** is embedded directly inside `install/index.html`
in `~/github/provenancelabel`. The page displays "System Prompt v3.3" as its
version badge. This is the closest thing to a canonical v3.3 — but it has never
been saved as a standalone `.md` file in plgen-build. It exists only as HTML
source inside the install page.

This live v3.3 is substantively different from `plgen-prompt-v3.md` (v3.1) sitting
in plgen-build. It is NOT the same prompt. It is a newer, more complete architecture
using PLGen v1.2 schema and a JSON-to-dashboard registration flow.

**`custom-gpt/system-prompt-v3.md`** is also labeled v3.3 but is a different
prompt — the GPT-specific version. Same command names, different registration logic.

**`plgen.txt`** at `~/github/provenancelabel/plgen.txt` is v3.0 — the old
publicly downloadable prompt. It is missing the anti-sycophancy enforcement
section added in v3.1. It has not been updated to match the live v3.3.

---

### Location-by-location findings

**`~/github/plgen-build`** — OLDER LOCAL COPY (not the one we've been working in)
- Has `ai_instructions/plgen-prompt-v3.md` — v3.1 (anti-sycophancy included,
  April 2026 update). Same file as the Documents copy.
- Has `prompts/claude-project-instructions.md` — an EARLIER Claude Project
  instructions file, simpler, uses direct `POST /api/labels/register` from the AI.
  No version label. Predates the current architecture.
- Has `prompts/skill.md` — PLGen v1.1 skill file with `javascript_tool` fetch
  registration approach. Registry-aware.
- Has TWO session logs NOT in the Documents copy: `2026-06-02.md` and
  `2026-06-03.md`. These describe the versioning audit that declared v1.2 as
  current, the /start and /install page builds, and the CSS refactor. Important
  context missing from upstream.
- Git remote: `provenancelabel/plgen-build` via HTTPS. Last commit: June 3.
  This copy is AHEAD of the state before today's session.

**`~/Documents/GitHub/provenancelabel/plgen-build`** — CURRENT WORKING COPY
- Same `plgen-prompt-v3.md` (v3.1). No v3.3 file.
- Has coach, PLGEN_DASHBOARD.md, PLGEN_KANBAN.md — built today.
- Does NOT have the June 2-3 session logs. Missing the versioning history.
- Now ahead on upstream (pushed today).

**STATUS: The two local copies of plgen-build have diverged.** Each has commits
the other doesn't. They point to the same upstream remote. The upstream currently
reflects the Documents copy (which we pushed today). The June 2-3 logs from
`~/github/plgen-build` are NOT in upstream and will be lost if that copy is
deleted without pulling first.

---

**`~/github/provenancelabel`** — ACTIVE MAIN SITE REPO
- Contains the live v3.3 Claude system prompt embedded in `install/index.html`
- Contains `custom-gpt/system-prompt-v3.md` — GPT v3.3
- Has /start, /install, /embedded pages (June 2026 — most recent)
- `plgen.txt` is v3.0 — outdated

**`~/Documents/GitHub/provenancelabel/provenancelabel`** — OLDER MAIN SITE COPY
- Last commit: April 2026 (no /start, /install, /embedded)
- Both point to `provenancelabel/provenancelabel` on GitHub
- The `~/github/` copy is the active one — the Documents copy is stale

**`~/github/plgen-registry`** — REGISTRY BACKEND
- `GET /api/members/average` — **does NOT exist.** Not implemented, not stubbed.
  The only member endpoints are `POST /api/members/signup` and `GET /api/members/me`.
- No reference to `plgen coach` anywhere in the codebase.

**`plgen.txt` as version marker** — exists at `provenancelabel.org/plgen.txt` but
contains v3.0, not v3.3. Would mislead anyone using it to check the current version.

---

## Step 2 — Label Schema Variants

| File | Declared version | Field names in label output | session_init / estimated | Registry-aware | Approx. date |
|---|---|---|---|---|---|
| Claude Project SKILL.md (in-project only, not on disk) | v1.0 | Human Contribution, AI Contribution, Collaboration Method, AI Tool(s), Human Roles, AI Roles | No | No | Early 2026 |
| `prompts/skill.md` (~/github/plgen-build) | PLGen v1.1 | author, work_title, work_url, human_pct, ai_pct, ai_tools, process_notes, session_init, estimated | Yes | Yes (javascript_tool fetch) | Mar 2026 |
| `prompts/claude-project-instructions.md` (~/github/plgen-build) | No version label | author, work_title, work_url, human_pct, ai_pct, ai_tools, process_notes, session_init, estimated | Yes | Yes (direct POST) | Mar 2026 |
| `ai_instructions/plgen-prompt-v3.md` (both plgen-build copies) | PLGen Collaborator Prompt v3.1 | title, author, human, ai, tools, process, session_init, estimated (label format); human_pct / ai_pct in PLGT1 token | Yes | Yes (PLGT1 token) | Apr 2026 |
| `plgen.txt` (~/github/provenancelabel) | PLGen v3.0 | title, author, human, ai, tools, process, session_init, estimated; PLGT1 token | Yes | Yes (PLGT1 token) | Mar 2026 |
| `install/index.html` live Claude prompt (labeled "System Prompt v3.3") | PLGen v1.2 | JSON: plgen, author, date, work_title, human_pct, ai_pct, tools, human_role, ai_role, note, ref, shared_by | Tracked internally; not in JSON output | Yes (JSON → dashboard paste) | Jun 2026 |
| `custom-gpt/system-prompt-v3.md` | Custom GPT v3.3 | Short Badge, Long Disclosure, JSON (same fields as install v1.2) | Tracked; not in label output | Yes (dashboard) | Apr 2026 |
| `PLGEN_DASHBOARD.md` (Documents plgen-build) | References "[PROVENANCE: PLGen v1.0]" | title, author, human, ai, tools, process, session_init, estimated | Yes | Yes | Jun 2026 |
| `ai_instructions/plgen-coach-v1.md` | Coach v1.0 | References human_pct (from generate output) | N/A — diagnostic prompt | References /api/members/average | Jun 2026 |

**Referenced by other files (not orphaned):**
- v3.1 (`plgen-prompt-v3.md`) — referenced by PLGEN_DASHBOARD.md and the AI prompt page
- live v3.3 (install/index.html) — referenced by /install, /start, the version badge
- Coach v1.0 — references `human_pct` (matches v3.3 live, not v3.1)

**Appears orphaned or superseded:**
- `prompts/claude-project-instructions.md` — earlier architecture, not deployed
- `plgen.txt` v3.0 — live but outdated, not matching deployed prompt
- Claude Project SKILL.md v1.0 — original upload, schema predates everything else
- `~/Documents/GitHub/provenancelabel/provenancelabel` — stale copy of main site

---

## Step 3 — Coach Compatibility Against Real v3.3

The "real v3.3" for Claude is the content inside `install/index.html`. Here is
the compatibility check:

| Coach assumes | v3.3 (live) has | Compatible? |
|---|---|---|
| `plgen generate` as a named command | Yes — `plgen generate` closes session and outputs label | ✓ |
| `plgen coach` as a named command | Not listed anywhere | ✗ — coach is the FIRST document to introduce this command. v3.3 does not enumerate it. |
| `human_pct` field name in generate output | Yes — v3.3 uses `human_pct` in JSON output | ✓ |
| `plg_live_` key detection for member ID | Yes — v3.3 asks for key at session start | ✓ |
| `GET /api/members/average` endpoint | Does not exist in registry | ✗ — coach already handles this gracefully (falls to 85% baseline). Not a blocker but endpoint is unbuilt. |
| Gate check: `plgen generate` ran in current session | v3.3 session model supports this — generate closes the session | ✓ |
| Tone/field naming alignment | v3.3 uses Short Badge / Long Disclosure Block / JSON format labels | ✓ — coach references these format names correctly |

**Summary:** Coach has two incompatibilities with current state:
1. `plgen coach` is not enumerated in v3.3. It needs to be added to v3.3's
   command list before a V1 compiled file is valid.
2. `GET /api/members/average` is unbuilt. Coach degrades gracefully to 85%
   baseline — this is not a functional blocker, but the personalized baseline
   feature won't work until the endpoint is built.

Coach is OTHERWISE compatible with live v3.3. It is NOT compatible with v3.1
(which uses a different label output format — `human: X | ai: Y` in a plain-text
block, not `human_pct` in JSON).

---

## Step 4 — Proposed Cleanup Plan

**PROPOSAL ONLY. Nothing below should be executed until Shelton reviews and approves.**

---

### 4a — Canonical schema going forward

**Live v3.3 (install/index.html) is canonical.** PLGen v1.2, JSON registration
via dashboard, `plgen init / generate / register / status / validate / formats /
help / cancel` command set. This is what's deployed and what users actually see.

Before any V1 compile, v3.3 must be extracted from install/index.html and saved
as a committed file in plgen-build.

Proposed path: `ai_instructions/plgen-prompt-v3.3-claude.md`
(The GPT version already lives at `custom-gpt/system-prompt-v3.md`.)

---

### 4b — Files to mark superseded

Recommend a `superseded/` folder in plgen-build with a short note in each file
explaining what replaced it. Silent deletion loses history.

| File | Status | Reason |
|---|---|---|
| `prompts/skill.md` (v1.1) | Superseded | javascript_tool architecture replaced by dashboard-paste flow in v3.3 |
| `prompts/claude-project-instructions.md` | Superseded | Earlier project instructions approach; replaced by current system prompt |
| `ai_instructions/plgen-prompt-v3.md` (v3.1) | Superseded by v3.3 | Keep in place with header note; move to superseded/ after v3.3 is committed |
| `plgen.txt` (v3.0) | Needs update, not deletion | Public-facing downloadable; should be updated to v3.3 content so it matches the install page |
| Claude Project SKILL.md (v1.0, in-project only) | Superseded | Replace with v3.3 system prompt + coach in Claude Project knowledge |
| `~/Documents/GitHub/.../provenancelabel` (stale main site copy) | Archive locally | Stale copy 2 months behind; not worth maintaining two local main-site clones |

---

### 4c — What must be built before V1 compile

In priority order:

1. **Extract and commit v3.3 Claude prompt** — `ai_instructions/plgen-prompt-v3.3-claude.md`
   Required before compile. Without it, the V1 file has no committed source of
   truth for the core prompt.

2. **Add `plgen coach` to v3.3 command enumeration** — one line in the Commands
   section of v3.3. Without this, a user reading v3.3 has no idea coach exists.

3. **Merge the two local plgen-build copies** — pull June 2-3 session logs from
   `~/github/plgen-build` into the Documents copy (or push them to upstream).
   Then retire `~/github/plgen-build` as a working copy.

4. **Fix PLGEN_DASHBOARD.md schema reference** — currently shows "[PROVENANCE:
   PLGen v1.0]" with v3.1 fields. Should reflect v1.2 schema now that v3.3
   is canonical.

5. **Build `GET /api/members/average`** — needed for personalized coach baseline.
   Not a blocker (coach degrades gracefully), but required before the member
   experience is complete.

6. **Update `plgen.txt`** — currently v3.0. Should match the live system prompt
   version (v3.3) so the public downloadable is not misleading.

---

### 4d — Proposed V1 compiled file format

The V1 file is a single prompt that combines the core generate prompt and coach.
Recommended seam format:

```
<!-- MODULE: plgen-core v3.3 -->
[content of plgen-prompt-v3.3-claude.md]
<!-- /MODULE: plgen-core v3.3 -->

<!-- MODULE: plgen-coach v1.0 -->
[content of plgen-coach-v1.md]
<!-- /MODULE: plgen-coach v1.0 -->
```

This makes it trivially machine-parseable if a V2 builder needs to split modules.
The compiled file would live at: `ai_instructions/plgen-v1-compiled.md`

---

## Repo Situation Summary (for reference)

| Repo | Active copy | Stale copy | Action needed |
|---|---|---|---|
| plgen-build | `~/Documents/GitHub/provenancelabel/plgen-build` | `~/github/plgen-build` | Pull June 2-3 logs into active copy; retire stale copy |
| provenancelabel (main site) | `~/github/provenancelabel` | `~/Documents/GitHub/provenancelabel/provenancelabel` | Delete stale local copy |
| plgen-registry | `~/github/plgen-registry` | None | No duplicates |

---

*Findings produced 2026-06-15 | Claude Sonnet 4.6 | investigate-only mode*
*No files were modified in the production of this document.*
