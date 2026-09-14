# Changelog

All notable changes to this project are documented here and mirrored in the in-app Help & About modal.

## [2.0.0] - 2026-09-14

### Added
- Granular Diagnostic Tiers: weighted all 8 categories across Tier 1 (Stock AI & Corporate filler @ 4.0x), Tier 2 (Lexical & Hedging @ 2.0x), and Tier 3 (Rhetorical triads, Em dashes, Contrast, Markdown @ 0.5x–1.0x).
- Multi-Signal Corroboration Multiplier ($C$) and Composite AI Likelihood / Certainty percentage (0%–100%) with Low (<25%), Uncertain (25%–59%), and High (60%+) Probability bands.
- Dual-metric Results UI displaying both AI Certainty / Likelihood and Raw Pattern Density.
- Transparent calculation breakdown drawer with live mathematical formula, tier-weighted points table, and corroboration multiplier explanation.
- Comprehensive Help & About modal documentation and Declaration of Independence case-study update.

## [1.3.0] - 2026-09-14

### Added
- Interactive Scoring Rubric breakdown bar directly in the Results panel displaying the three density bands: Low (<3.0), Moderate (3.0–7.9), and High (8.0+), with real-time active band highlighting.
- In-app threshold explanation drawer explaining the mathematical formula (`[Total Flags / Word Count] * 1,000`) and the characteristics of each band.
- Comprehensive Rubric and Declaration of Independence case-study documentation in the Help & About modal explaining why historical oratory scores 14.2 (High) and how density triage differs from authorship detection.

### Changed
- Default sample text updated to the official National Archives Stone Engraving transcription of the Declaration of Independence (1,335 words).

## [1.2.0] - 2026-09-14

### Fixed
- Rule-of-three false positives on proper-noun lists (signatures, delegations, place names). Discovered by testing the tool against the Declaration of Independence: the detector fired repeatedly on entries in the signers' list like "Button Gwinnett, Lyman Hall, George Walton," a list of names, not a rhetorical triad.

### Added
- A proper-noun-run detector: sequences of 3+ consecutive Title-Case comma items (names, states, delegations) are now excluded from the rule-of-three count. Lowercase and mixed-case triads, the actual AI-style pattern (e.g. "furious, frightened, undone" or "labs, classrooms, and offices"), are unaffected.

### Verified
- No regression against the v1.1.0 corporate-email test case (still correctly flags the "labs, classrooms, and offices" list).
- Genuine historical triads that are not proper-noun Title Case throughout (e.g. "Life, Liberty and the pursuit of Happiness," where the third item is a lowercase phrase) still correctly flag.
- Full-document test: the Declaration of Independence body plus a full signers' list dropped from 35 rule-of-three false positives (pre-fix) to 18, with all remaining hits being genuine 18th-century rhetorical triads ("it is their right, it is their duty"), not name lists.

### Context
Real-world testing surfaced this: pasting the Declaration of Independence produced an unexpectedly high score. Root cause was the interaction of two known behaviors, not a single bug: (1) the canonical National Archives transcription contains literal ASCII double-hyphens ("--") in three places, a period-typical substitute for an em dash, which the tool's "Em dash / double hyphen density" category correctly detects by design; and (2) the rule-of-three detector, as documented in v1.0.0/v1.1.0's known limitations, could not distinguish a rhetorical triad from a list of proper nouns. This release fixes (2). (1) remains intentional, documented behavior.

## [1.1.0] - 2026-09-14

### Added
- "Corporate / email AI patterns" category — 9 phrases covering business-email filler ("circle back," "as we navigate," "gentle reminder," "I hope you're all having a wonderful week," etc.)
- Help & About modal: per-category explanations, methodology, source attribution, known limitations, and version/changelog panel
- Sample-text loader (loads the exact test paragraph that surfaced the v1.0.0 gaps)
- Copy-summary button
- Cmd/Ctrl+Enter keyboard shortcut to run analysis
- Score count-up animation
- Responsive layout adjustments for narrow embedded widths (Google Sites iframe)

### Fixed
- Rule-of-three detector previously required the third list item to be immediately followed by sentence-ending punctuation, so lists that continued into the rest of the sentence (e.g., "labs, classrooms, and offices running smoothly") went undetected. The pattern now matches the list itself regardless of what follows.

### Context
Triggered by a real test: a 144-word, 100% AI-generated staff email scored 6.9 flags/1,000 words ("Moderate") under v1.0.0 but tripped only the em-dash counter once — every other category returned zero. Root cause was two issues: the rule-of-three regex bug above, and a lexical/phrase list built entirely from academic and fiction-prose research with no coverage of corporate email tone. After the fix, the same sample returns 9 corporate-pattern hits plus the rule-of-three list.

## [1.0.0] - 2026-09-14

### Added
- Initial release: 7 categories (lexical overuse, stock AI phrases, em dash density, contrast-framing rhetoric, rule-of-three lists, reflexive hedging, markdown emphasis markers)
- Density scoring (flags per 1,000 words) with Low/Moderate/High bands
- Expandable per-category hit lists with highlighted context
