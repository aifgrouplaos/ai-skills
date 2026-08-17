# Wiki write — reference

## Auth / read

Same as wiki-publish: `POST $WIKI_URL/graphql` with `Authorization: Bearer $WIKI_API_KEY`.

```graphql
query ($path: String!, $locale: String!) {
  pages {
    singleByPath(path: $path, locale: $locale) {
      id
      path
      title
      content
    }
  }
}
```

Typical locale: `"en"`.

## Project name

Source of truth is the wiki, not git.

1. Pull **`projects`** (index) via `singleByPath` first — one cheap page. Parse existing slugs from locale-absolute `/en/projects/<slug>` links on that page.
2. Never derive a slug from git repo name, cwd, or local folders.
3. User already named a slug **on that index** → use it; no ask.
4. Index has projects, none named → offer the wiki list (one pick).
5. Slug **not** on the index → ask for a new wiki slug (creating a new project tree).

## Template menu source

Index path: `standards/templates`  
Pull this page only for the *menu*. Do not bulk-load sibling `standards/*` pages.

## Template → destination path

| Template (GraphQL path) | Destination under `projects/<project-name>/` |
|---|---|
| `standards/templates/project` | `projects/<project-name>` (folder root) |
| `standards/templates/overview` | `projects/<project-name>/overview` |
| `standards/templates/backend` | `projects/<project-name>/backend` |
| `standards/templates/frontend` | `projects/<project-name>/frontend` |
| `standards/templates/adr` | `…/backend/adr` or `…/frontend/adr` (ask which side) |
| `standards/templates/glossary` | `…/backend/glossary` or `…/frontend/glossary` (ask which side) |
| `standards/templates/workflows` | `projects/<project-name>/workflows` |
| `standards/templates/workflow` | `projects/<project-name>/workflows/<journey>` |
| `standards/templates/integrations` | `projects/<project-name>/integrations` |
| `standards/templates/integration` | `projects/<project-name>/integrations/<system>` |
| `standards/templates/runbooks` | `projects/<project-name>/runbooks` |
| `standards/templates/runbook` | `projects/<project-name>/runbooks/<runbook>` |
| `standards/templates/api` | `projects/<project-name>/api` |

Leaf slugs (`<journey>`, `<system>`, `<runbook>`) come from the user in Step 3.

## Index propagation

When authoring a **leaf** or **section** page under a project, also refresh upstream index working copies so navigation stays current.

### Leaf → indexes

| Leaf template | Leaf path | Section index (patch) | Project root |
|---|---|---|---|
| `workflow` | `projects/<p>/workflows/<slug>` | `projects/<p>/workflows` | `projects/<p>` |
| `integration` | `projects/<p>/integrations/<slug>` | `projects/<p>/integrations` | `projects/<p>` |
| `runbook` | `projects/<p>/runbooks/<slug>` | `projects/<p>/runbooks` | `projects/<p>` |

### Section → project root

When authoring a **section** page (not a leaf), ensure **project root** lists it:

| Section template | Section path |
|---|---|
| `overview` | `projects/<p>/overview` |
| `backend` | `projects/<p>/backend` |
| `frontend` | `projects/<p>/frontend` |
| `workflows` | `projects/<p>/workflows` |
| `integrations` | `projects/<p>/integrations` |
| `runbooks` | `projects/<p>/runbooks` |
| `api` | `projects/<p>/api` |

All section rows patch `projects/<p>` (project root).

### Pull discipline

- **Index first** — upstream index page(s) are cheap; pull them via `singleByPath` before heavier reads or patching.
- **Exact paths only** — one GraphQL read per path in the tables below; never `pages.list` by prefix, never bulk-fetch `projects/<p>/*`, never pull sibling leaves or unrelated sections.
- **Leaf propagation order:** (1) section index, (2) project root — indexes only (the leaf body is already in the working copy).
- **Section propagation:** project root only.

### Patch rules

1. **Pull live** index content via GraphQL before editing.
2. Add a list entry if the link is missing: `- [<title>](/en/<path>)` — match the list style already on the page; add a one-line blurb only if sibling entries have one.
3. Link href = locale-absolute `/en/` + GraphQL path (no `./` or `../`).
4. Link text = the new page's `#` title (or user override).
5. **Do not duplicate** an existing link to the same path; update link text if the title changed.
6. Preserve existing content and ordering; append unless the live index uses another convention (e.g. alphabetical) — then follow it.
7. One working copy per path under `$TMPDIR/wiki-publish/`.

## Working copy filename

`$TMPDIR/wiki-publish/<path-with-/-as-->.md`  
Example: `projects/lao-post/workflows/rider-delivery` → `projects-lao-post-workflows-rider-delivery.md`

## Fill rules

- Mermaid: Wiki.js renders **8.8.2**. Use diagram syntax supported by that release only (no newer-only features).
- Replace every `<placeholder>`.
- Delete blockquoted template guidance from the published body.
- Keep Status lines (`active` / `todo` / `n/a` + Owner/Since/Reason as the template requires).
- In-body links: locale-absolute `/en/…` only (no `./` or `../`).
