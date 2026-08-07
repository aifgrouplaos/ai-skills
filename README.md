# ai-skills

Private company catalog of agent Skills for [AIF Group Laos](https://github.com/aifgrouplaos). Colleagues install with [`npx skills`](https://skills.sh).

## Install

Requires GitHub access to this private repo (`gh auth login` and/or SSH keys that can clone it).

```bash
# All skills in this catalog
npx skills@latest add aifgrouplaos/ai-skills

# One skill
npx skills@latest add aifgrouplaos/ai-skills@wiki-publish

# SSH if HTTPS auth is awkward
npx skills@latest add git@github.com:aifgrouplaos/ai-skills.git
```

Then invoke in the agent as `/wiki-publish` (user-invoked).

## Environment (wiki-publish)

Both required — the Skill fails closed and notifies you if either is missing:

| Variable | Purpose |
|---|---|
| `WIKI_URL` | Wiki.js base URL (no trailing path) |
| `WIKI_API_KEY` | Bearer token for GraphQL — **never commit** |

Example (local only, e.g. shell profile or a gitignored `.env`):

```bash
export WIKI_URL=https://wiki.aifgrouplaos.com
export WIKI_API_KEY=…   # from Wiki.js API access — do not paste into chat/PRs
```

## Skills

| Skill | What it does |
|---|---|
| [`wiki-publish`](./skills/wiki-publish/) | Bootstrap an ephemeral working copy, then create/update Wiki.js pages via GraphQL after exact `APPROVE` |

## Layout

```
skills/
  wiki-publish/
    SKILL.md
    REFERENCE.md
```

Domain glossary: [`CONTEXT.md`](./CONTEXT.md). Agent config: [`AGENTS.md`](./AGENTS.md).
