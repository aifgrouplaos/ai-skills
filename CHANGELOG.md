# Changelog

All notable changes to this project are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.2.0] - 2026-08-18

### Added

- **`wiki-write-lao`** company skill — translate one English wiki page into a Lao locale pair; write `.lo.md` working copies; hand off to `/wiki-publish`.

### Changed

- **`wiki-publish`** resolves `locale: "lo"` from `/lo/…` URLs and `.lo.md` working copies.
- **`wiki-write`** handoff mentions `/wiki-write-lao` for Lao locale pairs.
- **`CONTEXT.md`** glossary — Locale, Locale pair, Wiki write Lao; Wiki write is English-only.

## [1.1.0] - 2026-08-17

### Added

- **Index propagation** in `wiki-write` and `wiki-publish` — adding a leaf page (workflow, integration, runbook) or section page also patches the section index and project root with links to the new page.
- **`evidence-research`** engineering skill.

### Changed

- **`wiki-write`** resolves project slugs from the live wiki `projects` index only (never git repo or cwd); skips the project-name ask when the slug already exists on that index.
- **`wiki-write`** documents Mermaid **8.8.2** syntax limits (Wiki.js renderer version).
- Index propagation pulls upstream index page(s) first via exact `singleByPath` paths — no folder listings or unrelated bulk pulls.

## [1.0.0] - 2026-08-07

### Added

- Initial AIF Group Laos skills catalog installable via `npx skills` and Claude Code plugin (`aif-skills`).
- **Company skills:** `wiki-publish`, `wiki-write`.
- **Engineering skills:** ask-matt, code-review, codebase-design, debug-mantra, diagnosing-bugs, domain-modeling, grill-with-docs, implement, improve-codebase-architecture, post-mortem, prototype, qwen-agent, research, resolving-merge-conflicts, scrutinize, setup-matt-pocock-skills, tdd, to-spec, to-tickets, triage, wayfinder, wizard.
- **Productivity skills:** caveman, grill-me, grilling, handoff, management-talk, qwenchance, teach, to-questionnaire, wait-what, writing-for-agents.
- Repository docs: `CONTEXT.md`, `AGENTS.md`, ADRs for env-only wiki host and ephemeral working copies.
- Claude plugin marketplace config (`.claude-plugin/marketplace.json`, `.claude-plugin/plugin.json`).

[1.2.0]: https://github.com/aifgrouplaos/ai-skills/releases/tag/v1.2.0
[1.1.0]: https://github.com/aifgrouplaos/ai-skills/releases/tag/v1.1.0
[1.0.0]: https://github.com/aifgrouplaos/ai-skills/releases/tag/v1.0.0
