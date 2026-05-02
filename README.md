# Claude Code Skill Templates

Skill templates and conventions for Claude Code, distilled from production use across solo-operator workflows.

This repo is for anyone building [Claude Code](https://claude.com/claude-code) skills who has noticed that ad-hoc skills accumulate inconsistency: different output paths, different naming conventions, different ways of handling brand voice, different conflict-resolution patterns. This is the small set of conventions that makes a skill collection feel like a system instead of a junk drawer.

## What's in here

- **[Skill Conventions](./conventions.md)** — four conventions that make a skill collection coherent: naming, brand-voice pre-flight, output paths, conflict handling
- **[Skill Anatomy](./anatomy.md)** — what every skill needs at minimum, what every skill should consider
- **[Templates](./skills/)** — three ready-to-adapt skill templates:
  - `example-doku` — technical documentation skill
  - `example-textcheck` — text humanization skill
  - `example-research` — structured research skill

## Why this exists

Skill systems rot fast without conventions. Within a few weeks of building skills informally, you end up with:

- Two skills that solve overlapping problems with different output paths
- Inconsistent naming (some kebab-case, some snake_case, some German, some English)
- Some skills that respect brand voice, others that produce generic output
- No clear rule for what to do when a new skill name conflicts with an existing one

The conventions in this repo are not theoretical — they emerged from running ~25 skills in production for several months and feeling the friction of inconsistency directly.

## Who this is for

- Solo operators building 5+ skills for personal workflows
- Small teams sharing a Claude Code skill library
- Anyone who has built one Claude Code skill and is about to build a second one

If you have one skill, you don't need conventions yet. If you have ten, you needed them yesterday.

## The four conventions in one paragraph

1. **Slash naming**: `/<lowercase-no-hyphen>`, max 15 characters. The slash command is the primary trigger; phrase triggers are fallback.
2. **Brand-voice pre-flight**: any skill producing external-facing text loads a defined brand-voice file at the start.
3. **Output paths**: outputs land in defined vault/repo paths, not in chat limbo. Filename pattern: `YYYY-MM-DD-<kebab-title>.md`.
4. **Conflict handling**: before adding a skill, check for slash-name collisions. Document skipped skills explicitly so the SKIP itself becomes a record.

Details in [`conventions.md`](./conventions.md).

## License

[MIT](./LICENSE) — use, modify, redistribute freely.

## Maintainer

Built and maintained by [Dirk Häger](https://github.com/dirktbilisi) — independent learning architect at [focusinstitute.io](https://focusinstitute.io)
