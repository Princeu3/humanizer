# AGENTS.md

Guidance for AI coding agents (Claude Code, Codex, Warp, etc.) working in this repository.

## What this repo is

A portable agent skill implemented entirely as Markdown. The runtime artifact is `SKILL.md`: the agent reads its YAML frontmatter and editor prompt. There is no build step, and the repo should avoid wording that limits support to one or two harnesses.

## Key files

- `SKILL.md` — the skill itself. Portable YAML frontmatter (`name`, `description`, `license`, `metadata.version`) followed by two layers: `STRUCTURE FIRST` (rules S1-S11, discourse-level) and the canonical numbered pattern list 1-33 with before/after examples. **This is the source of truth.**
- `references/narrative-structure.md` — loaded on demand, not at skill start. Holds the evidence behind S1-S11: the 30 core features with human/AI means, per-model fingerprints, the fiction-to-nonfiction mapping, and the study's limits. Keep its numbers and the ones quoted in `SKILL.md` in agreement.
- `README.md` — for humans: installation, usage, a summary table of the patterns, and a version history.
- `.claude-plugin/plugin.json` — optional Claude Code plugin manifest.
- `.claude-plugin/marketplace.json` — optional single-repo marketplace entry so `/plugin marketplace add blader/humanizer` works.
- `scripts/validate-package.py` — dependency-free package and synchronization checks used locally and in CI.

## The maintenance contract

`SKILL.md` and `README.md` must stay in sync. When you change behavior or content:

- **Patterns:** the skill currently defines **33 numbered style patterns** plus **11 structural rules (S1-S11)**. If you add, remove, or renumber any, update the README tables, the "N Patterns Detected" heading, and every cross-reference in the same change. Keep numbering stable unless you are deliberately renumbering. The two sets are numbered separately so `validate-package.py`, which checks for patterns 1-33, does not trip on the S-prefixed rules.
- **Line budget:** `validate-package.py` caps `SKILL.md` at 500 lines. New structural detail goes in `references/narrative-structure.md`, not the runtime prompt.
- **Version:** `SKILL.md` frontmatter stores the version under `metadata.version`, `README.md` has a "Version History" section, and `.claude-plugin/plugin.json` has a `version` field. Bump them together so package metadata matches the skill. Keep the skill version under `metadata`; a top-level `version` key is not portable across Agent Skills hosts. (`marketplace.json` intentionally omits a version so `plugin.json` stays the package source of truth.)
- **Compatibility:** keep install and usage language harness-neutral. The skill should work in any agent harness that can load Markdown skill instructions; Claude Code, OpenCode, Codex, and other harnesses are examples, not limits.
- **Validation:** run `python3 scripts/validate-package.py`, `npx skills add . --list`, and `claude plugin validate .` before publishing.
- **Non-obvious fixes:** if you change the prompt to handle a tricky failure mode (a repeated mis-edit, an unexpected tone shift), add a short note to the README version history explaining what was fixed and why.

## Editing SKILL.md

- Preserve valid YAML frontmatter (formatting and indentation).
- The prompt below the frontmatter is the product. Edit it like a careful instruction document, not code.
