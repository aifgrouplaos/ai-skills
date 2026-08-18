# ADR-0001: Wiki host is env-only

## Context

Wiki publish talks to a company Wiki.js instance. An earlier draft hard-coded `https://wiki.aifgrouplaos.com` as the default with `WIKI_URL` as override.

## Decision

Require `WIKI_URL` (and `WIKI_API_KEY`) from the workspace `.env`, reading **those two keys only**. Do not ship a hard-coded host default in the Skill. Do not `source` the whole file.

## Consequences

- Colleagues must put both keys in a gitignored workspace `.env` before the wiki Skills work.
- Same Skill works against staging/other Wiki.js hosts without editing the Skill.
- Other secrets in `.env` stay unread.
- Misconfiguration fails closed at preflight instead of silently hitting the wrong host.
