# Glossary

## Skill

An installable agent-skill folder under `skills/<bucket>/<name>/` (`company`, `engineering`, or `productivity`), containing at least `SKILL.md`. Colleagues install Skills from this repo with `npx skills`.

## Skills catalog

This private GitHub repository (`aifgrouplaos/ai-skills`). It holds Skills for internal company use — not wiki page content.

## Wiki page (durable)

A published page on the company Wiki.js host. The wiki is the durable store for Engineering Wiki content.

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

The Skill that authors one Engineering Wiki page per run from a live `standards/templates/…` template. It never mutates Wiki.js. Flow: confirm **project name** → offer template menu from the live templates index (not a full `standards/` dump) → user picks one template → pull that template only → grill from its placeholders → fill codebase-second when code exists → write one working copy under `projects/<project-name>/…` → hand off to `/wiki-publish`.

## Project name (wiki)

The slug the user confirms for the wiki tree. Maps to path prefix `projects/<project-name>/` (URL form `$WIKI_URL/<locale>/projects/<project-name>/`). Not necessarily identical to the git repo name — confirm with the user.

