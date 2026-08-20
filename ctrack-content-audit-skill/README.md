# ctrack-content-audit

A Claude Code skill for auditing and rewriting Ctrack content. Combines a full AI-writing pattern detector with a Ctrack-specific accuracy layer — catches both "this reads like it was written by AI" and "this stat has no source" / "this contradicts what the page says three paragraphs down."

Built for the SEO/content team from real findings on the fleet-management and fleet-tracking page reviews (Aug 2026).

## What it catches

**AI-writing patterns** — hollow buzzwords (delve, leverage, robust, seamless...), template phrases, unsourced vague attributions, keyword stuffing, structural tells (Title Case headings, em-dash overuse, uniform sentence rhythm), and more — organized by severity (P0 credibility killers → P2 polish).

**Ctrack-specific accuracy issues** — every number checked against a maintained table of Ctrack's real, sourced facts. Flags stats with no citation, pages that contradict their own stated specs, duplicated trust badges, inconsistent heading case, and unnamed-authority superlatives ("leading," "best," "trusted by") — including in `<title>` tags and schema, not just visible copy.

It never invents a source or a number to fill a gap. Unsourced claims get flagged or cut — never guessed at.

## Install

Copy `SKILL.md` and `LICENSE` into your Claude Code skills directory:

```bash
mkdir -p ~/.claude/skills/ctrack-content-audit
cp SKILL.md LICENSE ~/.claude/skills/ctrack-content-audit/
```

That's it — Claude Code auto-discovers any folder in `~/.claude/skills/` with a `SKILL.md`. Restart your session (or start a new one) to pick it up.

## Use it

Plain language, no special syntax:

- `"Audit https://ctrack.com/au/solutions/[page] for AI writing and accuracy issues"` — detect mode, flags only
- `"Rewrite this page copy"` — flags and fixes, returns clean copy
- `"Clean up draft.md in place"` — edit mode, fixes a file directly

## Keep the reference table current

Section B of `SKILL.md` has a table of Ctrack's known facts (certifications, sourced stats) and known unverified claims. Every audit is a chance to grow it — if you confirm a new source or find a new unsourced claim, update the table so the next audit catches it automatically.

## Attribution

Section A (the AI-writing pattern catalog) incorporates and adapts the open-source [avoid-ai-writing](https://github.com/conorbronsdon/avoid-ai-writing) project by Conor Bronsdon, MIT License — see `LICENSE` and the attribution section at the bottom of `SKILL.md`. Section B (the Ctrack-specific layer) is original, built for this team.
