# ADR-0002: Ephemeral working copies; wiki is durable

## Context

An earlier docs-home pattern kept paste sources under `docs/wiki-packs/` inside project repos. For company-wide `/wiki-publish`, that couples service repos to wiki IA and leaves stale local copies.

## Decision

- Do **not** store wiki markdown under `docs/wiki-packs/` (or similar) inside project repos.
- Wiki publish uses an **ephemeral working copy** (temp path outside the project tree).
- **Wiki.js is the durable store.**
- After a **successful** wiki mutate, delete the working copy.
- Templates and missing pages are pulled from the live wiki when possible; otherwise stub.

## Consequences

- Colleagues need Wiki.js credentials to author, not a pack checkout in every service repo.
- No git history of wiki drafts in service repos (history lives in Wiki.js).
- Failed mutates should keep the working copy so the user can retry without re-bootstrap.
