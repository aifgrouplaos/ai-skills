# Wiki.js GraphQL — page mutations

## Auth

```http
POST {WIKI_URL}/graphql
Authorization: Bearer <WIKI_API_KEY>
Content-Type: application/json
```

Body: `{ "query": "...", "variables": { ... } }`

Both `WIKI_URL` and `WIKI_API_KEY` are required (no hard-coded host default).

## Read

```graphql
query ($path: String!, $locale: String!) {
  pages {
    singleByPath(path: $path, locale: $locale) {
      id
      path
      title
      content
      updatedAt
    }
  }
}
```

## Create

Required args: `content`, `description`, `editor`, `isPublished`, `isPrivate`, `locale`, `path`, `title`, `tags`.

```graphql
mutation (
  $content: String!
  $description: String!
  $editor: String!
  $isPublished: Boolean!
  $isPrivate: Boolean!
  $locale: String!
  $path: String!
  $title: String!
  $tags: [String]!
) {
  pages {
    create(
      content: $content
      description: $description
      editor: $editor
      isPublished: $isPublished
      isPrivate: $isPrivate
      locale: $locale
      path: $path
      tags: $tags
      title: $title
    ) {
      responseResult { succeeded message errorCode }
      page { id path title }
    }
  }
}
```

Typical variables: `editor: "markdown"`, `isPublished: true`, `isPrivate: false`, `description: ""`, `tags: []`.

## Update

```graphql
mutation (
  $id: Int!
  $content: String!
  $title: String!
  $editor: String!
  $isPublished: Boolean!
) {
  pages {
    update(
      id: $id
      content: $content
      title: $title
      editor: $editor
      isPublished: $isPublished
    ) {
      responseResult { succeeded message errorCode }
      page { id path title updatedAt }
    }
  }
}
```

## Path map (wiki path → working copy)

| Wiki path | Working copy under `$TMPDIR/wiki-publish/` |
|---|---|
| `standards/templates/overview` | `standards-templates-overview.md` |
| `projects/lao-post` | `projects-lao-post.md` |
| `projects/lao-post` (locale `th`) | `projects-lao-post.th.md` |
| leaf `README` on wiki | bare path → same slug file |

Locale stays in `locale`, never in `path`. For `locale: "th"`, resolve the `.th.md` working copy; for `en`, the plain `.md` file. Lao **document language** is published at locale `th` (ADR-0003).

When patching indexes for `locale: "th"`, link hrefs use `/th/…` (mirror wiki-write `/en/…` rules).

## Template pull

When Step 0 mode is **pull template**, fetch live wiki content from the matching `standards/templates/…` page (e.g. overview → `standards/templates/overview`). If that page is missing, use **stub**.

## Index propagation

When publishing a **leaf** or **section** under `projects/<p>/…`, also publish upstream index updates so navigation stays current. Same mapping and patch rules as wiki-write [REFERENCE.md](../wiki-write/REFERENCE.md#index-propagation).

### Auto-expand targets

| Published path pattern | Also include in plan |
|---|---|
| `projects/<p>/workflows/<slug>` | `projects/<p>/workflows`, `projects/<p>` |
| `projects/<p>/integrations/<slug>` | `projects/<p>/integrations`, `projects/<p>` |
| `projects/<p>/runbooks/<slug>` | `projects/<p>/runbooks`, `projects/<p>` |
| `projects/<p>/{overview,backend,frontend,workflows,integrations,runbooks,api}` | `projects/<p>` |

For each expanded index path: use its working copy if present under `$TMPDIR/wiki-publish/`; else pull live via `singleByPath` and patch per wiki-write rules before Plan B.

**Pull discipline:** index page(s) first (section index, then project root for leaves); exact propagation-table paths only — never `pages.list` by prefix, never bulk-pull the project tree or sibling leaves.

Indexes are almost always **update** (not create). If an index page is missing, stop and ask — run `/wiki-write` for the section template first.

### Mutate order

1. Leaf or section page (create or update).
2. Section index (when leaf).
3. Project root.

## Gotchas

- Create may succeed while GraphQL errors on selecting nullable `page.locale` — trust `responseResult` + `singleByPath` verify.
- Parent paths must exist before child creates.
- API Access must be enabled; new keys sometimes need a Wiki.js restart.
- Prefer `curl` if Python HTTPS SSL verify fails on the machine.
- In-body markdown links must be locale-absolute (`/en/…` or `/th/…` matching the page locale). `./standards/…` or `../…` nests under the current page (e.g. `/en/standards/standards/…`).
