# Glossary

## Skill

An installable agent-skill folder under `skills/<bucket>/<name>/` (`company`, `engineering`, or `productivity`), containing at least `SKILL.md`. Colleagues install Skills from this repo with `npx skills`.

## Skills catalog

This private GitHub repository (`aifgrouplaos/ai-skills`). It holds Skills for internal company use — not wiki page content.

## Wiki page (durable)

A published page on the company Wiki.js host, identified by **(path, locale)** — same GraphQL path can exist in multiple locales (e.g. English and Lao). The wiki is the durable store for Engineering Wiki content.

## Locale (wiki)

The Wiki.js language code for a Wiki page (`en`, `lo`, …). Locale appears in the live URL (`$WIKI_URL/<locale>/<path>`) and in GraphQL as a separate field — never inside `path`. Project slugs stay English across locales.

## Locale pair

Two Wiki pages that share the same GraphQL path but differ by locale (e.g. `/en/projects/<p>/overview` and `/lo/projects/<p>/overview`). English is canonical; Lao is derived from the English page.

## Working copy (ephemeral)

A temporary local markdown file used only for drafting and publishing via Wiki publish. It lives outside project repos (e.g. under `$TMPDIR`), never under `docs/wiki-packs/` in a service repo. After a successful wiki mutate, the Skill deletes it.

Avoid: committing wiki content into project repos; calling a Skill a "pack".

## Wiki publish

The Skill that creates or updates live Wiki.js pages from a Working copy via GraphQL. Flow: optional Step 0 → Plan A (local write) → `APPROVE` → write working copy → stop for human edit → Plan B (wiki mutate) → `APPROVE` → mutate → delete working copy on success.

## Wiki host

The Wiki.js base URL used by Wiki publish. Supplied only via environment (`WIKI_URL`) — never a hard-coded default in the Skill. Missing `WIKI_URL` or `WIKI_API_KEY` fails closed: the agent notifies the user and stops — it does not invent a host or echo the key.

## Step 0 (bootstrap)

The Wiki publish branch when no working copy exists yet. Modes: **stub**, **pull template** (from live wiki `standards/templates/…`, else stub), **pull live** (target path via GraphQL). Always followed by Plan A + `APPROVE` before writing the working copy.

## Wiki write

The Skill that authors one **English** Engineering Wiki page per run from a live `standards/templates/…` template. It never mutates Wiki.js. Flow: resolve **project name** from the live wiki `projects` index (skip the ask when the slug already exists; never use git repo name) → offer template menu from the live templates index (not a full `standards/` dump) → user picks one template → pull that template only → grill from its placeholders → fill codebase-second when code exists → write one English working copy under `projects/<project-name>/…` → hand off to `/wiki-publish`.

_Avoid_: emitting Lao in the same run; re-grilling for Lao (use **Wiki write Lao**).

## Wiki write Lao

The Skill that authors one **Lao** Wiki page per run by translating an existing English page at the same GraphQL path. It never mutates Wiki.js. Flow: user supplies one English target path → read English from a working copy if present, else the live English Wiki page → translate to Lao → write one Lao working copy → hand off to `/wiki-publish`. No template menu, no grill.

_Avoid_: "version" when you mean locale; a second GraphQL path for Lao; cloning the Wiki write interview flow.

## Project name (wiki)

The slug under `projects/<project-name>/` (URL form `$WIKI_URL/<locale>/projects/<project-name>/`). Taken from the wiki `projects` index only — never from the git repo or cwd. Ask the user only when creating a project that is not yet on that index.

