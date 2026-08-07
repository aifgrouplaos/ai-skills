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
| leaf `README` on wiki | bare path → same slug file |

Locale stays in `locale`, never in `path`.

## Template pull

When Step 0 mode is **pull template**, fetch live wiki content from the matching `standards/templates/…` page (e.g. overview → `standards/templates/overview`). If that page is missing, use **stub**.

## Gotchas

- Create may succeed while GraphQL errors on selecting nullable `page.locale` — trust `responseResult` + `singleByPath` verify.
- Parent paths must exist before child creates.
- API Access must be enabled; new keys sometimes need a Wiki.js restart.
- Prefer `curl` if Python HTTPS SSL verify fails on the machine.
- In-body markdown links must be locale-absolute (`/en/standards/…`, `/en/projects/…`). `./standards/…` or `../…` nests under the current page (e.g. `/en/standards/standards/…`).
