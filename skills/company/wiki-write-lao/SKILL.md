---
name: wiki-write-lao
description: >
  Translate one English Engineering Wiki page into Lao — read English,
  write a Lao working copy. Never publishes. User-invoked only.
disable-model-invocation: true
---

# Wiki write Lao

*Translate* one English **Wiki page** into its Lao **locale pair** at the same GraphQL path. Write ephemeral *working copy* files only — hand off to `/wiki-publish`.

Env: `WIKI_URL` + `WIKI_API_KEY` required. Missing either → notify user and stop. Never echo the key.

**Leading words:** *translate* · *focus* · *working copy* · *index propagation* · *handoff*

Translation rules, path parsing, propagation tables: [REFERENCE.md](REFERENCE.md).  
Prefer `curl` if Python SSL verify fails.

Speak locale-absolute paths (`/lo/projects/<name>/…`). GraphQL `path` omits `lo/`.

---

## Step 1 — Resolve target (*focus*)

Completion: exactly one GraphQL `path` is known; English source URL stated (`$WIKI_URL/en/<path>`).

1. Parse user input → GraphQL path (strip host, `/en/`, `/lo/`). Examples: `/en/projects/lao-post/overview` → `projects/lao-post/overview`.
2. **One path per run** — a project leaf, section, or project root under `projects/<p>/…`.
3. If the user names multiple paths → ask which one for this pass.

---

## Step 2 — Load English + preflight

Completion: English markdown loaded; auth OK.

1. Confirm env; light auth check (`pages.list` or equivalent).
2. **English source** (in order):
   - `$TMPDIR/wiki-publish/<path-with-/-as-->.md` if present (unpublished English draft).
   - Else `singleByPath(path, locale: "en")` on the live wiki.
3. If neither exists → notify user and stop (author English with `/wiki-write` first).

---

## Step 3 — Optional glossary

Completion: glossary pulled for term hints, or user declined / none exists.

Only when the user asks for consistent domain terms: pull one English glossary via `singleByPath` — `projects/<p>/backend/glossary` or `projects/<p>/frontend/glossary` (ask side if unclear). One cheap read; skip if missing.

---

## Step 4 — *Translate*

Completion: Lao markdown satisfies every rule in [REFERENCE.md](REFERENCE.md#translation).

1. *Translate* title, headings, prose, table cell text, Mermaid labels, and link text.
2. Keep code blocks, identifiers, GraphQL paths, URL slugs, and API names in English.
3. Rewrite in-body links from `/en/…` to `/lo/…`.
4. Mermaid: Wiki.js renders **8.8.2** — syntax supported by that release only.

---

## Step 5 — Write *working copy* (+ *index propagation*)

Completion: Lao leaf/section `.lo.md` written; upstream Lao index working copies patched when applicable; links use `/lo/…`.

1. Lao working-copy path: `$TMPDIR/wiki-publish/<path-with-/-as-->.lo.md`.
2. Title = translated first `#` heading unless the user overrides.
3. **Index propagation** — per [REFERENCE.md](REFERENCE.md#index-propagation): pull live Lao indexes **first** (`locale: "lo"`, exact paths only); patch links to `/lo/<path>`; write `.lo.md` index working copies.
   - **Leaf** (`…/workflows/<slug>`, `…/integrations/<slug>`, `…/runbooks/<slug>`): section index, then project root.
   - **Section** (`overview`, `backend`, `frontend`, `workflows`, `integrations`, `runbooks`, `api`): project root only.
   - Skip only when the user explicitly opts out.
4. Show every `.lo.md` path + destination URL (`$WIKI_URL/lo/<path>`). Stop for review.

---

## Step 6 — *Handoff*

Completion: user knows next action; session does not mutate Wiki.js.

1. Remind: publish with `/wiki-publish` for **every** `.lo.md` working copy from this pass (leaf/section + any index patches). Use `/lo/…` URLs so publish resolves `locale: "lo"`.
2. Ask whether to *translate* another single path or stop.

---

## Guards (positive form)

- One GraphQL path per *focus* pass.
- English is canonical; *translate* the loaded English body — no template *menu*, no grill, no web research.
- Author `.lo.md` working copies; `/wiki-publish` performs mutate.
- Leaf and section targets trigger Lao *index propagation* unless the user opts out.
- Propagation pulls **index paths first**, exact paths only — no folder listings, no unrelated pages.
