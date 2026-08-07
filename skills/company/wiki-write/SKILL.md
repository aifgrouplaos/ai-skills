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

Env: `WIKI_URL` + `WIKI_API_KEY` required. Missing either → notify user and stop. Never echo the key.

**Leading words:** *menu* · *focus* · *grill* · *working copy* · *handoff*

GraphQL read shapes + template→path map: [REFERENCE.md](REFERENCE.md).  
Prefer `curl` if Python SSL verify fails.

Speak locale-absolute paths (`/en/projects/<name>/…`). GraphQL `path` omits `en/`.

---

## Step 1 — Project name

Completion: user confirmed wiki slug `<project-name>`; target prefix is `projects/<project-name>/`.

1. Ask for the **project name** used on the wiki (may differ from the git repo name).
2. State the URL form: `$WIKI_URL/en/projects/<project-name>/`.

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

## Step 5 — Fill working copy

Completion: one markdown file written; template guidance blockquotes removed; placeholders replaced; in-body links locale-absolute (`/en/…`).

1. Working-copy path: `$TMPDIR/wiki-publish/<path-with-/-as-->.md`.
2. Write the filled page (title from first `#` unless overridden).
3. Show path + destination wiki URL form. Stop for the user to review.

---

## Step 6 — Handoff

Completion: user knows next action; session does not mutate Wiki.js.

1. Remind: publish with `/wiki-publish` for this path when ready.
2. Ask whether to run another **one-template** pass (new *menu* pick) or stop.

---

## Guards (positive form)

- One template per focused pass.
- Pull index for *menu*; pull the chosen template body only.
- Author working copies; `/wiki-publish` performs *mutate*.
- Exact project slug confirmed before any destination path is written.
