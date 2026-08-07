# ai-skills

Private company catalog of agent Skills for [AIF Group Laos](https://github.com/aifgrouplaos). Colleagues install with [`npx skills`](https://skills.sh).

## Install

Requires GitHub access to this private repo (`gh auth login` and/or SSH keys that can clone it).

All skills in this catalog (interactive picker):

```bash
npx skills@latest add aifgrouplaos/ai-skills
```

One skill:

```bash
npx skills@latest add aifgrouplaos/ai-skills@wiki-publish
```

SSH if HTTPS auth is awkward:

```bash
npx skills@latest add git@github.com:aifgrouplaos/ai-skills.git
```

Then invoke in the agent as `/skill-name` (e.g. `/wiki-publish`, `/caveman`, `/tdd`).

## Environment (wiki-write / wiki-publish)

Both required — these Skills fail closed and notify you if either is missing:

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

Grouped under `skills/<bucket>/<name>/`. Upstream pins and licenses: [`third-party/`](./third-party/). Matt Pocock skills documentation: [aihero.dev/skills](https://www.aihero.dev/skills).

### `company/` — AIF-authored

| Skill | What it does |
|---|---|
| [`wiki-write`](./skills/company/wiki-write/) | Author one page from a live standards template (grill placeholders) → working copy; never publishes |
| [`wiki-publish`](./skills/company/wiki-publish/) | Bootstrap/update an ephemeral working copy, then create/update Wiki.js pages via GraphQL after exact `APPROVE` |

### `engineering/` — daily code work

| Skill | Source | What it does |
|---|---|---|
| [`ask-matt`](./skills/engineering/ask-matt/) | mattpocock | Router — which skill/flow fits |
| [`code-review`](./skills/engineering/code-review/) | mattpocock | Standards + Spec review via parallel sub-agents |
| [`codebase-design`](./skills/engineering/codebase-design/) | mattpocock | Deep-module design vocabulary |
| [`debug-mantra`](./skills/engineering/debug-mantra/) | 9arm | Four-mantra debugging discipline |
| [`diagnosing-bugs`](./skills/engineering/diagnosing-bugs/) | mattpocock | Disciplined diagnosis loop |
| [`domain-modeling`](./skills/engineering/domain-modeling/) | mattpocock | Glossary + ADRs |
| [`grill-with-docs`](./skills/engineering/grill-with-docs/) | mattpocock | Grilling + domain docs inline |
| [`implement`](./skills/engineering/implement/) | mattpocock | Build from a spec/tickets (`/tdd` + `/code-review`) |
| [`improve-codebase-architecture`](./skills/engineering/improve-codebase-architecture/) | mattpocock | Deepening survey + HTML report |
| [`post-mortem`](./skills/engineering/post-mortem/) | 9arm | Canonical bug writeup after a validated fix |
| [`prototype`](./skills/engineering/prototype/) | mattpocock | Throwaway prototype for a design question |
| [`qwen-agent`](./skills/engineering/qwen-agent/) | 9arm | Cheap Qwen subagent via `claude-9arm` (needs that CLI) |
| [`research`](./skills/engineering/research/) | mattpocock | Primary-source research → Markdown in-repo |
| [`resolving-merge-conflicts`](./skills/engineering/resolving-merge-conflicts/) | mattpocock | Resolve in-progress merge/rebase by intent |
| [`scrutinize`](./skills/engineering/scrutinize/) | 9arm | Outsider end-to-end review of plan/PR/change |
| [`setup-matt-pocock-skills`](./skills/engineering/setup-matt-pocock-skills/) | mattpocock | One-time issue tracker / triage / domain layout setup |
| [`tdd`](./skills/engineering/tdd/) | mattpocock | Red-green-refactor |
| [`to-spec`](./skills/engineering/to-spec/) | mattpocock | Conversation → issue-tracker spec |
| [`to-tickets`](./skills/engineering/to-tickets/) | mattpocock | Plan → tracer-bullet tickets with edges |
| [`triage`](./skills/engineering/triage/) | mattpocock | Issue/PR triage state machine |
| [`wayfinder`](./skills/engineering/wayfinder/) | mattpocock | Large work as a map of decision tickets |
| [`wizard`](./skills/engineering/wizard/) | mattpocock | Interactive bash wizard for human-only steps |

### `productivity/` — non-code workflow

| Skill | Source | What it does |
|---|---|---|
| [`caveman`](./skills/productivity/caveman/) | caveman | Terse communication mode (`/caveman`); say `stop caveman` to exit |
| [`grill-me`](./skills/productivity/grill-me/) | mattpocock | Relentless plan/design interview |
| [`grilling`](./skills/productivity/grilling/) | mattpocock | Shared grilling primitive |
| [`handoff`](./skills/productivity/handoff/) | mattpocock | Compact conversation for another agent |
| [`management-talk`](./skills/productivity/management-talk/) | 9arm | Engineer prose → leadership channel copy |
| [`qwenchance`](./skills/productivity/qwenchance/) | 9arm | Break loops / bound context / handoff before window fills |
| [`teach`](./skills/productivity/teach/) | mattpocock | Multi-session teaching workspace |
| [`to-questionnaire`](./skills/productivity/to-questionnaire/) | mattpocock | Decision → async questionnaire |
| [`wait-what`](./skills/productivity/wait-what/) | mattpocock | Re-pitch a message that didn't land |
| [`writing-for-agents`](./skills/productivity/writing-for-agents/) | mattpocock | How to write skills / AGENTS.md |

## Layout

```
skills/
  company/       # AIF-authored
  engineering/   # daily code work
  productivity/  # non-code workflow
third-party/     # upstream licenses + pin SHAs
docs/agents/     # issue tracker, triage labels, domain docs pointers
CONTEXT.md
AGENTS.md
```

Domain glossary: [`CONTEXT.md`](./CONTEXT.md). Agent config: [`AGENTS.md`](./AGENTS.md).
