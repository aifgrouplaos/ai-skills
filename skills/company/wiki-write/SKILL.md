---
name: wiki-write
description: >
  Author one Engineering Wiki page from a live standards template — project
  name, pick a template, grill placeholders, write a working copy. Never
  publishes. User-invoked only.
disable-model-invocation: true
---

# Wiki write

Read templates from Wiki.js; write an ephemeral *working copy*. **Never** mutate the wiki — hand off to `/wiki-publish`.

Env: load `WIKI_URL` + `WIKI_API_KEY` from workspace `.env` (**those two keys only**). Missing file or either value → notify user and stop. Never echo the key. Never `source` `.env`.

**Leading words:** *menu* · *focus* · *grill* · *working copy* · *index propagation* · *handoff*

GraphQL read shapes + template→path map: [REFERENCE.md](REFERENCE.md).  
Prefer `curl` if Python SSL verify fails.

Speak locale-absolute paths (`/en/projects/<name>/…`). GraphQL `path` omits `en/`.

---

## Step 1 — Project name

Completion: wiki slug `<project-name>` resolved from the **wiki** `projects` index; target prefix is `projects/<project-name>/`.

1. Pull **`projects`** (index) via `singleByPath` — cheap, first. Existing slugs come **only** from that page. Never git repo, cwd, or local folder names.
2. If the user already named a slug that is on that index → use it; state `$WIKI_URL/en/projects/<project-name>/`; proceed to Step 2 — **do not ask**.
3. If the index has existing projects and none named → offer that wiki list (**one pick**). Still no free-form name ask.
4. New project (slug not on the index) → then ask for a new wiki slug. Confirm before Step 2.

---

## Step 2 — Template menu (thin pull)

Completion: user picked **exactly one** template from the live index.

1. `preflight`: confirm env; light auth check (`pages.list` or equivalent).
2. Pull **only** `standards/templates` (index page). Build a *menu* of available templates (name + one-line purpose from the index).
3. Ask which template to start with. **One choice.**

Do not pull other `standards/*` bodies yet.

---

## Step 3 — Focus pull + path

Completion: chosen template body loaded; destination GraphQL `path` known; leaf slugs asked if needed.

1. Pull **only** that template page (e.g. `standards/templates/workflow`).
2. Map to destination under `projects/<project-name>/…` per [REFERENCE.md](REFERENCE.md).
3. If the template needs a leaf slug (`<journey>`, `<system>`, `<runbook>`), ask for it **now** before grilling content.

---

## Step 4 — Grill from placeholders (*focus*)

Completion: every `<placeholder>` in the chosen template has an answer, `todo`/`n/a`+Reason, or an explicit skip the user approved.

1. Run `/grilling` (or `/grill-me`) scoped to **this template only**.
2. Derive questions from: placeholders, the template’s “what this answers / must NOT contain” guidance, and Status fields.
3. Cold start (no project docs): grill carries product facts. If code exists, use it to pre-fill and only grill gaps.
4. Stay in *focus* — do not interview for other templates in this pass.

---

## Step 5 — Fill working copy (+ index propagation)

Completion: leaf/section markdown written; upstream index working copies patched when applicable; template guidance blockquotes removed; placeholders replaced; in-body links locale-absolute (`/en/…`).

1. Working-copy path: `$TMPDIR/wiki-publish/<path-with-/-as-->.md`.
2. Write the filled page (title from first `#` unless overridden).
3. Mermaid diagrams: Wiki.js renders **Mermaid 8.8.2** — use only syntax supported by that version.
4. **Index propagation** — per [REFERENCE.md](REFERENCE.md):
   - **Leaf** (`workflow`, `integration`, `runbook`): pull live indexes **first** — section index, then project root (`singleByPath` each; no other reads); patch link to the new leaf; write index working copies.
   - **Section** (`overview`, `backend`, `frontend`, `workflows`, `integrations`, `runbooks`, `api`): pull live **project root** first; patch link to the section; write project-root working copy.
   - Propagation table paths only — never list folders or pull unrelated project pages.
   - Skip only when the user explicitly opts out.
5. Show every working-copy path + destination wiki URL. Stop for the user to review.

---

## Step 6 — Handoff

Completion: user knows next action; session does not mutate Wiki.js.

1. Remind: publish with `/wiki-publish` for **every** working copy from this pass (leaf/section + any index patches) when ready. For Lao, run `/wiki-write-lao` then publish the `.th.md` copies with `/th/…` URLs.
2. Ask whether to run another **one-template** pass (new *menu* pick) or stop.

---

## Guards (positive form)

- One template per focused pass.
- Pull index for *menu*; pull the chosen template body only; propagation pulls **index paths first**, exact paths only — no folder listings, no unrelated pages.
- Author working copies; `/wiki-publish` performs *mutate*.
- Leaf and section pages trigger *index propagation* unless the user opts out.
- Project slugs come from the wiki `projects` index only — never git repo or cwd. Skip the name ask when the slug is already on that index; ask only for a **new** wiki project.
