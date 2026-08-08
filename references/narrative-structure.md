# Structural tells: the measured reference

Load this when you need the full numbers, the per-model fingerprints, or the nonfiction
mapping. The working rules live in SKILL.md; this file is the evidence behind them.

Source: Russell, Rajendhran, Pham, Iyyer & Wieting, "StoryScope: Investigating
idiosyncrasies in AI fiction," COLM 2026 (arXiv:2604.03136v5).

## What the study did

61,608 stories, ~4,750 words each. 10,272 writing prompts, each answered by one human
author (Books3) and five models: Claude Sonnet 4.6, GPT-5.4, Gemini 3 Flash, DeepSeek
V3.2, Kimi K2.5. Every story was scored on 304 discourse-level features (plot, agents,
time, revelation, setting, perspective, social network, events, structure, style), then
an XGBoost classifier was trained on the feature vectors and read with SHAP.

Numbers worth carrying:

- Narrative features alone separate human from AI at **93.2% macro-F1**, holding 97% of
  the performance of a model that also sees style.
- 30 "core" features carry most of it: **84.8% macro-F1** on their own.
- Style-only features reach 85.8%. So structure alone beats style alone.
- **Stylistic editing barely helps.** Running AI stories through LAMP span-level
  rewriting (removes cliché, purple prose, redundant exposition) moved detection from
  95.5% to 93.9% macro-F1. A 1.6-point drop.
- The five models cluster together and sit apart from humans. Mean human-AI centroid
  distance is 1.6x the mean AI-AI distance. The closest human-AI pair is farther apart
  than the most distant AI-AI pair.
- Human stories are rarer: mean rarity percentile 0.71 vs 0.49 (Cohen's d = 0.83). At
  the prompt level the human version is the rarest of the six **57.8%** of the time,
  against 16.7% by chance. 24.7% of human stories land in the corpus-wide rarest 10%,
  versus 7.1% of AI stories.
- Human stories are also more spread out. Mean distance to their own centroid is 22%
  greater than the average AI radius.

The reason this matters for a humanizer: the features are structural, so they survive
paraphrase. You cannot edit them out at the sentence level. You have to decide them
while drafting.

## The 30 core features, with means

Gap = Human minus AI. Negative gap = AI-elevated. `s` = 1-5 Likert mean, `o` = ordinal
mean, `→` = prevalence of that one option.

### AI-elevated: thematic over-determination

| Feature | Human | AI | Gap |
|---|---|---|---|
| Thematic explicitness and moralizing `s` | 3.28 | 3.94 | -0.65 |
| Moral / philosophical weighting `s` | 3.26 | 3.68 | -0.42 |
| Thematic unity (does everything serve one point) `s` | 4.41 | 4.74 | -0.33 |
| Narrator explicitly comments on theme → yes | 52% | 77% | -25 |
| Dialogue function → philosophical debate | 34% | 59% | -25 |
| Reference explicitness → implicit echoes | 50% | 72% | -22 |

### AI-elevated: sensory and embodied performativity

| Feature | Human | AI | Gap |
|---|---|---|---|
| Emotional expression → embodied metaphor | 38% | 81% | -42 |
| Setting as psychological mirror `s` | 3.58 | 4.07 | -0.49 |
| Environmental and ecological emphasis `s` | 2.83 | 3.21 | -0.38 |
| Sensory modalities → olfactory | 57% | 82% | -26 |
| Sensory density `s` | 3.66 | 3.93 | -0.26 |
| Depth of interior access `s` | 3.67 | 3.93 | -0.26 |

The embodied-emotion gap of 42 points is the largest single gap in the table.

### AI-elevated: structural streamlining

| Feature | Human | AI | Gap |
|---|---|---|---|
| Causal chain continuity `s` | 3.92 | 4.20 | -0.28 |
| Spatial granularity `o` | 2.27 | 2.53 | -0.26 |
| Agency in resolution → protagonist's choice | 46% | 69% | -23 |
| Character introduction → external description | 30% | 52% | -22 |
| Subplot integration → no subplots | 57% | 79% | -22 |
| Resolution mode → internal understanding | 27% | 47% | -21 |
| Pre-threat character investment `s` | 2.76 | 2.99 | -0.23 |
| Opening spatial grounding `o` | 2.12 | 2.33 | -0.20 |

### Human-elevated: intertextual richness

| Feature | Human | AI | Gap |
|---|---|---|---|
| Intertextual strategy → explicit named reference | 47% | 24% | +23 |
| Reference explicitness → balanced mix | 37% | 16% | +21 |

### Human-elevated: reader engagement

| Feature | Human | AI | Gap |
|---|---|---|---|
| Fourth-wall permeability `o` | 0.67 | 0.39 | +0.28 |
| Direct reader address `o` | 0.28 | 0.07 | +0.21 |

### Human-elevated: temporal complexity

| Feature | Human | AI | Gap |
|---|---|---|---|
| Depth of recontextualization after a surprise `s` | 3.28 | 2.95 | +0.34 |
| Chronological discontinuity `s` | 2.40 | 2.12 | +0.28 |
| Nonlinear framing for delayed disclosure `s` | 1.96 | 1.68 | +0.28 |
| Anachrony intensity (flashback / flash-forward) `s` | 2.58 | 2.31 | +0.27 |

### Human-elevated: narrative diversity

| Feature | Human | AI | Gap |
|---|---|---|---|
| Location variety scope `o` | 1.34 | 1.08 | +0.26 |
| Dialogue-to-narration proportion `s` | 2.95 | 2.70 | +0.24 |
| Subplot integration → thematically parallel | 42% | 21% | +22 |
| Moral polarity toward protagonist → ambivalent | 59% | 38% | +21 |
| Emotional expression → explicit labels | 29% | 8% | +21 |

## Per-model fingerprints

Features where one source diverges from the other five. Uniq. = uniqueness ratio against
the next-best class. Higher means more distinctive.

### Claude (26 fingerprints)

The most distinctive of the five models. 77.1% F1 on narrative features alone, second
only to human.

| Feature | SHAP | Uniq. |
|---|---|---|
| Strength of event escalation (flat) | 0.402 | 22.4 |
| Event-type diversity (low) | 0.491 | 10.7 |
| Ending temporal scope → epilogue / flash-forward | 0.096 | 8.9 |
| Dreams / visions as temporal distortion → no | 0.116 | 7.7 |
| Setting mood → uncanny / haunted | 0.059 | 4.6 |

Plus 21 more: event density, conflict modality, relationship trajectory, heteroglossia,
closure.

Flat event escalation at 22.4 is the strongest fingerprint of any model in the study. In
prose terms: Claude starts at a given intensity and stays there. Also takes a
reverent/continuist stance toward tradition (62% of Claude stories, vs 39-56% for the
others) rather than subverting it, keeps the most uniform narrative voice of any source,
favors epilogues, and avoids dream sequences. The paper's summary: "Claude keeps it cool."

### GPT (11 fingerprints)

| Feature | SHAP | Uniq. |
|---|---|---|
| Role of gossip and rumor → salient | 0.200 | 22.1 |
| Narrator temporal distance → distant retrospective | 0.119 | 6.8 |
| Reader expectation strategy → subverts | 0.098 | 3.9 |
| Iterative / habitual narration → no | 0.144 | 3.2 |
| Reconciliation → partial / ambiguous | 0.066 | 2.6 |

Gossip as plot mechanism in 64% of GPT stories vs 44-55% elsewhere. Frames events as
recollections from years or decades back. Ensemble-heavy social networks at human levels.

### Gemini (11 fingerprints)

Protagonist's social circle expands; speech reported directly; siege/ordeal schema; named
personal names; frequent flashbacks. Produces the tidiest endings and longest denouements,
and the bleakest settings (88% tagged bleak and oppressive).

### DeepSeek (7 fingerprints)

Visible narrator presence, emotion through behavioral cues, atmosphere over plot,
backstory evenly interleaved, embedded storytelling scenes. Front-loads context other
sources hold back.

### Kimi (3 fingerprints)

Character introduced in an action event, in medias res opening, no explicit trait
labeling. Fewest fingerprints and lowest attribution F1 — sits at the generic center of
the AI distribution.

### Human (32 fingerprints)

| Feature | SHAP | Uniq. |
|---|---|---|
| Character introduction → in dialogue | 0.110 | 21.4 |
| Breadth of focalization → single focal | 0.083 | 15.7 |
| Narrator address mode → no direct address | 0.069 | 12.6 |
| Overall revelation pacing → back-loaded | 0.094 | 7.7 |
| Literary ambition → crossover genre | 0.116 | 6.8 |

Note the tension with the core-feature table: humans address the reader more often overall
(28% vs 7%), yet "no direct address" is a human fingerprint against the AI classes. Both
tails are human. AI sits in the middle. That is the dispersion result showing up in a
single feature, and it is the reason you should vary rather than invert.

## Which dimensions carry the signal

Trained on one NarraBench dimension at a time (binary macro-F1):

| Dimension | Alone | Removed |
|---|---|---|
| Agents | 80.2 | -1.2 |
| Situatedness | 77.3 | -0.8 |
| Plot | 74.5 | -0.4 |
| Perspective | 71.3 | -0.6 |
| Setting | 70.6 | -0.4 |
| Social networks | 67.9 | -0.2 |
| Events | 67.8 | -0.4 |
| Revelation | 67.2 | +0.3 |
| Temporal structure | 62.0 | -0.2 |
| All narrative | 93.2 | — |

No dimension is sufficient and none is necessary. The signal is redundant across
correlated dimensions, which is why fixing one axis does not help much. Character handling
carries the most on its own, so start there.

## Mapping to nonfiction

The study measured fiction. These transfer to essays, docs, reports and posts; the rest
do not.

| Fiction feature | Nonfiction form |
|---|---|
| Thematic explicitness and moralizing | The paragraph that tells the reader what the preceding paragraph meant |
| Narratorial thematic commentary | "What this shows is...", "The takeaway here is..." |
| Thematic unity 4.74 | Every section dutifully serving the thesis; no digression, no unresolved aside |
| Causal chain continuity | Argument that runs A→B→C with no branch and no dead end |
| No subplots | One idea per piece, nothing running alongside it |
| Agency in resolution → protagonist choice | Ending on what the reader should now do |
| Resolution → internal understanding | Ending on a shift in perspective rather than a fact |
| Setting as psychological mirror | Framing metaphors that conveniently match the argument |
| Sensory density, olfactory imagery | Decorative scene-setting in an opening that has no reason to be there |
| Emotional expression → embodied | "It's frustrating when..." dressed up as a felt scene |
| Reference explicitness → implicit echoes | "Studies show", "some argue", unnamed sources |
| Pre-threat investment | Long warm-up before the actual point |
| Explicit named reference (human) | Name the paper, the tool, the version, the person |
| Direct reader address (human) | Talk to the reader; admit what you don't know |
| Chronological discontinuity (human) | Start in the middle, backfill later |
| Moral ambivalence (human) | Leave a tradeoff unresolved because it is unresolved |
| Location variety (human) | Range across domains instead of staying in one register |

## Limits worth knowing

- Fiction only, ~5,000 words per story. Short-form and technical writing were not tested.
- Features were assigned by Gemini 3 Flash, not by people. Repeatability was high
  (Krippendorff's alpha 0.90) and human-model agreement was Cohen's kappa 0.84, above the
  0.74 the two human annotators managed with each other. But it is model-scored.
- The AI stories came from reverse-engineered prompts with no style instruction. Text
  written under a strong voice constraint may sit elsewhere.
- Raw-text baselines still beat this: fine-tuned ModernBERT hit 99.9% macro-F1. Structure
  is the durable signal, not the strongest one.
- Model versions move. Claude's fingerprint here is Sonnet 4.6. Treat per-model sections
  as a worked example of what a fingerprint looks like, not a fixed fact.
