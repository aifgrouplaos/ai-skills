# ADR-0001: Wiki host is env-only

## Context

Wiki publish talks to a company Wiki.js instance. An earlier draft hard-coded `https://wiki.aifgrouplaos.com` as the default with `WIKI_URL` as override.

## Decision

Require `WIKI_URL` (and `WIKI_API_KEY`) from the environment. Do not ship a hard-coded host default in the Skill.

## Consequences

- Colleagues must set both env vars (documented in the repo README) before `/wiki-publish` works.
- Same Skill works against staging/other Wiki.js hosts without editing the Skill.
- Misconfiguration fails closed at preflight instead of silently hitting the wrong host.
