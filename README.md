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

## Origin

Built out of a working reference on documented AI writing and image "tells," compiled from PubMed/arXiv word-frequency studies and 2026 AI-image-detection field guides. The v1.1.0 corporate-email category and rule-of-three fix were both driven by a real test: a 100% AI-generated staff email scored "Moderate" under v1.0.0 but tripped only one flag (an em dash) because the original lists were built for academic/fiction prose. See `CHANGELOG.md` for details.
