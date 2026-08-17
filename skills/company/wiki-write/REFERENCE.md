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

## Working copy filename

`$TMPDIR/wiki-publish/<path-with-/-as-->.md`  
Example: `projects/lao-post/workflows/rider-delivery` → `projects-lao-post-workflows-rider-delivery.md`

## Fill rules

- Mermaid: Wiki.js renders **8.8.2**. Use diagram syntax supported by that release only (no newer-only features).
- Replace every `<placeholder>`.
- Delete blockquoted template guidance from the published body.
- Keep Status lines (`active` / `todo` / `n/a` + Owner/Since/Reason as the template requires).
- In-body links: locale-absolute `/en/…` only (no `./` or `../`).
