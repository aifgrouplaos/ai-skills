# Wiki write Lao — reference

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

Source locale: `"en"`. Target locale: `"lo"`.

**Session language:** agent messages to the user stay English. Only *working copy* file bodies are Lao.

## Path parsing

| User input | GraphQL path |
|---|---|
| `/en/projects/lao-post/workflows/rider-delivery` | `projects/lao-post/workflows/rider-delivery` |
| `/lo/projects/lao-post/overview` | `projects/lao-post/overview` |
| `projects/lao-post/backend` | `projects/lao-post/backend` |
| `$WIKI_URL/en/projects/lao-post` | `projects/lao-post` |

Strip host and locale prefix; **path never contains `en/` or `lo/`**.

## English source resolution

| Priority | Source |
|---|---|
| 1 | `$TMPDIR/wiki-publish/<path-with-/-as-->.md` |
| 2 | Live wiki: `singleByPath(path, locale: "en")` |

If both are missing, stop — run `/wiki-write` for the English page first.

## Working copy filename

| GraphQL path | Lao working copy |
|---|---|
| `projects/lao-post/overview` | `$TMPDIR/wiki-publish/projects-lao-post-overview.lo.md` |
| `projects/lao-post/workflows/rider-delivery` | `$TMPDIR/wiki-publish/projects-lao-post-workflows-rider-delivery.lo.md` |

English and Lao working copies for the same path differ by the `.lo` suffix before `.md`.

## Translation

| Translate | Keep in English |
|---|---|
| `#` title, headings, prose, list text | Fenced code blocks and inline `` `code` `` |
| Table cells (non-code) | GraphQL paths, URL path segments, slugs |
| Mermaid node labels (8.8.2 syntax only) | API / env / CLI identifiers |
| Markdown link text | Status field keys (`Owner`, `Since`, `Reason`) |
| | Product names when the English page keeps them |

**Links:** every in-body href `/en/<path>` → `/lo/<path>`. No `./` or `../`.

**Ambiguous domain term:** ask the user. Optional glossary (Step 3) is the only extra wiki read for terminology — no web search.

## Index propagation

Same path topology as [wiki-write REFERENCE](../wiki-write/REFERENCE.md#index-propagation). Lao differs only by locale, link prefix, and working-copy suffix.

### Leaf → indexes

| Leaf path pattern | Section index (patch) | Project root |
|---|---|---|
| `projects/<p>/workflows/<slug>` | `projects/<p>/workflows` | `projects/<p>` |
| `projects/<p>/integrations/<slug>` | `projects/<p>/integrations` | `projects/<p>` |
| `projects/<p>/runbooks/<slug>` | `projects/<p>/runbooks` | `projects/<p>` |

### Section → project root

| Section path |
|---|
| `projects/<p>/overview` |
| `projects/<p>/backend` |
| `projects/<p>/frontend` |
| `projects/<p>/workflows` |
| `projects/<p>/integrations` |
| `projects/<p>/runbooks` |
| `projects/<p>/api` |

Project root (`projects/<p>`) has no upstream index to patch.

### Pull discipline

- Pull Lao indexes with `locale: "lo"` via `singleByPath` — **index first**, exact propagation-table paths only.
- Never `pages.list` by prefix; never bulk-fetch `projects/<p>/*`.
- **Leaf order:** (1) section index, (2) project root.
- **Section:** project root only.

### Patch rules

1. Pull live Lao index content before editing.
2. Add a list entry if missing: `- [<translated title>](/lo/<path>)` — match list style on the page.
3. Link href = locale-absolute `/lo/` + GraphQL path.
4. Link text = the new page's translated `#` title.
5. Do not duplicate an existing link to the same path; update link text if the title changed.
6. Preserve existing content and ordering.
7. One `.lo.md` working copy per index path under `$TMPDIR/wiki-publish/`.

### Missing Lao parent

If a Lao index page does not exist yet (`singleByPath` returns no `id` for `locale: "lo"`), stop and ask — create the Lao project tree (English `/wiki-write` + `/wiki-publish` for structure, or bootstrap Lao indexes) before propagating.

## Optional glossary paths

| Path | When |
|---|---|
| `projects/<p>/backend/glossary` | User asked for term consistency; backend project |
| `projects/<p>/frontend/glossary` | User asked for term consistency; frontend project |

Pull English (`locale: "en"`) only. Use as terminology hints during *translate*; do not copy the glossary body into the target page.
