---
name: wiki-publish
description: >
  Publish markdown to company Wiki.js via GraphQL — bootstrap an ephemeral
  working copy, then create or update live pages. User-invoked only.
disable-model-invocation: true
---

# Wiki publish (Wiki.js GraphQL)

Endpoint: `POST $WIKI_URL/graphql`  
Auth: `Authorization: Bearer $WIKI_API_KEY` (env / `.env` — never print the key)

**Leading words:** *preflight* · *working copy* · *index propagation* · *plan* · *APPROVE* · *mutate* · *cleanup*

Mutation shapes: [REFERENCE.md](REFERENCE.md).

**Durable store = Wiki.js.** Working copies are ephemeral under `$TMPDIR/wiki-publish/`. Never write wiki markdown into a project repo (no `docs/wiki-packs/`).

Speak **locale-absolute** URLs to the user (`/en/…`, `/th/…`). GraphQL `path` omits the locale prefix (locale is a separate field). Never use markdown hrefs `./standards/…` or `../…` — Wiki.js nests those under the current page.

---

## Step 0 — Resolve target + working copy

Completion: every target has `locale`, GraphQL `path`, working-copy path, intended `title`, and a create/update intent once preflight runs.

1. Parse user request into wiki path(s) (`/en/standards/templates/overview` or `./standards/templates/overview` → GraphQL path `standards/templates/overview`).
2. Locale: `/en/…` → `locale: "en"`; `/th/…` → `locale: "th"`; **path has no locale prefix**.
3. Working-copy path: `$TMPDIR/wiki-publish/<path-with-/-as-->.md` for `en`; same stem with `.th.md` for `th` (e.g. `projects-foo-overview.th.md`).
4. If the working copy is missing → **Step 0b (bootstrap)**. If present → use it as `content`.
5. Title = first `#` heading unless the user overrides.
6. If authoring markdown that will be published, in-body links must be locale-absolute (`/en/…` or `/th/…` matching the page locale), not `./` or `../`.
7. **Index propagation:** when the target is a project leaf or section, expand the target set per [REFERENCE.md](REFERENCE.md#index-propagation) (section index + project root for leaves; project root for sections). Use existing index working copies; if missing, pull live **index page(s) first** (`singleByPath`, propagation-table paths only) + patch before Plan B.

### Step 0b — Bootstrap (no working copy)

Completion: user has chosen a mode; **Plan A** is ready (no file written yet).

Offer exactly one mode per missing target:

| Mode | Source of `content` |
|---|---|
| **stub** | Minimal markdown: `# <title>` + short placeholder body |
| **pull template** | Live wiki page under `standards/templates/…` matching the leaf type; if missing → **stub** |
| **pull live** | `pages.singleByPath` for the target path; if missing → **stub** |

Show **Plan A**:

| path | working copy | mode | title |
|---|---|---|---|
| … | `$TMPDIR/…` | stub \| pull template \| pull live | … |

**Stop.** Wait for exact `APPROVE`. Then write the working copy only. Print the file path. **Stop again** for human edit — do not enter Plan B until the user says they are ready or re-invokes `/wiki-publish`.

---

## Step 1 — Preflight

Completion: auth OK; for each path, parents exist; create-vs-update decided.

1. Require `WIKI_URL` and `WIKI_API_KEY`. If either is missing → **notify the user** and stop. Do not invent a host. Do not echo the key.
2. `pages.list(limit: 1)` (or equivalent) — fail closed on auth errors; notify the user.
3. For each target: `pages.singleByPath(path, locale)` → **update** if `id`, else **create**.
4. For **create**, ensure parent path exists. If missing, add parent creates to Plan B (parents first) or stop and ask.
5. Prefer `curl` for HTTPS if Python SSL verify fails.

---

## Step 2 — Plan B (wiki mutate — stop here)

Completion: user sees the plan table; **no GraphQL mutations yet**.

| path | id or — | action | working copy | title |
|---|---|---|---|---|
| … | … | create \| update | `$TMPDIR/…` | … |

Live URL form: `$WIKI_URL/<locale>/<path>`.

**Stop.** Wait for exact `APPROVE`.  
`yes` / `ok` / `go` are not enough.

---

## Step 3 — Mutate (only after APPROVE)

Completion: every planned row has success **and** `singleByPath` verify.

1. **create** — `pages.create` with `editor: "markdown"`, `isPublished: true`, `isPrivate: false`, `tags: []`, body from working copy.  
   Do **not** request `page.locale` in the create selection set. Prefer `responseResult` + verify query.
2. **update** — `pages.update(id: …)` with new `content` / `title`; keep `editor: "markdown"`, `isPublished: true`.
3. When *index propagation* applies: mutate leaf/section first, then section index, then project root ([REFERENCE.md](REFERENCE.md#mutate-order)).
4. After each write: `singleByPath` → confirm `id`, `path`, `title`.
5. Report live URLs + failures.

---

## Step 4 — Cleanup

Completion: every **successfully** verified working copy from this run is deleted; failures remain with path printed.

1. On success (mutate + verify): delete that working copy.
2. On failure or abandon: keep the file; print its path for retry.

---

## Guards (positive form)

- Draft in the working copy; wiki publish *mutate* is a separate gated step.
- Create when missing; update when `id` exists.
- Exact `APPROVE` gates Plan A (local write) and Plan B (wiki mutate).
- Ephemeral working copies only — wiki is durable.
- Project leaf/section publishes expand to *index propagation* targets unless the user opts out.
- Index propagation: pull **index paths first**, exact paths only — no folder listings, no unrelated pages.
