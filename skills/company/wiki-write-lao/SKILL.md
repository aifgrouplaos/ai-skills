---
name: wiki-write-lao
description: >
  Translate one English wiki page (/en/…) into Lao — read English, write a
  Lao working copy under the Thai Wiki.js locale. Never publishes.
  User-invoked only.
disable-model-invocation: true
---

# Wiki write Lao

*Translate* one English **Wiki page** into its Lao **locale pair** at the same GraphQL path. Write ephemeral *working copy* files only — hand off to `/wiki-publish`.

Env: load `WIKI_URL` + `WIKI_API_KEY` from workspace `.env` (**those two keys only**). Missing file or either value → notify user and stop. Never echo the key. Never `source` `.env`.

**Leading words:** *translate* · *focus* · *working copy* · *index propagation* · *handoff* · *session English*

**Host constraint:** Wiki.js has no `lo` locale. Lao **document language** uses locale `th` (ADR-0003). URLs `/th/…`; working copies `.th.md`.

**Session language:** reply to the user in **English** for the whole run — status, questions, summaries, handoff. Lao belongs **only** in `.th.md` *working copy* files, not in chat.

Translation rules, path parsing, propagation tables: [REFERENCE.md](REFERENCE.md).  
Prefer `curl` if Python SSL verify fails.

Speak locale-absolute paths (`/th/…`). GraphQL `path` omits `th/`.

---

## Step 1 — Resolve target (*focus*)

Completion: exactly one GraphQL `path` is known; English source URL stated (`$WIKI_URL/en/<path>`).

1. Parse user input → GraphQL path (strip host, `/en/`, `/th/`). Examples: `/en/projects/lao-post/overview` → `projects/lao-post/overview`; `/en/standards/templates/workflow` → `standards/templates/workflow`.
2. **One path per run** — any English page (`/en/<path>`). Not limited to `projects/`.
3. If the user names multiple paths → ask which one for this pass.

---

## Step 2 — Load English + preflight

Completion: English markdown loaded; auth OK.

1. Confirm env; light auth check (`pages.list` or equivalent).
2. **English source** (in order):
   - `$TMPDIR/wiki-publish/<path-with-/-as-->.md` if present (unpublished English draft).
   - Else `singleByPath(path, locale: "en")` on the live wiki.
3. If neither exists → notify user and stop (an English `/en/<path>` page must exist first).

---

## Step 3 — Optional glossary

Completion: glossary pulled for term hints, or user declined / none exists.

Only when the user asks for consistent domain terms **and** the target is under `projects/<p>/`: pull one English glossary via `singleByPath` — `projects/<p>/backend/glossary` or `projects/<p>/frontend/glossary` (ask side if unclear). One cheap read; skip if missing or the target is not a project page.

---

## Step 4 — *Translate*

Completion: Lao markdown satisfies every rule in [REFERENCE.md](REFERENCE.md#translation).

1. *Translate* title, headings, prose, table cell text, Mermaid labels, and link text **into Lao** (not Thai).
2. Keep code blocks, identifiers, GraphQL paths, URL slugs, and API names in English.
3. Rewrite in-body links from `/en/…` to `/th/…`.
4. Mermaid: Wiki.js renders **8.8.2** — syntax supported by that release only.

---

## Step 5 — Write *working copy* (+ *index propagation*)

Completion: Lao `.th.md` written; upstream `th` index working copies patched only when the path is a project leaf/section; links use `/th/…`.

1. Working-copy path: `$TMPDIR/wiki-publish/<path-with-/-as-->.th.md`.
2. Title = translated first `#` heading unless the user overrides.
3. **Index propagation** — only when the path matches the project tables in [REFERENCE.md](REFERENCE.md#index-propagation). Otherwise write the one `.th.md` and skip. When it matches: pull live `th` indexes **first** (`locale: "th"`, exact paths only); patch links to `/th/<path>`; write `.th.md` index working copies. Skip if the user opts out.
4. Show every `.th.md` path + destination URL (`$WIKI_URL/th/<path>`). Stop for review.

---

## Step 6 — *Handoff*

Completion: user knows next action; session does not mutate Wiki.js.

1. Remind: publish with `/wiki-publish` for **every** `.th.md` working copy from this pass (page + any index patches). Use `/th/…` URLs so publish resolves `locale: "th"`.
2. Ask whether to *translate* another single path or stop.

---

## Guards (positive form)

- *Session English* — chat with the user in English; Lao output goes in `.th.md` files only.
- One GraphQL path per *focus* pass.
- English is canonical; *translate* the loaded English body into Lao — no template *menu*, no grill, no web research.
- Author `.th.md` working copies; `/wiki-publish` performs mutate at `locale: "th"`.
- Project leaf/section targets trigger `th` *index propagation* unless the user opts out; other `/en/…` paths write one `.th.md` only.
- Propagation pulls **index paths first**, exact paths only — no folder listings, no unrelated pages.
