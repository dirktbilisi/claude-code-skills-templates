---
name: research
description: Comprehensive, well-sourced research on a topic, delivered as a structured document. Trigger when /research is invoked or at "deep dive", "research report", "comprehensive overview", "I need a report on", "research thoroughly". Output is a saved document in research/deep/, not a chat answer. For quick overviews use /quickresearch.
---

# Research — Deep Dive

## Pre-flight: Load Brand Voice

**Always first:** Load the project's brand-voice file. Apply the defined tonality
specifically to the **Recommendations** section (section 5) of the output. The other
sections (factual research) are written in neutral analytical voice.

## Purpose

`/research` produces a structured research document on a topic, saved to a defined path.

For quick overviews use `/quickresearch` (no file output, chat only).

## Boundaries

- Not for casual questions (use plain chat)
- Not for code research (use `/codesearch`)
- Not for legal/contract review (use `/contractcheck`)
- Not for personal opinions or essays (use `/essay`)

## Input

`/research <topic>` — free-form topic description.

If no topic given, ask: "What should I research?" and wait.

## Procedure

### Phase 1 — Scope clarification

If topic is ambiguous, ask 1-2 clarifying questions in a single batched message:

- What's the use case for this research? (decision support / general knowledge / writing prep)
- How deep — overview, working knowledge, expert level?
- Time horizon — current state only, or historical context too?

Wait for response before proceeding. Do NOT make multiple back-and-forth question rounds.

### Phase 2 — Source gathering

1. Use web search for current sources (last 12-24 months)
2. Cross-reference at least 3 independent sources per major claim
3. Note source quality:
   - **A** — peer-reviewed academic, official government data, established industry reports
   - **B** — quality journalism, expert blogs, industry publications
   - **C** — secondary sources, opinion pieces, marketing material

Prioritize A-sources. Use B-sources for context. Use C-sources only when nothing else exists, and label them as such.

### Phase 3 — Synthesis

Structure the document with these sections:

1. **Topic & scope** — what the research covers, what it doesn't, why it matters (one paragraph)
2. **Current state** — facts and consensus, with inline source citations [1], [2]
3. **Key debates** — where experts disagree, with positions of each side
4. **Gaps and uncertainties** — what is unknown, what is contested, what is changing fast
5. **Practical implications and recommendations** — what this means for the user's specific context (this section applies brand voice)
6. **Sources** — numbered list with URLs, source quality grade, access date

### Phase 4 — Save and report

1. Save to: `research/deep/YYYY-MM-DD-<topic-slug>.md`
   - Slug: kebab-case, max 5 words, no special chars
   - Example: `research/deep/2026-05-02-ai-in-corporate-learning.md`
2. Report the file path back to user
3. Brief metadata: word count, source count, A/B/C source distribution

## Output

- File: `research/deep/YYYY-MM-DD-<kebab-topic>.md`
- Format: Markdown with all six sections
- Length: 1500-3000 words typical, 5000+ for complex topics

## Confirmation pattern

```
✓ Research saved to: research/deep/YYYY-MM-DD-<topic>.md

Length: ~XXXX words
Sources: N total (A: x, B: y, C: z)
Key debates section: N positions documented

Should I dig deeper on any specific section?
```

## Rules

- **No hallucination** — if a source can't be found for a claim, mark the claim as unverified or remove it
- **Inline citations** — every factual claim has [N] reference to the sources section
- **Date stamping** — every source has an "accessed YYYY-MM-DD" note
- **Quality labeling** — A/B/C grade per source in the sources list
- **Recommendations stay in scope** — section 5 recommendations come from the research, not from external opinion
- **Brand voice applies only to section 5** — the rest is neutral analytical writing

## Anti-patterns

- ❌ Long preamble before the actual research
- ❌ Padding with "It's important to note that..." filler sentences
- ❌ Recommendations not grounded in the research (external advice masquerading as analysis)
- ❌ Claims without sources
- ❌ Single-source dependencies for major claims
- ❌ Outdated sources (2+ years old) treated as current

## Iteration

If user wants to dig deeper on a section:

1. Identify which section (1-6) needs expansion
2. Run a focused sub-research on that section only
3. Append to the original document with a `## Update YYYY-MM-DD` block
4. Do NOT overwrite the original — additive changes only, so the research history stays visible

## Length guidance

- **Quick overview** (use /quickresearch instead): 300-800 words
- **Standard deep dive**: 1500-3000 words
- **Comprehensive**: 3000-5000 words
- **Reference document**: 5000+ words (rare, ask if user wants this depth)

If the topic doesn't require deep dive depth, switch to `/quickresearch` and explain why.
