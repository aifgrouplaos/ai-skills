---
name: evidence-research
description: Evidence-first research using search/browsing tools, primary sources, and source-backed synthesis, with upfront user-intent grilling when the research goal is ambiguous. Use when the user asks to investigate a prompt or question, find accurate/current information, compare claims, gather references, provide evidence, cite original sources, produce a researched answer, or spend extra time searching for the best available sources.
---

# Evidence Research

## Overview

Use this skill to turn a user question into a sourced answer that shows its evidence trail. The goal is accuracy over speed: search broadly enough to find the best available sources, verify claims against origin sources, and make uncertainty visible.

## Workflow

1. Clarify intent before searching.
   - If the user already gave a concrete research question, audience, scope, and desired output format, restate the target briefly and continue.
   - If the prompt is broad, ambiguous, high-stakes, goal-sensitive, or could branch into several different research products, use `$grilling` before searching.
   - With `$grilling`, ask the current frontier of user decisions: what decision the research should support, required scope, acceptable source types, output format, depth, deadline/freshness needs, exclusions, and confidence bar.
   - Do not ask the user for facts that can be searched. Ask for preferences, intent, constraints, and decisions.
   - Begin research only after the user confirms the shared understanding.

2. Restate the research target as one or more answerable questions.
   - Preserve the user's exact constraints: dates, geography, jurisdiction, audience, technical version, budget, or source preferences.
   - Ask a clarifying question only when a missing constraint would make the answer materially misleading. Otherwise, state the assumption and continue.

3. Search in passes.
   - Start with broad queries to map the topic and vocabulary.
   - Run targeted queries for primary sources, official docs, datasets, papers, standards, laws, changelogs, statements, or original announcements.
   - Run follow-up queries for disagreement, criticism, limitations, and recent updates.
   - For time-sensitive questions, search explicitly for the latest date/version and compare publication dates.

4. Prefer origin sources.
   - Use official documentation, original papers, standards bodies, government pages, company announcements, public filings, repository release notes, datasets, or direct transcripts when available.
   - Use reputable secondary sources to discover leads, context, or competing interpretations; do not let them replace origin sources for core claims.
   - For niche claims, find at least two independent sources or mark the claim as weak.

5. Build an evidence log while researching.
   - Track source title, author/organization, publication or update date, URL, source type, and the specific claim it supports.
   - Note whether the source is primary, secondary, commentary, dataset, or user-generated.
   - Record conflicts between sources instead of smoothing them away.

6. Synthesize only after source review.
   - Separate what the sources establish from your inference.
   - Prefer precise claims over broad summaries.
   - Include caveats for stale sources, limited samples, missing data, ambiguous definitions, or conflicting evidence.

7. Return a source-backed answer.
   - Lead with the direct answer or strongest finding.
   - Cite each important factual claim with a link to the source.
   - Include a short evidence section listing the most important sources and why they matter.
   - Include a confidence note when evidence is incomplete, contradictory, or rapidly changing.

## Source Standards

- Use the search/browsing tools available in the current environment when the answer depends on external facts, recent information, primary-source verification, or exact citations.
- Open and inspect sources before relying on them; search-result snippets are not evidence.
- Prefer sources with stable URLs, clear dates, named organizations/authors, and direct access to the underlying claim.
- Avoid citing low-quality summaries when a primary source is available.
- For legal, medical, financial, safety, or security topics, cite authoritative sources and clearly bound the answer as informational rather than professional advice.
- Do not fabricate citations, dates, quotes, or source contents. If a source cannot be accessed, say so.

## Output Pattern

Use this shape unless the user requested a different format:

```markdown
**Answer**
[Concise answer with citations.]

**Evidence**
- [Source title](URL) - source type, date, why it matters.
- [Source title](URL) - source type, date, why it matters.

**Notes**
[Assumptions, conflicts, limits, and confidence.]
```

For larger research tasks, add:

```markdown
**Method**
[Search passes performed, key query families, and inclusion/exclusion choices.]
```
