---
name: provenance-label
description: Create and register a Provenance Label (PLGen) for AI-human collaborative work
---

# PLGen — Provenance Label Standard v1.1

## Overview

PLGen is an open standard for disclosing AI-human collaboration on creative, technical, or intellectual work. It produces a timestamped, registerable label with a permanent URL and embeddable badge.

**Trigger phrases:** `plgen this` · `generate my provenance label` · `PLGen this`

---

## Session Behavior

### On Session Start — Ask Once

Ask the user once at the beginning of any session where PLGen is in scope:

> "Are you a PLGen member? If yes, paste your API key (`plg_live_...`) and I'll register your label automatically. If not, no problem — I'll generate a label you can copy directly."

- Store the key silently for the session
- Do not ask again
- If key provided → use registered workflow (3a)
- If no key → use free text workflow (3b)

### Silent Tracking (from session start)

Track continuously throughout the session:
- Which ideas, directions, and decisions came from the human
- Which text / code / structure Claude drafted vs. human wrote or substantially revised
- Running split estimate (must sum to 100%)
- Flag: `session_init: "pre"` if PLGen was set up at session start, `"post"` if initialized mid/late session
- Flag: `estimated: true` if initialized after session start (add note to process_notes)

---

## On `plgen this`

### Step 1 — Calculate

Honestly assess the session:
- Human contribution: ideation, direction, content ownership, decisions, approvals, rewrites
- AI contribution: drafting, code generation, design, research, structure, implementation
- Percentages must sum to 100%
- Write 1–2 sentences describing how the collaboration worked

### Step 2 — Set flags

- `session_init`: `"pre"` if PLGen tracking started at session open, `"post"` if mid/late
- `estimated`: `true` if post-session, `false` if tracked from start
- If `estimated: true`, add to `process_notes`: *"Percentages estimated — label initialized after session start"*

### Step 3a — Registered member (API key present)

**Do NOT use curl or server-side HTTP calls.** Claude's container sandbox blocks outbound requests to external domains. Use JavaScript injected into the user's browser tab instead.

#### Pre-flight check
Ensure a tab is open on `registry.provenancelabel.org`. If not, navigate there first:
```
navigate to: https://registry.provenancelabel.org/new
```

#### Fire the registration via javascript_tool

```javascript
(async () => {
  const response = await fetch('https://registry.provenancelabel.org/api/labels/register', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'x-plgen-key': '[STORED_KEY]'
    },
    body: JSON.stringify({
      author: '[AUTHOR_NAME]',
      work_title: '[WORK_TITLE]',
      work_url: '[WORK_URL_OR_EMPTY_STRING]',
      human_pct: [HUMAN_PCT],
      ai_pct: [AI_PCT],
      ai_tools: '[AI_TOOL_AND_VERSION]',
      process_notes: '[1-2 sentence collaboration description]',
      session_init: '[pre|post]',
      estimated: [true|false]
    })
  });
  window._plgenResult = await response.json();
})();
```

Then retrieve the result with a second call:
```javascript
window._plgenResult
```

Wait ~2 seconds between the fetch call and the result retrieval.

#### Expected successful response
```json
{
  "pl_id": "PL-000002",
  "tier": "registered",
  "url": "https://registry.provenancelabel.org/PL-000002",
  "badge_html": "...",
  "member": "Shelton Davis",
  "created_at": "2026-03-09T21:28:40.317Z"
}
```

#### Return to user
```
✅ Label registered — PL-000002
🔗 https://registry.provenancelabel.org/PL-000002
🏷️ Badge HTML: [badge_html] (or: check dashboard if blocked by browser extension)
```

#### Known edge cases
- `badge_html` may return `[BLOCKED: Cookie/query string data]` if a browser security extension intercepts it — non-blocking, label is still registered. Direct user to dashboard to retrieve badge HTML manually.
- If `window._plgenResult` returns `undefined`, wait 1–2 more seconds and retry the retrieval call. The async IIFE may not have resolved yet.
- The fetch must run from a tab on `registry.provenancelabel.org` domain (same-origin). Cross-origin fetch from unrelated tabs may be blocked by CORS.

---

### Step 3b — Free / no key

Display plain-text label:

```
Provenance Label v1.1
─────────────────────────────
Author: [name]
Work: [title]
Human Contribution: [X]%
AI Contribution: [Y]%
AI Tool(s): [tool + version]
Collaboration: [1-2 sentences]
Session Init: [pre|post]
Estimated: [true|false]
─────────────────────────────
To register and get a permanent URL + embeddable badge:
→ provenancelabel.org
```

---

## API Contract Reference

**Endpoint:** `POST https://registry.provenancelabel.org/api/labels/register`

**Headers:**
```
Content-Type: application/json
x-plgen-key: plg_live_...   ← required for registered tier; omit for free
```

**Body fields:**
| Field | Type | Required | Notes |
|---|---|---|---|
| `author` | string | ✅ | Name or handle |
| `human_pct` | integer | ✅ | Must sum to 100 with ai_pct |
| `ai_pct` | integer | ✅ | Must sum to 100 with human_pct |
| `work_title` | string | optional | Title of piece or session |
| `work_url` | string | optional | URL of published work |
| `ai_tools` | string | optional | e.g. "Claude Sonnet 4.6" |
| `process_notes` | string | optional | How collaboration worked |
| `session_init` | string | optional | `"pre"` or `"post"` |
| `estimated` | boolean | optional | `true` if post-session init |

---

## What Doesn't Work (Do Not Attempt)

| Method | Why it fails |
|---|---|
| `curl` / bash from Claude container | Sandbox blocks outbound HTTP to external domains |
| Server-side `fetch` from container | Same restriction |
| UI form automation (click + type) | Brittle, error-prone, wrong interface for machine workflow |

**Canonical method: `javascript_tool` fetch from user's browser tab on registry domain.**

---

## Integration Notes for Future Improvement

- [ ] PLGen key could be stored in user memory via `memory_user_edits` so it doesn't need to be re-pasted each session
- [ ] `work_url` is currently empty for local/in-progress work — consider a naming convention for draft sessions
- [ ] Badge HTML retrieval blocked by some security extensions — registry could offer a separate `/api/labels/[id]/badge` endpoint as fallback
- [ ] Consider a `plgen status` command to pull current session tracking summary without registering
- [ ] `session_init: "post"` + `estimated: true` is the common real-world case — should be treated as default, not exception
