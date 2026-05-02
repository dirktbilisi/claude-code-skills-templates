# Skill Conventions

Four conventions that turn a skill collection into a system.

## Convention 1 — Slash Naming

### The rule

Skill names follow the pattern `/<lowercase-name>`:
- All lowercase
- No hyphens
- No underscores
- Max 15 characters
- The slash command is the primary trigger; natural-language phrase triggers are fallback

### Examples

| Bad | Good | Why |
|---|---|---|
| `/check-the-text` | `/textcheck` | hyphens fragment the command, length matters |
| `/research_deep` | `/research` | underscores feel like code, not a command |
| `/ContractReview` | `/contractcheck` | mixed case breaks tab completion |
| `/notes-from-meeting` | (folded into `/meeting`) | sub-features belong inside parent skills |

### Why it matters

Slash commands are typed, often in a hurry. Friction in typing = unused skill. Hyphens, underscores, and capitalization slow typing and break muscle memory.

### Frontmatter pattern

Every skill file (`SKILL.md`) starts with:

```yaml
---
name: skillname
description: [What it does]. Trigger when /skillname is invoked or at "[phrase 1]", "[phrase 2]". [Output path hint].
---
```

The description **must** explicitly mention the slash trigger. This lets meta-tools (skill registries, conflict checkers) parse triggers reliably.

---

## Convention 2 — Brand-Voice Pre-flight

### The rule

Any skill that produces **external-facing text** (LinkedIn posts, newsletters, slides, emails, pitches, recommendations) loads a defined brand-voice file at the start of execution. This block is mandatory in such skills:

```markdown
## Pre-flight: Load Brand Voice

**Always first:** Read `path/to/brand-voice.md` and apply the defined tonality
as the standard. Brand voice overrides default writing rules.
```

### Skills that need this

- Text generation (any external copy)
- Recommendation drafts
- Pitch / proposal generation
- Communication templates (email drafts, message templates)

### Skills that don't

- Pure operational skills (sync, status, doku)
- Code-only skills (refactor, test, build)
- Internal-only output (status reports for self)

### Why it matters

LLMs default to a generic institutional voice. Without an explicit brand-voice load, every skill produces text in that default voice. Within weeks, a brand's actual voice gets diluted — and the operator has to hand-edit every output.

The pre-flight block ensures **voice consistency is a feature of the skill itself**, not a manual checklist the operator maintains externally.

See the companion repo [`brand-voice-prompting`](https://github.com/dirktbilisi/brand-voice-prompting) for the pre-flight block structure.

---

## Convention 3 — Output Paths

### The rule

Skill output lands in a **defined repository or vault path**, not in chat limbo. Each skill declares its output path explicitly. Filename pattern is consistent across the system.

### Standard mapping (adapt to your context)

| Output Type | Path Pattern | Example skill |
|---|---|---|
| Research (deep dive) | `research/deep/YYYY-MM-DD-<topic>.md` | `/research` |
| Research (quick) | `research/quick/YYYY-MM-DD-<topic>.md` | `/quickresearch` |
| Meeting notes | `meetings/YYYY-MM-DD-<title>.md` | `/meeting` |
| Decisions | `decisions/YYYY-MM-DD-<decision>.md` | `/decision` |
| Contract reviews | `legal/contracts/YYYY-MM-DD-<title>.md` | `/contractcheck` |
| Conversation prep | `conversations/YYYY-MM-DD-<title>.md` | `/sensitivetalk` |
| Technical docs | `projects/<project>/TECHNICAL.md` | `/doku` |
| Daily logs | `daily/YYYY-MM-DD.md` | `/daily` |

### Filename pattern

`YYYY-MM-DD-<kebab-title>.md`

- **Date first** — chronological sort in any file browser
- **Kebab-case title** — no special characters, easy to grep
- **`.md` default** — `.pdf` only when the skill genuinely produces PDFs

### Why it matters

Outputs that land in chat have a 24-hour half-life. Outputs that land in defined paths are findable, searchable, linkable. The cost of defining a path is one line in the skill file. The cost of NOT defining a path is the slow rotting of the system into "where did I save that thing".

---

## Convention 4 — Conflict Handling and SKIP Documentation

### The rule

Before adding a new skill, check whether the slash name collides with an existing skill or system command. If it does, **document the SKIP explicitly** in a SKIP file, so the decision itself becomes part of the record.

### Conflict detection process

```bash
# Before adding /newskill, check:
ls .claude/skills/ | grep -i newskill
grep -ri "/newskill" .claude/

# Check system commands too (e.g. claude-code built-ins)
```

If a conflict exists, three options:

1. **Rename the new skill** (most common — find a non-conflicting name)
2. **Merge the new skill into the existing one** (as a sub-command)
3. **Skip the new skill entirely** (and document why)

### SKIP documentation pattern

When a skill is intentionally not added, create a SKIP file:

`skills-skipped-YYYY-MM-DD/<skill-name>.md`

Content:

```markdown
# SKIP: /<skill-name>

**Date:** YYYY-MM-DD
**Reason:** [why this skill was not added]
**Conflict with:** [existing skill if applicable]
**Decision:** [what to do instead]
**Reconsider when:** [condition that would change the decision]
```

### Why it matters

Without SKIP documentation, the same skill gets considered, evaluated, and rejected multiple times — once every few months when someone forgets the previous decision. SKIP files turn one-time decisions into durable knowledge.

Also: SKIP files are useful when reviewing a skill collection. They tell a future reader "we considered this, here's why we didn't do it" — which is often more valuable than "here's what we did".

---

## How to apply these conventions

### To an existing skill collection

1. Audit current skills against the four conventions
2. List violations: bad names, missing pre-flight blocks, undefined output paths, undocumented conflicts
3. Fix in order of impact: name conventions first (one-time renames), then pre-flight blocks (per skill), then output paths (per skill), then SKIP file backfilling
4. Document the conventions in a `conventions.md` file in your skill repository

### To a new skill collection

1. Adopt the conventions from day one
2. Use the templates in [`skills/`](./skills/) as starting points
3. When in doubt about a convention, document the choice — even temporary decisions decay without notes

### When to break the conventions

Conventions are not laws. Break them when:

- A specific skill genuinely needs a different output path (e.g. system integration with external tools)
- A name shorter than 15 chars genuinely doesn't capture the function (rare)
- A pre-flight block would slow down a fast-loop skill that doesn't need it

When breaking a convention, document the deviation in the skill file itself: `## Deviation: <which convention> <why>`.

The cost of a documented deviation is low. The cost of an undocumented deviation is that the next skill copies it without understanding why.
