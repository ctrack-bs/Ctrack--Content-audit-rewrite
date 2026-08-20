---
name: ctrack-content-audit
description: Full-power AI-writing audit and rewrite skill, combined with a Ctrack AU accuracy layer. Section A is a complete adaptation of the open-source avoid-ai-writing pattern catalog — all word/phrase tiers, structural tells, voice profiles, context profiles with tolerance matrix, house-style config support, detect/rewrite/edit modes. Section B adds Ctrack-specific checks built from real findings on the fleet-management and fleet-tracking page reviews — stat sourcing against Ctrack's known facts, internal contradictions, duplicate elements, heading-case drift, unnamed-superlative claims. Use for any content review, Ctrack or otherwise; Section B activates automatically when the content is Ctrack's. Trigger on "audit this," "detect AI patterns," "clean up AI writing," "Ctrack content QA," "check this against our facts," or when a ctrack.com/au/ URL is named.
version: 2.0.0
license: MIT
metadata:
  author: Ctrack AU content team
  origin: >
    Section A incorporates and adapts the open-source avoid-ai-writing project
    (github.com/conorbronsdon/avoid-ai-writing), MIT License, Copyright (c) 2026
    Conor Bronsdon — full text retained at the end of this file per the license's
    requirement that the copyright and permission notice be included in copies and
    substantial portions of the software. Section B is original to this skill, built
    for Ctrack AU from the fleet-management and fleet-tracking page audits (2026-08-18/19).
---

# Ctrack AU Content Audit

One skill, two layers. **Section A** is the full AI-writing pattern catalog — everything that makes copy sound machine-generated, regardless of who or what wrote it. **Section B** is a Ctrack-specific accuracy layer on top — checks for wrong or contradicted numbers, duplicated elements, and inconsistent styling that a pattern catalog alone won't find, because they're about whether a claim is *true*, not whether it's *phrased* like AI output.

Run both together on Ctrack content. On non-Ctrack content, Section A works standalone — Section B's checks simply won't find anything to match against and can be skipped.

## What this skill is and isn't

This is a **writing-quality tool**, not a verdict. The patterns flagged in Section A are statistically more common in LLM output, but humans on autopilot — especially under deadline pressure, in unfamiliar genres, or writing in a second language — produce the same shapes. Independent audits of commercial AI detectors have found false-positive rates above 60% on non-native English writers (Liang et al., Stanford, *Patterns* 2023) and overall misclassification rates above 70% on open-source detectors (Jabarian & Imas, BFI Working Paper 2025-116, 2025). Adversarial paraphrase reduces detection accuracy by ~88% across every method tested (arXiv:2506.07001, 2025).

Use these patterns as a signal for cleaning up writing and assessing whether a piece reads as AI-generated — never as the sole basis for a consequential decision (attribution, academic integrity, hiring). Section B's checks are a different kind of finding: those are about factual accuracy and internal consistency, and are worth acting on directly once verified.

## Modes

**`rewrite`** (default) — Flag issues and rewrite the text to fix them.

**`detect`** — Flag only, no rewriting. Use when the writer wants to see what's flagged and decide what to fix themselves, when auditing published content you don't want altered, or for a quick scan.

**`edit`** — Edit a named file in place. Minimal, targeted edits to flagged spans only — leave already-clean passages untouched. Don't edit quoted material, code blocks, tables, or text attributed to someone else; flag those instead. Re-read the file after editing and confirm the flagged patterns are resolved.

Trigger detect mode on "detect," "flag only," "audit only," "scan." Trigger edit mode when a file is named and the writer asks to fix it in place. Default to rewrite mode otherwise.

**Invocation.** Natural language is enough. Power users can pass explicit options: `[--mode rewrite|detect|edit]`, `[--voice casual|professional|technical|warm|blunt]`, `[--context linkedin|blog|technical-blog|investor-email|docs|casual]`, `[--file PATH]`, `[--iterate N]` (max 2), `[--style CONFIG|GUIDE]`.

**Iterate to convergence (optional).** Rewrite mode already runs one corrective second pass — that *is* pass 2, so `--iterate` doesn't stack on top of it. On request, repeat the audit→rewrite cycle until clean or N passes (cap 2). Report how many passes it took.

---

In **rewrite** mode: (1) audit — identify every issue from Section A and Section B, citing the text; (2) rewrite — a clean version with every editable Section A issue fixed (Section B issues get flagged, not silently resolved — see below); (3) diff summary of what changed and why.

In **detect** mode: (1) audit, citing text, grouped by severity; (2) assessment — clear problem vs. judgment call.

In **edit** mode: (1) read the named file; (2) minimal in-place fixes via the Edit tool; (3) verify by re-reading.

**Section B issues are never silently fixed by a rewrite.** An unsourced stat, a contradiction, or a claim with no reference-table match gets flagged in every mode, including rewrite — the fix requires a real number or a decision only a person can make. Rewrite mode may cut an unsupportable claim if the surrounding sentence still works without it, but never replaces it with an invented figure.

---

# Section A — AI-writing pattern catalog

*(Full adaptation of avoid-ai-writing, MIT License, Conor Bronsdon.)*

## What to remove or fix

### Formatting
- **Em dashes (— and --)**: Target zero, hard max one per 1,000 words, in headings too. Carve-out: an em dash as the separator in a bulleted list item opening with a bolded lead term or link (`- **Term** — description`) is typography, not a prose splice.
- **Bold overuse**: One bolded phrase per major section at most. If something's important enough to bold, restructure the sentence to lead with it instead.
- **Emoji in headers**: Remove entirely. Social posts may use 1-2, end of line only.
- **Excessive bullet lists**: Convert bullet-heavy sections to prose. Bullets only for genuinely list-like content.
- **Curly quotation marks and apostrophes**: Weak, corroborating-only signal — most human prose has these too (Word/Docs/macOS/iOS auto-curl). Meaningful mainly in plain-text contexts (code comments, commit messages).
- **Immaculate typography in casual registers**: Same weak-signal tier as curly quotes. When editing a human's casual text, preserve their typos and idiosyncratic capitalization rather than correcting them.

### Sentence structure
- **"It's not X — it's Y"**: Max one per piece. Includes the split-sentence form ("The headline isn't the speed. The real story is Y."), the multi-negation countdown, and the tailing negation ("...no guessing" → write the real clause or cut).
- **Hollow intensifiers**: Cut `genuine(ly)`, `real` (as intensifier), `truly`, `quite frankly`, `to be honest`, `let's be clear`, `it's worth noting that`, `actually` (when pure emphasis — default fix is deletion, not substitution).
- **Vague endorsement**: Cut or replace "worth reading/exploring/your time" — say why instead.
- **Hedging**: Cut `perhaps`, `could potentially`, `it's important to note that`, `to be clear`.
- **Missing bridge sentences**: Each paragraph should connect to the last.
- **Compulsive rule of three**: Vary groupings; max one adjective-triad per piece.

### Words and phrases to replace

Tier 1 = always flag (5-20x more common in AI text). Tier 2 = flag when 2+ cluster in one paragraph. Tier 3 = flag only at high density (~3%+ of words). Each entry covers morphological variants unless a variant carries a distinct honest sense.

**Tier 1A — AI frequency markers** (replace on sight): delve/delve into, landscape (metaphor), tapestry, realm, paradigm, embark, beacon, testament to, robust, comprehensive, cutting-edge, leverage (verb), pivotal, underscores, meticulous(ly), seamless(ly), game-changer, hit differently, watershed moment, marking a pivotal moment, the future looks bright, only time will tell, nestled, vibrant, thriving, despite challenges...continues to thrive, showcasing, deep dive/dive into, unpack(ing), bustling, intricate/intricacies, complexities, ever-evolving, enduring, daunting, holistic(ly), actionable, impactful, learnings, thought leader(ship), best practices, at its core, synergy/synergies, interplay, keen (intensifier), genuinely/genuine (intensifier), symphony (metaphor), embrace (metaphor), load-bearing (metaphor — hyphen required; construction terms like "load-bearing wall/beam/column" are exempt).

**Tier 1B — Clarity edits, not authorship evidence** (same fix, weaker claim — these fire on ordinary human professional writing at a meaningful rate too): utilize→use, in order to→to, due to the fact that→because, serves as→is, features (verb)→has/includes, boasts→has, presents (inflated)→is/shows/gives, commence→start, ascertain→find out, endeavor→effort/try. In detect mode, report these separately from Tier 1A — a wordiness fix is not authorship evidence.

**Tier 2 — flag in clusters**: harness, navigate/navigating, foster, elevate, unleash, streamline, empower, bolster, spearhead, resonate/resonates with, revolutionize, facilitate(s), underpin, nuanced, crucial, multifaceted, ecosystem (metaphor), myriad, plethora, encompass, catalyze, reimagine, galvanize, augment, cultivate, illuminate, elucidate, juxtapose, paradigm-shifting, transformative/transformation, cornerstone, paramount, poised (to), burgeoning, nascent, quintessential, overarching, quietly, deeply (significance collocations only — "deeply nested" doesn't count), underpinning(s).

**Tier 3 — flag only at high density**: significant(ly), innovative/innovation, effective(ly), dynamic(s), scalable/scalability, compelling, unprecedented, exceptional(ly), remarkable(ly), sophisticated, instrumental, world-class/state-of-the-art/best-in-class, verbatim (usually cut — redundant with the verb).

**Tier 3 phrases** (flag at 2+ same-phrase repeats, or 3+ distinct phrases in one piece): emerging sector/space/category, the integration of (X with Y), the intersection of (X and Y), community-driven, long-term sustainability, user engagement, decentralized compute, (sustainable) reward emissions, tokenized incentive structures, designed for long-term [X].

### Template phrases
- "a [adjective] step towards/forward for [X]" → describe the specific outcome.
- "Whether you're [X] or [Y]" → false-breadth filler. Pick the real audience or cut, unless the endpoints are genuinely concrete (a real size/type range is fine — the tell is vagueness, not the construction itself).
- "I recently had the pleasure of [verb]-ing" → just say what happened.

### Transition phrases
"Moreover/Furthermore/Additionally" → restructure or use "and/also." "In today's [X]" / "In an era where" → cut or be specific. "It's worth noting that/Notably" → just state it. "Here's what's interesting" → let content signal its own importance. "In conclusion/In summary" → your conclusion should be obvious. "When it comes to" → talk about the thing directly. "At the end of the day" → cut. "That said" → cut or use but/yet/however, don't overuse.

### Structural issues
- **Uniform paragraph length**: vary deliberately, some 1-2 sentence, some longer.
- **Formulaic openings**: lead with the news/insight, not broad context first.
- **Suspiciously clean grammar**: don't sand away all personality — deliberate fragments and comma splices are fine if the natural voice uses them.

### Significance inflation
"marking a pivotal moment in the evolution of..." inflates routine events. If the sentence still works after deleting the inflation clause, delete it.

### Aphorism formulas
Slot-fill profundity ("X is the language of Y," "the architecture of trust") turns an ordinary claim into something that sounds quotable without adding precision. Replace with the concrete claim it gestures at. Quotations and established idioms are exempt.

### Generic future-narrative closers
Modal + "become" + "one of the most [adjective]" + narrative/trend/chapter — grammatically a prediction, contains no testable content. Pick the falsifiable version or cut.

### Hedge-stacked predictions
"could potentially create," "may eventually unlock" — pick one word, not both.

### "Real/actual" adjective inflation
`real`/`actual`/`genuine`/`true` as an empty intensifier on an abstract noun implies the rest of the field is fake, without naming what makes this instance real. Carve-out: a named contrast ("real settlement, not bridged IOUs") is honest writing.

### Moral-adjective category errors
Moral adjectives (`honest`, `genuine`, `faithful`) glued onto non-agentic nouns ("an honest shape") — category error. State the concrete property instead. Related: "the assumption stops being true" (assumptions degrade, don't flip) and gratuitous universal quantifiers ("every," "taught in every course").

### Hashtag stuffing
6+ hashtags on one post is near-universal in LLM social content. Exclude issue/PR refs, hex colors, code fences, URL fragments. Fix: 2-3 specific tags or none.

### Bullet lists of bare noun phrases
5+ consecutive short (≤6 word) adjective+noun items, no verb, all the same shape — reads as a marketing one-pager. Doesn't apply to genuine list content (changelogs, parameter docs, ingredient lists).

### Copula avoidance
"serves as/features/boasts/presents/represents" instead of plain "is/has." Default to the plain verb unless a more specific one genuinely adds meaning.

### Subjectless fragments and agentless passives
"No configuration file needed." / "The results are preserved automatically." — name the actor when it clarifies. Carve-out: README feature lists, changelog entries, commit subjects.

### Synonym cycling
Rotating "developers...engineers...practitioners...builders" in one paragraph reads as thesaurus abuse. Repeat the clearest word.

### Vague attributions
"Experts believe," "Studies show" without naming the source — cite specifically or state the claim directly.

### Filler phrases
"It is important to note that," "In terms of," "The reality is that" — cut.

### Generic conclusions
"The future looks bright," "Only time will tell," "As we move forward" — cut, or make the closing thought specific.

### Chatbot artifacts
"I hope this helps!", "Certainly!", "Feel free to reach out," "Let's dive in!" — remove entirely; these are chat-interface tics, not writing.

### "Let's" constructions
"Let's explore/break this down" as a false-collaborative transition — just start with the point.

### Notability name-dropping
Piling on prestigious citations to manufacture credibility — one specific reference with context beats four name-drops. Related: historical analogy stacking (rapid-fire lists of past technologies to borrow their weight).

### Vague third-party validation
Unnamed authority + generic superlative ("independent testing confirms," "analysts agree") — the claim is unfalsifiable because the reader can't check who measured what. Named, checkable validation (a linked report, a dated audit) is legitimate and unflagged — the tell is vagueness, not the act of citing.

### Superficial -ing analyses
Strings of present participles as pseudo-analysis ("symbolizing...reflecting...showcasing") say nothing. Same move without -ing: "this represents a broader shift." Replace with specific facts or cut.

### Promotional language
Tourism-brochure prose ("nestled within the breathtaking foothills," "a thriving ecosystem") — replace with plain description. If you wouldn't say it in conversation, cut it.

### Formulaic challenges
"Despite challenges, [subject] continues to thrive" is a non-statement — name the actual challenge and response, or cut.

### Speculative scenario openers
"Imagine a world where..." — the hypothetical does the persuading instead of evidence. Cut and state the real claim. Fiction and genuine thought experiments are exempt.

### False ranges
"From the Big Bang to dark matter" sounds sweeping, says nothing. List the actual topics, or pick the one that matters.

### Inline-header lists / list-label periods
"**Performance:** Performance improved by..." — strip the repeating header. For label+gloss bullets, use a colon not a period (`- **Intros:** years of...`, not `- **Intros.** Years of...`).

### Title case headings
Sentence case for subheadings; Title Case only (if at all) for the piece's main title.

### Hyphenated modifier stacking / unnecessary hyphenation
Cut compound-modifier density to the one that matters. Close standard compounds ("code-base"→"codebase"). Remove attributive hyphens used adverbially ("in real-time"→"in real time," but keep "real-time analytics" before a noun). Preserve established compounds (high-quality, third-party, server-side).

### Cutoff disclaimers / speculative gap-filling
"As of my last update," "I don't have access to real-time data" — model limitations leaking into prose; find the info or remove the hedge. Speculative gap-filling hides the same gap behind plausible-sounding filler instead ("is believed to have," "likely began his career in") — worse, because the reader can't tell what's known from invented. Cut or replace with a sourced fact.

### Unfilled placeholders
`[Your Name]`, `[INSERT SOURCE]`, `2025-XX-XX` — near-definitive evidence of unedited AI boilerplate. Fill in or delete the sentence.

### Chatbot citation markup leaks / AI-tool URL parameters
`citeturn0search0`, `oai_citation`, `contentReference[oaicite:0]` — strip mechanically, these are fingerprints not patterns. Same for `utm_source=chatgpt.com`/`claude.ai`/`perplexity.ai` tracking params — strip the parameter, keep the URL if the link matters.

### Novelty inflation
"He introduced a term," "a failure mode nobody talks about" — most ideas are applications of existing concepts, not inventions. Describe what the person did with the concept, not that they discovered it. Also flag invented labels (pseudo-analytical compound terms coined and never defined).

### Infomercial engagement hooks
"The catch?", "Here's the thing.", "Plot twist:" — fake-momentum teasers. Delete the hook, state the thing. Same move in fake-candid register: "Honestly?", "Real talk:" as theatrical setup-and-reveal (mid-sentence "honestly" in casual prose is fine).

### Social endorsement closers
"This one is worth your time:", "Thank me later." — generic, demonstrative-anchored recommendation with no reason to click. Say what the thing is and who it's for instead.

### Emotional flatline
"What surprised me most," "I was fascinated to discover" — claims an emotion without earning it through the writing. If genuinely surprising, let the content show it.

### Lingering-attention claims
"The line I keep coming back to," "I can't stop thinking about this" — claims duration of attention, unfalsifiable, arrives before the reader has reason to care. Carve-out: a stated reason for the recurrence is legitimate.

### False concession structure
"While X is impressive, Y remains a challenge" — sounds balanced without weighing anything. Make the concession specific or pick a side.

### Invented contrast-pair mirroring
"False precision rather than genuine accuracy" — one half is a real term of art, the other a fabricated mirror for parallelism. Reach for an actual opposite or drop the contrast structure.

### Rhetorical question openers
"But what does this mean for developers?" as a stalling transition — if you know the answer, just say it.

### Parenthetical hedging
"(and, increasingly, Z)" — if the aside matters, give it its own sentence; if not, cut it.

### Numbered list inflation
"Five things to know" padded to hit a number — only use numbered lists for genuinely that many discrete, parallel items.

### Reasoning chain artifacts
"Let me think step by step," "Breaking this down," "Step 1:" — chain-of-thought scaffolding leaking into published prose. State the conclusion, then the evidence.

### Sycophantic tone
"Great question!", "You're absolutely right!" — conversational rewards from chat interfaces, remove entirely.

### Narrated candor
Announcing disclosure instead of disclosing: "Two caveats I would rather flag than let you discover later:" — the content is "Two caveats:"; the rest advertises forthrightness. Deletion test: cut the frame, if no information is lost it was never content. Carve-outs: the disclosure itself (substantive admissions) and conflict-of-interest labels ("in the interest of full disclosure, I own shares...") both stay.

### Acknowledgment loops
"You're asking about," "To answer your question," restating the prompt before answering — just answer.

### Confidence calibration phrases
"It's worth noting that," "Interestingly," "Certainly" — signals how the reader should feel instead of letting the fact speak. One per 2,000 words is fine; three in 500 is emphasis stacking. Related: persuasive-authority tropes ("the real question is," "at its core," "make no mistake") — assert depth instead of showing it.

### Self-labeling significance
"That last move is the contrarian one" — the label does the work the content should. Cut it and let the explanation carry the weight, or reposition the item so it doesn't need the label.

### Wall-of-text replies
Reply-length text (under ~150 words, 4+ sentences) with zero line breaks, in conversational registers (issue/PR comments, chat, DMs). Break at thought boundaries. Doesn't apply to continuous long-form prose (a blog intro is correctly one dense paragraph).

### Recap-flattery opener
Replying by summarizing the other person's own work back at them with praise before the point — performs appreciation instead of conveying information. Substance first; if thanks is warranted, one plain clause.

### Excessive structure
More than 3 headings in under 300 words; 8+ bullets in under 200 words; formulaic section headers ("Overview," "Key Points"); fragmented headers (a heading followed by a one-line restatement before real content starts).

### Diff-anchored writing
Documentation narrating a change instead of describing the thing as it is ("This function was added to replace..."). Describe current behavior; history belongs in the changelog. Changelogs/release notes/decision records are correctly version-scoped and exempt.

### Manufactured punchlines and staccato drama
Three or more same-shape clipped fragments in a row, each posing as a reveal. Keep the one that earns emphasis, fold the rest into ordinary sentences.

### Rhythm and uniformity
Structure is the #1 detection signal — weighted higher than vocabulary by tools like Pangram. Vary sentence length (mix 3-8 word punches with 20+ word flowing sentences); vary paragraph length; repeat the right word rather than cycling synonyms; use first person where the piece is supposed to have a voice. **Over-polishing** (sanding away all natural irregularity) pushes writing *toward* an AI statistical profile — don't apply every rule at maximum strictness.

### Vocabulary diversity (stylometric)
Type-token ratio below ~0.40 on 200+ word general prose is worth a second look (human prose typically runs 0.50-0.65). Narrow technical topics and second-language writing legitimately compress this. Fix: broaden the *what* — name specific things — not thesaurus the existing words.

### Paragraph-reshuffle immunity / treadmill effect (writer-side tests, not regexes)
Can you swap two body paragraphs without breaking the piece? If order doesn't matter, it's a list of points, not an argument. For each paragraph, ask "what's actually new here?" — if you could cut 40-60% and lose no information, cut it.

### When to rewrite from scratch vs. patch
5+ flagged vocabulary hits across categories, 3+ distinct pattern categories, and uniform rhythm together mean the structure itself needs rebuilding — state the core point in one sentence and rebuild from there, rather than patching individual phrases.

## Severity tiers
- **P0 (fix immediately)**: cutoff disclaimers, chatbot artifacts, unsourced vague attributions, significance inflation on routine events, hashtag stuffing on social/investor profiles.
- **P1 (fix before publishing)**: word-list violations, template phrases, "let's" openers, synonym cycling, formulaic openings, bold overuse, em-dash frequency, future-narrative closers, social endorsement closers, lingering-attention claims, narrated candor, hedge-stacked predictions, real/actual inflation, moral-adjective errors, invented contrast-pair mirroring, bare-noun-phrase bullet lists, Tier 3 phrase clustering.
- **P2 (fix when time allows)**: generic conclusions, rule of three, uniform paragraph length, copula avoidance, transition phrases, hashtag stuffing on blog profiles, Tier 3 phrase repetition, unnecessary hyphenation.

## Self-reference escape hatch
When writing *about* AI writing patterns, quoted examples are exempt from flagging — only flag patterns in the author's own prose, never in cited examples of bad writing.

## Context profiles
Optional strictness hint. Auto-detect when unspecified: short + hashtags → `linkedin`; code blocks/architecture → `technical-blog`; salutation + fundraising language → `investor-email`; step-by-step/parameter docs → `docs`; no strong signal → `blog` (default, full strength).

| Rule | linkedin | blog | technical-blog | investor-email | docs | casual |
|---|---|---|---|---|---|---|
| Em dashes | relaxed | strict | strict | strict | relaxed | skip |
| Bold overuse | relaxed | strict | strict | strict | relaxed | skip |
| Emoji in headers | relaxed | strict | strict | strict | skip | skip |
| Excessive bullets | skip | strict | relaxed | strict | skip | skip |
| Hedging | strict | strict | relaxed | strict | relaxed | skip |
| Word table (full list) | strict | strict | partial (see below) | strict | relaxed | P0 only |
| Promotional language | relaxed | strict | strict | extra strict | strict | skip |
| Significance inflation | strict | strict | strict | extra strict | relaxed | skip |
| Copula avoidance | skip | strict | relaxed | strict | skip | skip |
| Uniform paragraph length | skip | strict | strict | strict | relaxed | skip |
| Numbered list inflation | relaxed | strict | relaxed | strict | skip | skip |
| Rhetorical questions | relaxed | strict | strict | strict | strict | skip |
| Transition phrases | skip | strict | strict | strict | relaxed | skip |
| Generic conclusions | skip | strict | strict | extra strict | skip | skip |
| Hashtag stuffing | strict | strict | strict | extra strict | skip | skip |
| Bullet-NP lists | strict | strict | relaxed | strict | relaxed | skip |
| Tier 3 phrase clustering | strict | strict | strict | extra strict | relaxed | skip |
| Future-narrative closers | strict | strict | strict | extra strict | skip | skip |
| Social endorsement closers | strict | strict | strict | strict | skip | relaxed |
| Hedge-stacked predictions | strict | strict | relaxed | extra strict | relaxed | skip |
| Real/actual inflation | strict | strict | strict | extra strict | relaxed | skip |
| Moral-adjective errors | strict | strict | relaxed | strict | relaxed | skip |
| Invented contrast-pair mirroring | strict | strict | relaxed | strict | relaxed | skip |
| Subjectless fragments/passives | relaxed | strict | relaxed | strict | skip | skip |

**Technical-blog word-table exceptions**: `robust`, `comprehensive`, `seamless`, `ecosystem`, `leverage` (platform/API sense), `facilitate`, `underpin`, `streamline` are unflagged in technical context. Still flag: `delve`, `tapestry`, `beacon`, `embark`, `testament to`, `game-changer`, `harness`.

"Extra strict" = flag even borderline instances. "Skip" = don't audit this category for this profile.

## Voice profiles
Independent axis from context (context = how strict; voice = how it sounds). Optional — infer from the input's existing register if unspecified, don't impose a persona on text that already has one.

- **`casual`**: contractions throughout, short sentences (≤14 words avg), fragments allowed, near-zero jargon. Blog, social, community.
- **`professional`**: active voice, varied sentence length, one concrete claim per paragraph, explicit ask, low hedging tolerance. LinkedIn, investor email, pitches.
- **`technical`**: plain copulatives over inflated substitutes, one idea per sentence, imperative for instructions, jargon fine if defined on first use. Docs, technical blog.
- **`warm`**: address the reader directly, cut intensifiers in favor of stronger verbs, no performative-empathy openers, medium sentences (15-20 words). Mentorship, onboarding.
- **`blunt`**: lead with the claim, near-zero hedging, rare em-dashes, short declaratives with occasional long-sentence contrast. Decision memos, hard feedback.

Calibrate to a writing sample when given one instead of a named profile — match sentence-length pattern, contraction rate, recurring word choices; don't "upgrade" their vocabulary. Where voice and context disagree on the same rule, resolve toward the stricter of the two.

## House style: `--style <config-or-guide>`
Copyedits to a house style on top of the de-AI pass. `--style ./house.json` applies a JSON config (`register` + `mechanics`) and verifies the checkable subset. `--style "APA"` with no config applies from general knowledge as best-effort with an explicit no-compliance-claim disclaimer — never reproduces the guide's copyrighted text. Precedence when combined with `--voice`/`--context`: mechanics beat everything, then voice, then a config's register, then context.

## Tone calibration
Writing should sound like a person wrote it — direct, specific, confidence demonstrated not asserted. Vary sentence length; be concrete (numbers, names, dates); have a voice where appropriate; take a position if the piece is supposed to; earn emphasis rather than announcing it. If the original is already strong, say so and make only necessary cuts.

### Never inject these (rewrite-mode guardrail)
Never **add**, even to "put voice back on purpose," what the source didn't contain:
- **Fake first person** — "I've seen this a hundred times" where the source had no author presence.
- **Manufactured stakes** — "the stakes have never been higher."
- **Forced contrarianism** — inventing a foil the source never argued against.
- **Performed candor** — "let's be honest," "real talk."
- **Em-dash theatrics** — dashes staged for drama the content hasn't earned.
- **Staccato conversion** — chopping ordinary sentences into fragments to manufacture rhythm.
- **Invented specifics** — a number, name, date, or mechanism the source never contained. If a concrete detail is missing, flag the gap. Never fill it.

The test for every edit: did the information in the rewrite come from the source? Subtraction and sharpening are in scope. Addition of stance, personality, or fact is not.

---

# Section B — Ctrack-specific checks

*(Original to this skill — built for Ctrack AU from the fleet-management and fleet-tracking page audits.)*

## Reference facts
Cross-check every claim against this table. Keep it updated — every audit is a chance to grow it (output format, item 5).

| Fact | Value | Source |
|---|---|---|
| Company age | 40+ years / founded 1985 | self-consistent across pages checked |
| Global subscriptions | 300,000+ | Ctrack's own copy |
| Data centres | Sydney and Melbourne, Australia | fleet-management FAQ |
| Security certification | ISO/IEC 27001 | multiple pages |
| Device certification | TCA Type-Approved (unit model: TX650) | fleet-tracking page |
| GPS update interval | 10-60 seconds | fleet-tracking page, stated 3x |
| Telematics adoption rate | 49% of large Australian fleets | sourced: AfMA |
| Fuel savings | 10-15% | industry benchmark (labelling inconsistent across pages — worth standardising) |
| Maintenance cost reduction | ~20% | unsourced everywhere it appears — treat as needing a citation |
| Idle fuel waste | 7% | sourced: EROAD Australia |
| Stolen vehicle recovery rate (AU) | 72% | sourced: NMVTRC |
| CoR penalty figure | $4.23M (NHVR, July 2026) | appears in body copy + FAQ + FAQPage schema on fleet-management — flagged for verification, not yet confirmed |

## Checks
1. **Stat sourcing** — every number: sourced / matches reference table / unsourced. Never invent a source or replacement number.
2. **Internal contradiction** — read the whole page. Check promotional claims against the page's own stated specs (caught "up-to-the-second" against a stated "10-60s"), and check claims against the reference table for drift.
3. **Duplicate elements** — badge rows and trust strips for accidental repeats; stat blocks and full sentences for near-verbatim restatement that adds nothing new (a stat once in prose + once in a skimmable tile is fine; the same sentence twice in prose is not).
4. **Heading-case consistency** — flag headings breaking with the page's dominant convention — catches template drift, not a style preference.
5. **Unnamed-authority superlatives** — "[best/leading/trusted by] [adjective] [audience]" with no name, ranking, or evidence. Check `<title>`, meta description, and JSON-LD `description` too — this pattern showed up in structured data as often as in prose.
6. **Cross-page consistency** — when multiple Ctrack pages are audited in one session, flag numeric drift in a shared claim. Wording variation is fine; number variation isn't.

---

## Output format

**Detect mode:**
1. **Section A issues** — grouped by severity (P0/P1/P2), quoted, Tier 1A/1B kept visually separate.
2. **Section B issues** — grouped by check (1-6), quoted; name both halves of any contradiction explicitly.
3. **Assessment** — clear problem vs. judgment call, for both sections.
4. **Open items carried over** — unresolved known issues from the Section B tables, if relevant to this page.
5. **Reference table updates** — new confirmed facts or newly-found unsourced claims to add for next time.

**Rewrite mode:** the four sections above, plus a full rewritten version (Section A fixes applied per the guardrails; Section B issues flagged inline or in a "needs verification" list, never invented) and a second-pass audit of the rewrite itself.

**Edit mode:** edits made (file location, before → after) and verification that flagged patterns are resolved, noting anything deliberately left alone.
