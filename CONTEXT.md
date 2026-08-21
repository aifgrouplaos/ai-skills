# Glossary

## Skill

An installable agent-skill folder under `skills/<bucket>/<name>/` (`company`, `engineering`, or `productivity`), containing at least `SKILL.md`. Colleagues install Skills from this repo with `npx skills`.

## Skills catalog

This private GitHub repository (`aifgrouplaos/ai-skills`). It holds Skills for internal company use — not wiki page content.

## Wiki page (durable)

A published page on the company Wiki.js host, identified by **(path, locale)** — same GraphQL path can exist in multiple locales. The wiki is the durable store for Engineering Wiki content.

## Locale (wiki)

The Wiki.js language-pack code for a Wiki page (`en`, `th`, …). Locale appears in the live URL (`$WIKI_URL/<locale>/<path>`) and in GraphQL as a separate field — never inside `path`. Project slugs stay English across locales. This host has no `lo` pack; Lao **document language** uses locale `th` (ADR-0003).

_Avoid_: treating locale as the language of the markdown body.

## Document language

The language of a Wiki page's markdown body (English or Lao). Distinct from **Locale**. English pages use locale `en`; Lao pages use locale `th`.

## Locale pair

Two Wiki pages that share the same GraphQL path but differ by locale (e.g. `/en/projects/<p>/overview` English body and `/th/projects/<p>/overview` Lao body). English is canonical; Lao is derived from the English page.

## Working copy (ephemeral)

A temporary local markdown file used only for drafting and publishing via Wiki publish. It lives outside project repos (e.g. under `$TMPDIR`), never under `docs/wiki-packs/` in a service repo. After a successful wiki mutate, the Skill deletes it.

Avoid: committing wiki content into project repos; calling a Skill a "pack".

## Wiki publish

The Skill that creates or updates live Wiki.js pages from a Working copy via GraphQL. Flow: optional Step 0 → Plan A (local write) → `APPROVE` → write working copy → stop for human edit → Plan B (wiki mutate) → `APPROVE` → mutate → delete working copy on success.

## Wiki host

The Wiki.js base URL used by Wiki publish / Wiki write / Wiki write Lao. Taken from workspace `.env` keys `WIKI_URL` and `WIKI_API_KEY` only — never a hard-coded default, never the rest of `.env`. Missing file or either value fails closed: the agent notifies the user and stops — it does not invent a host or echo the key.

## Step 0 (bootstrap)

The Wiki publish branch when no working copy exists yet. Modes: **stub**, **pull template** (from live wiki `standards/templates/…`, else stub), **pull live** (target path via GraphQL). Always followed by Plan A + `APPROVE` before writing the working copy.

## Wiki write

The Skill that authors one **English** Engineering Wiki page per run from a live `standards/templates/…` template. It never mutates Wiki.js. After selecting one template, it routes evidence by page type: Shared templates establish cross-role meaning before code corroboration; backend and frontend inspect code before grilling gaps; ADR establishes the decision before verifying implementation; glossary inspects existing language before resolving ambiguity. Conflicting sources require human resolution. The Skill then writes ephemeral working copies for the selected page and applicable index propagation, and hands off to `/wiki-publish`.

_Avoid_: emitting Lao in the same run; re-grilling for Lao (use **Wiki write Lao**).

## Wiki write Lao

The Skill that authors one **Lao** Wiki page per run by translating an existing English page at the same GraphQL path (`/en/<path>` → `/th/<path>`). Not limited to `projects/`. It never mutates Wiki.js. Flow: user supplies one English target path → read English from a working copy if present, else the live English Wiki page → translate to Lao → write one `.th.md` working copy → hand off to `/wiki-publish` with `/th/…`. No template menu, no grill. Agent chat stays English; Lao is only in the working copy. Locale is `th` because Wiki.js has no `lo` pack.

_Avoid_: "version" when you mean locale; a second GraphQL path for Lao; writing Thai prose; cloning the Wiki write interview flow; replying to the user in Lao during the skill run.

## Project name (wiki)

The slug under `projects/<project-name>/` (URL form `$WIKI_URL/<locale>/projects/<project-name>/`). Taken from the wiki `projects` index only — never from the git repo or cwd. Ask the user only when creating a project that is not yet on that index.

