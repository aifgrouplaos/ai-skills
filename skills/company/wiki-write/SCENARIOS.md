# Wiki write — evidence-routing scenarios

Run these scenarios when changing Step 4 evidence routing or its conflict gate. Observe the questions and the resulting working copy through the normal Wiki write flow; implementation wording and internal reasoning are not test outputs.

Use a disposable wiki project and synthetic evidence. Stop before `/wiki-publish`.

## Shared — meaning before corroboration

**Given:** an Overview template; the user says “Dispatchers assign deliveries and riders report delivery outcomes”; code identifies an API service that coordinates assignments, PostgreSQL as durable storage, and Redis for asynchronous coordination, but contains no reliable statement of product purpose or actor outcomes.

**When:** the authoring pass reaches Step 4.

**Observe:**

- The user is grilled for purpose, scope, actors or surfaces, behavior, and intended contracts before code supplies page claims.
- Code is inspected afterward and may corroborate deployed components, shared-contract identifiers, and observable behavior.
- The working copy does not infer product intent or business meaning from component names.

## Stack — implementation before gaps

**Given:** a Backend template; a repository that makes Node.js, the delivery-module boundary, and its dependencies verifiable; and a user answer that the Platform team owns the boundary. Ownership rationale is absent.

**When:** the authoring pass reaches Step 4.

**Observe:**

- Verifiable stack facts are pre-filled from code before questions begin.
- The grill asks only for unresolved placeholders, intent, and judgment calls.
- The working copy distinguishes observed implementation from human-supplied rationale.

## Decision — intent before implementation state

**Given:** an ADR template; a user decision to use an asynchronous delivery-status boundary; and code that implements that design but does not record why it was chosen or which alternatives were rejected.

**When:** the authoring pass reaches Step 4.

**Observe:**

- The user is grilled for the decision, alternatives, rationale, and consequences before code inspection.
- Code is used afterward only to verify current implementation state.
- The working copy does not present the implemented design as self-justifying evidence of the decision.

## Language — usage before canonical choice

**Given:** a Glossary template where documentation uses “customer,” code uses both `customer` and `account_holder`, and neither source defines their relationship; the user resolves Customer as the service recipient and Account holder as the person responsible for billing, who may be different.

**When:** the authoring pass reaches Step 4.

**Observe:**

- Existing documentation and code terminology is inspected before the grill.
- The grill surfaces the ambiguity and asks the user to define meanings and choose canonical terms.
- The working copy records the resolved language rather than selecting the most frequent identifier automatically.

## Conflict — unresolved until a human decides

**Given:** any template where the user says authentication is session-based, code shows bearer-token middleware, and existing documentation says API-key authentication.

**When:** the authoring pass compares the evidence.

**Observe:**

- All three claims and their sources are stated to the user.
- The affected placeholder remains unresolved while the user decides the authority or reconciliation.
- No working-copy claim is filled by choosing the source inspected last.

## Regression guards

For every scenario, also observe:

- one-template focus;
- project slug resolution from the live `projects` index;
- exact-path template and index reads;
- locale-absolute `/en/` links;
- ephemeral working copies;
- index propagation where applicable; and
- handoff to Wiki publish without a Wiki.js mutation.

## Captured dry-run fixtures

These compact fixtures record the public interaction and working-copy outcome expected from the synthetic evidence above. Template headings are abbreviated; page-specific template tests own the complete rendered shape.

### Shared fixture

**Visible order:** ask “What does the project do, and what outcome does each actor need?” → receive the human answer → inspect code → report API service, PostgreSQL, and Redis as corroborating system facts.

**Working-copy excerpt:**

```markdown
## Purpose

Dispatchers assign deliveries and riders report delivery outcomes.

## System at a glance

- API service — coordinates delivery assignments.
- PostgreSQL — durable system component.
- Redis — asynchronous coordination component.
```

### Stack fixture

**Visible order:** report the observed runtime, module boundaries, and dependencies → ask who owns the boundary and why it exists.

**Working-copy excerpt:**

```markdown
## Runtime and boundaries

- Runtime: Node.js
- Boundary: delivery module
- Owner: Platform team — confirmed by the user
```

### Decision fixture

**Visible order:** ask for decision, alternatives, rationale, and consequences → receive the human decision → inspect code → report whether implementation matches it.

**Working-copy excerpt:**

```markdown
## Decision

Use an asynchronous delivery-status boundary.

## Current implementation

The inspected code implements the selected asynchronous boundary.
```

### Language fixture

**Visible order:** report observed uses of “customer” and `account_holder` → ask whether they name one concept or two → receive the canonical distinction.

**Working-copy excerpt:**

```markdown
## Customer

The person receiving the service.

## Account holder

The person responsible for the billing account; may differ from the Customer.
```

### Conflict fixture

**Visible order:** report session authentication from the user and bearer-token authentication from code → ask which is authoritative or how they reconcile.

**Outcome:** no working copy is produced while the conflict is unresolved. The authentication claim remains blocked instead of adopting either source.
