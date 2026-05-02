# Skill Anatomy

What every Claude Code skill needs at minimum. What every skill should consider.

## File structure

A skill lives in `.claude/skills/<skillname>/SKILL.md`. The directory name and the slash command match exactly.

```
.claude/
└── skills/
    └── doku/
        └── SKILL.md
```

Skills can also include companion files (templates, references, helper scripts) in the same directory:

```
.claude/
└── skills/
    └── research/
        ├── SKILL.md
        ├── template-deep.md
        ├── template-quick.md
        └── source-checklist.md
```

## Mandatory sections

Every `SKILL.md` has these sections, in this order:

### 1. Frontmatter

```yaml
---
name: skillname
description: [What it does]. Trigger when /skillname is invoked or at "[phrase 1]", "[phrase 2]". [Output path hint].
---
```

The frontmatter is parsed by Claude Code to register the skill. Everything else lives in the body.

### 2. Title and Purpose (one paragraph)

Plain English: what this skill does, when to use it.

### 3. Boundaries (1-3 lines)

What this skill does NOT do. What overlapping skills handle different cases.

### 4. Input pattern

What the user types after the slash command. Examples.

### 5. Procedure (step by step)

The actual instructions Claude follows when the skill is invoked. Numbered steps. Each step concrete and actionable.

### 6. Output specification

Where the output goes (path, filename). What format. What metadata.

### 7. Confirmation pattern

What the skill reports back to the user when done.

## Optional sections (use when relevant)

### Pre-flight: Brand Voice load

Required if the skill produces external-facing text. See [Convention 2](./conventions.md#convention-2--brand-voice-pre-flight).

### Pre-flight: Reference loads

If the skill needs domain knowledge from external files (style guides, technical references, frameworks).

### Iteration / refinement section

For skills with multi-step user feedback (e.g. text generation skills that get iterative edits).

### Error handling

What to do if the skill can't complete (missing inputs, API failures, file conflicts).

### Anti-patterns

What this skill should NOT do, even if asked. Useful for skills with a clear domain.

### Examples

Worked examples of input → output. Especially useful for skills with non-obvious behavior.

## Minimum viable skill — example

Here is the smallest valid skill, all sections accounted for:

```markdown
---
name: timestamp
description: Insert a current ISO 8601 timestamp into the chat. Trigger when /timestamp is invoked.
---

# Timestamp — Insert Current Time

## Purpose

Inserts the current local time as ISO 8601, for use in notes and logs.

## Boundaries

- Not for time zone conversions (use a calendar tool)
- Not for date arithmetic
- Not for scheduling

## Input

`/timestamp` — no arguments needed.

## Procedure

1. Read the current local time.
2. Format as ISO 8601 with timezone offset: `YYYY-MM-DDTHH:MM:SS+HH:MM`.
3. Output the timestamp as plain text.

## Output

Single line, plain text, no extra characters.

## Confirmation

The timestamp itself IS the confirmation.
```

That is a complete skill. Most skills will be longer, but they should cover the same sections in the same order.

## A more substantial skill — pattern

For skills that produce file-based output (research reports, decisions, meeting notes), the structure expands:

```markdown
---
name: research
description: Comprehensive structured research on a topic. Trigger when /research is invoked or at "deep dive", "research report", "comprehensive overview". Output lands in research/deep/YYYY-MM-DD-<topic>.md.
---

# Research — Deep Dive

## Pre-flight: Brand Voice
**Always first:** Load `02-brand/brand-voice.md`. Apply tonality to the recommendations section.

## Purpose

Produces a structured research document on a topic, saved to a defined path.
For quick overviews use `/quickresearch` (no file output, chat only).

## Boundaries

- Not for casual questions (use plain chat)
- Not for code research (use `/codesearch`)
- Not for legal/contract review (use `/contractcheck`)

## Input

`/research <topic>` — free-form topic description.

If no topic given, ask: "What should I research?" and wait.

## Procedure

### Phase 1 — Scope clarification (if needed)

If topic is ambiguous, ask 1-2 clarifying questions:
- What's the use case for this research?
- How deep — overview, working knowledge, expert level?

Wait for response before proceeding.

### Phase 2 — Source gathering

1. Use web search for current sources
2. Cross-reference at least 3 independent sources
3. Note source quality (academic, industry report, blog, etc.)

### Phase 3 — Synthesis

Structure the document with these sections:
1. **Topic & scope** (one paragraph)
2. **Current state** (3-5 paragraphs with citations)
3. **Key debates** (where experts disagree, with positions)
4. **Practical implications** (what this means for the user's context)
5. **Recommendations** (3-5 concrete next steps — applies brand voice)
6. **Sources** (numbered list with URLs)

### Phase 4 — Save and confirm

1. Save to `research/deep/YYYY-MM-DD-<topic-slug>.md`
2. Report file path back to user
3. Offer follow-up: "Should I dig deeper on any specific section?"

## Output

- File: `research/deep/YYYY-MM-DD-<kebab-topic>.md`
- Format: Markdown, all six sections
- Length: 1500-3000 words typical

## Confirmation

```
✓ Research saved to: research/deep/YYYY-MM-DD-<topic>.md
  Length: ~XXXX words
  Sources: N independent sources cross-referenced
  
  Should I dig deeper on any section?
```
```

## What separates good skills from bad

Three markers of a good skill:

1. **It has a clear no-go list.** Skills that "do everything in domain X" are too vague. Skills that say "this and not that" are usable.
2. **The output goes to a defined place.** No "I'll send it back in chat" — that's an unstructured ad-hoc tool, not a skill.
3. **It can be invoked twice in the same week without reading the docs.** If you have to look up how to use your own skill, the input pattern is too complex.

Three markers of a bad skill:

1. **It's a renamed plain prompt.** If `/sumarize` just runs the model with "summarize this", it's not a skill, it's a typo-saver.
2. **It overlaps unclearly with other skills.** Two skills that both kind-of-handle research, with no clear boundary.
3. **It produces no persistent artifact.** If nothing of value is created beyond chat, the skill provides no leverage over time.

## When NOT to make a skill

Some workflows look like skills but shouldn't be. Don't make a skill for:

- One-time tasks (just do them in chat)
- Tasks where the input/output is genuinely unique each time
- Tasks where the procedure changes faster than you can update the skill
- Tasks where Claude Code's default behavior is already correct
