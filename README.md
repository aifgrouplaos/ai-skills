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

Then invoke in the agent as `/skill-name` (e.g. `/wiki-publish`, `/caveman`, `/tdd`).

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

Flat layout under `skills/<name>/`. Upstream pins and licenses: [`third-party/`](./third-party/).

### Company

| Skill | What it does |
|---|---|
| [`wiki-publish`](./skills/wiki-publish/) | Bootstrap an ephemeral working copy, then create/update Wiki.js pages via GraphQL after exact `APPROVE` |

### From [mattpocock/skills](https://github.com/mattpocock/skills) (public engineering + productivity)

| Skill | What it does |
|---|---|
| [`ask-matt`](./skills/ask-matt/) | Router — which skill/flow fits |
| [`code-review`](./skills/code-review/) | Standards + Spec review via parallel sub-agents |
| [`codebase-design`](./skills/codebase-design/) | Deep-module design vocabulary |
| [`diagnosing-bugs`](./skills/diagnosing-bugs/) | Disciplined diagnosis loop |
| [`domain-modeling`](./skills/domain-modeling/) | Glossary + ADRs |
| [`grill-me`](./skills/grill-me/) | Relentless plan/design interview |
| [`grill-with-docs`](./skills/grill-with-docs/) | Grilling + domain docs inline |
| [`grilling`](./skills/grilling/) | Shared grilling primitive |
| [`handoff`](./skills/handoff/) | Compact conversation for another agent |
| [`implement`](./skills/implement/) | Build from a spec/tickets (`/tdd` + `/code-review`) |
| [`improve-codebase-architecture`](./skills/improve-codebase-architecture/) | Deepening survey + HTML report |
| [`prototype`](./skills/prototype/) | Throwaway prototype for a design question |
| [`research`](./skills/research/) | Primary-source research → Markdown in-repo |
| [`resolving-merge-conflicts`](./skills/resolving-merge-conflicts/) | Resolve in-progress merge/rebase by intent |
| [`setup-matt-pocock-skills`](./skills/setup-matt-pocock-skills/) | One-time issue tracker / triage / domain layout setup |
| [`tdd`](./skills/tdd/) | Red-green-refactor |
| [`teach`](./skills/teach/) | Multi-session teaching workspace |
| [`to-questionnaire`](./skills/to-questionnaire/) | Decision → async questionnaire |
| [`to-spec`](./skills/to-spec/) | Conversation → issue-tracker spec |
| [`to-tickets`](./skills/to-tickets/) | Plan → tracer-bullet tickets with edges |
| [`triage`](./skills/triage/) | Issue/PR triage state machine |
| [`wait-what`](./skills/wait-what/) | Re-pitch a message that didn't land |
| [`wayfinder`](./skills/wayfinder/) | Large work as a map of decision tickets |
| [`wizard`](./skills/wizard/) | Interactive bash wizard for human-only steps |
| [`writing-for-agents`](./skills/writing-for-agents/) | How to write skills / AGENTS.md |

### From [thananon/9arm-skills](https://github.com/thananon/9arm-skills) (shippable)

| Skill | What it does |
|---|---|
| [`debug-mantra`](./skills/debug-mantra/) | Four-mantra debugging discipline |
| [`management-talk`](./skills/management-talk/) | Engineer prose → leadership channel copy |
| [`post-mortem`](./skills/post-mortem/) | Canonical bug writeup after a validated fix |
| [`qwen-agent`](./skills/qwen-agent/) | Cheap Qwen subagent via `claude-9arm` (needs that CLI) |
| [`qwenchance`](./skills/qwenchance/) | Break loops / bound context / handoff before window fills |
| [`scrutinize`](./skills/scrutinize/) | Outsider end-to-end review of plan/PR/change |

### From [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman)

| Skill | What it does |
|---|---|
| [`caveman`](./skills/caveman/) | Terse communication mode (`/caveman`); say `stop caveman` to exit |

## Layout

```
skills/
  <name>/SKILL.md   # + optional reference files / scripts
third-party/        # upstream licenses + pin SHAs
docs/agents/        # issue tracker, triage labels, domain docs pointers
CONTEXT.md
AGENTS.md
```

Domain glossary: [`CONTEXT.md`](./CONTEXT.md). Agent config: [`AGENTS.md`](./AGENTS.md).
