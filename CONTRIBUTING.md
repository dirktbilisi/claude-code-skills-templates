# Contributing

Thanks for considering a contribution. The most valuable additions are **skill templates from your own production use** — not theoretical examples.

## High-value contributions

- **New skill templates** that follow the four conventions
  - Examples we'd love: linkedin-post, email-draft, contract-review, meeting-prep, decision-framework, research-brief
  - Must include: complete SKILL.md, real input/output examples, boundaries clearly stated
- **Convention refinements** with proof
  - "I tried convention X with Y change, here's what worked better"
- **Migration guides** — how you migrated from informal skills to convention-compliant ones
- **Anti-pattern reports** — failure modes you've discovered

## Lower-value contributions (please skip)

- "Tutorial skills" that re-teach Claude Code basics
- Skills that wrap a single API call without added structure
- Generic prompt collections without skill structure
- Theoretical conventions without production proof

## How to contribute

### Small fixes
PR directly.

### New skill template
1. Create `skills/example-<name>/SKILL.md` following the structure of existing examples
2. Test it in your own setup for at least 2 weeks
3. Submit PR with a note about real usage

### Convention changes
Open an issue first to discuss.

## Skill template requirements

Every skill template must include:
- ✅ Frontmatter with `name` and `description` (description must mention slash trigger)
- ✅ Clear boundaries (what the skill does NOT do)
- ✅ Defined output path
- ✅ Brand-voice pre-flight if producing external text
- ✅ Anti-patterns section
- ✅ Real-world example

## License

Contributions are released under [MIT](./LICENSE).
