---
name: textcheck
description: Detect and remove linguistic markers of AI-generated text so it sounds human and matches the defined brand voice. Trigger when /textcheck is invoked or at "humanize this text", "sounds like ChatGPT", "make this less AI", "remove AI fingerprints", "too smooth", "too formal" or when a text needs a final pass before publication. Output is the rewritten text in chat (or saved to drafts/ if a path is given).
---

# Textcheck — Humanize AI-Generated Text

## Pre-flight: Load Brand Voice

**Always first:** Load the project's brand-voice file (default: `brand-voice.md` in the brand directory). Apply the defined tonality as the standard. Brand voice overrides default writing rules.

If no brand-voice file exists, ask the user for:
1. One sentence describing the voice (positioning + 2 anti-markers)
2. 5-10 forbidden words/phrases
3. One sample of "good" voice (2-3 sentences)

Treat these as the temporary brand-voice block for this session.

## Purpose

`/textcheck` takes a text that sounds AI-generated (smooth, formal, generic) and rewrites it to sound human and on-brand.

Complementary to text-generation skills: those create, this cleans.

## Boundaries

- Not for translation between languages
- Not for fact-checking (use a research skill)
- Not for grammar correction only (use a standard editor)
- Not for stylistic transformation between voices (e.g. formal → casual without brand definition)

## Input

`/textcheck <text>` — the text to humanize

Or: paste the text after the command, on the next lines

If no text given: ask "Which text should I check?" and wait.

## Procedure

### Phase 1 — Load brand voice

See pre-flight above.

### Phase 2 — Detect AI markers

Scan the text for common AI-generation markers:

**Vocabulary markers:**
- "Let's dive in", "Let me explain"
- "In today's [adjective] landscape"
- "Now more than ever"
- "The new normal"
- "Unleash", "journey", "ecosystem", "holistic", "synergy"
- "It's important to note that..."
- "Furthermore", "Moreover", "However" used as connective fluff

**Structural markers:**
- Three-part lists when one item would do
- Hedging clauses that add no information ("It can be helpful to consider that...")
- Generic intro-sentences before the actual point
- Wrap-up sentences that just restate the topic
- Emoji-heavy or emoji-decorated lists

**Tonal markers:**
- Excessive enthusiasm ("amazing", "incredible", "wonderful")
- Coaching-speak ("you've got this", "your unique journey")
- Marketing-speak ("transform", "unleash", "elevate")
- Hedge-language for legal cover

### Phase 3 — Rewrite

Three-pass rewrite:

**Pass 1 — Cut:** Remove all forbidden vocabulary. Remove hedging clauses. Remove generic intro and outro sentences.

**Pass 2 — Sharpen:** Replace passive constructions with active verbs. Replace abstract claims with specific examples or numbers when available.

**Pass 3 — Voice:** Apply the brand-voice positive markers. Ensure the opening matches the defined opening pattern. Ensure the closing matches the defined closing pattern.

### Phase 4 — Self-check before output

Before returning the text, check:
- Does this sound like the brand-voice example, or could it be from any consultant?
- Has every sentence earned its place?
- Are there still any words from the forbidden vocabulary list?

If any answer is "could improve", do another pass.

### Phase 5 — Output

Return the rewritten text in chat. If the user specified a path (e.g. "save as draft for LinkedIn"), save to:
`drafts/YYYY-MM-DD-<short-title>.md`

Brief notes alongside the rewritten text:
- Number of forbidden-vocabulary instances removed
- Length change (words before / words after)
- Any judgment calls made (e.g. "kept the original opening because it already matched the voice")

## Anti-patterns

This skill does NOT:

- Translate between languages (different skill)
- Add information that wasn't in the original
- Change factual claims
- Rewrite something that was already on-brand (do nothing in that case, say so)
- Apply a generic "make it punchier" pass — it applies the *specific* defined voice, not a generic punch-up

## Iteration

If the output still sounds AI-generated:

1. Check the brand-voice file — is the example field genuinely sharp?
2. Check the forbidden vocabulary list — is it specific enough?
3. Add the failure-mode words from the bad output to the forbidden vocabulary list
4. Run `/textcheck` again

The brand-voice file is the lever. Iterate that, not the text.

## Examples

**Input (AI-sounding):**
> "In today's rapidly evolving business landscape, it's more important than ever to leverage cutting-edge AI tools to unleash your team's full potential. Let me explain how this works."

**After /textcheck (with a contrarian-substantive brand voice):**
> "Most teams using AI tools see no productivity gain. The difference between teams that benefit and teams that don't is not the tool — it's whether they redesigned the work around it."

Same topic, opposite voice.
