# AI Tell Checker

A single-file, client-side tool that scans pasted text for documented AI-writing signals — lexical overuse, rhetorical framing, punctuation density, and structural patterns — and reports a density score for editorial triage. No data leaves the browser; there is no backend.

Live version: https://thisiscrazy.ai/ai-checker
Embedded via iframe on Google Sites.

## What it checks

| Category | Method |
|---|---|
| Lexical overuse | Word list with documented post-ChatGPT frequency spikes (delve, underscore, leverage, etc.) |
| Stock AI phrases | Boilerplate transitional phrases common in generated essay/report prose |
| Corporate / email AI patterns | Filler and softening phrases specific to AI-generated business email tone |
| Em dash / double hyphen density | Counted per 1,000 words; flagged as a secondary signal only |
| Contrast-framing rhetoric | "It was not X, it was Y" / "not just X, X" constructions |
| Rule-of-three lists | Three-item comma sequences used as a rhythmic device |
| Reflexive hedging | Softening counterpoints attached to nearly every claim |
| Markdown emphasis markers | Bold/italic markup on statements that may not need it |

Full explanation of each category, sources, and known limitations are in the in-app **Help & About** modal (the "?" button in the header).

## What it explicitly does not do

It does not determine authorship. No published method reliably proves a specific passage was AI-written once a human has reviewed or lightly edited it. This is a triage aid — it points an editor at passages worth a second look, not a verdict.

## Granular Diagnostic Measures & Signal Tiers

Categories are weighted by diagnostic precision:

| Tier | Weight | Category | Rationale |
|---|:---:|---|---|
| **Tier 1 (High Diagnostic)** | **4.0x** | Stock AI Phrases, Corporate / Email Filler | Hallmark boilerplate formulas almost never found naturally in formal vintage or academic writing. |
| **Tier 2 (Moderate Diagnostic)** | **2.0x** | Lexical Overuse (PubMed/arXiv spike words), Reflexive Hedging | Post-ChatGPT statistical vocabulary surges and neutrality avoidance markers. |
| **Tier 3 (Secondary / Structural)** | **0.5x – 1.0x** | Contrast Rhetoric (1.0x), Rule-of-three Triads (0.5x), Em Dash / Double Hyphen (0.5x), Markdown (0.5x) | Rhythmic and stylistic elements that occur naturally in classical human rhetoric and essay drafting. |

## Multi-Signal Corroboration & Composite AI Likelihood

To prevent single-signal false alarms (e.g. classical oratorical triads in historical speeches), the tool applies a **Multi-Signal Corroboration Multiplier ($C$)**:
- **Single-Signal / Isolated Stylistic Spike (0 Tier 1 markers):** $C = 0.35\times$ (dampens false alarms).
- **Multi-Category AI Saturation ($4+$ active categories):** $C = 1.30\times \text{–} 1.50\times$.

The Composite Index ($I = D_w \times C$) is mapped to an **AI Likelihood / Certainty Percentage (0% – 100%)**:

| Band | Probability Range | Meaning |
|---|---|---|
| **LOW PROBABILITY** | `< 25%` | Baseline human writing or isolated stylistic habits (e.g. Declaration of Independence at 14%). |
| **UNCERTAIN / MIXED** | `25% – 59%` | Moderate presence of AI-associated patterns; editorial review recommended. |
| **HIGH PROBABILITY** | `60% – 100%` | Multi-category saturation across high-confidence AI boilerplate and vocabulary spikes. |

## Files

- `index.html` — the entire application (HTML/CSS/vanilla JS, no build step, no dependencies)
- `CHANGELOG.md` — version history, kept in sync with the in-app changelog shown in the Help modal

## Local development

No build tooling required. Open `index.html` directly in a browser, or serve it locally:

```
python3 -m http.server 8000
```

Then visit `http://localhost:8000`.

## Deploying / embedding

The file is fully self-contained (no external font or script loads), so it can be hosted from any static file host and embedded in Google Sites via **Insert → Embed → By URL**, pointing at the hosted `index.html`. Update the hosted copy whenever this repo's `index.html` changes; there is no auto-deploy configured.

## Known limitations

- Rule-of-three and contrast-framing detection are regex heuristics, not syntactic parsing. Expect some false positives on legitimate three-item lists and negation sentences.
- The lexical and phrase lists reflect patterns documented as of late 2025 / 2026 and will go stale as specific words fall in and out of fashion. Revisit periodically rather than treating the lists as fixed.
- Text-only. There is no equivalent check for AI-generated images.

## Origin & Sources

Built out of a working reference on documented AI writing and image "tells," based on:
- [The AI Tells Nobody Agrees On (and the Ones They Do)](https://assuredinformation.blogspot.com/2026/09/the-ai-tells-nobody-agrees-on-and-ones.html) — Core reference on documented LLM lexical spikes, corporate email filler, em-dash tokenizer economics, and rhetorical tricolon overlap.
- Longitudinal PubMed & arXiv word-frequency studies (2024–2026) comparing pre/post-ChatGPT vocabulary trends.
- Tokenizer token-economy analyses (GPT-3.5 vs. GPT-4/4o em dash frequencies).
- National Archives Stone Engraving transcription of the Declaration of Independence (historical oratory baseline).
