## Purpose

This repo is a template for VibePaper-style academic writing agents. The main goal is to help the human author write and refine `paper.md` using the skills in `.agents/skills/`. There is no build/test pipeline here; the "source of truth" is the markdown/LaTeX content and the SKILL instructions.

Only add repo-specific, high-signal behavior here. For anything about paper structure or detailed workflows, defer to the SKILL docs and `writingrules.md`.

## Files That Matter

- `paper.md` – the main VibePaper markdown paper for this repo. Treat it as user-owned content.
- `writingrules.md` – canonical description of the VibePaper GUI structure and constraints.
- `.agents/skills/*/SKILL.md` – executable instructions for each skill (markdown-helper, markdown-review, latex2markdown, markdown2latex, relatedwork-finder, human-comment-helper).
- LaTeX sources (`main.tex`, `tex/*.tex`, `packages.tex`, `IEEEtran.cls`, etc.) – alternative representation of the paper; used by `latex2markdown` and `markdown2latex` workflows.
- `templates/technical_paper.md` – example template; not automatically used once `paper.md` exists.

## Core Conventions (Do Not Violate)

1. **Do not change the VibePaper structure**
   - Never rename, delete, or re-level headers `##`–`#####` in `paper.md` unless the human explicitly instructs you to redesign the outline.
   - Level 1 `#` is the paper title and may be edited; Level 6 `######` are paragraphs.
   - Treat `writingrules.md` as the source of truth for header semantics and limits.

2. **Preserve metadata comments**
   - Never delete or rewrite `<!-- description: ... -->` blocks unless asked.
   - When expanding content, follow the guidance inside each `description` comment instead of inventing your own structure.

3. **Respect length limits but don’t enforce destructively**
   - Topic sentence (Level 6 header text) should be concise (the rules say ≤50 characters; do not automatically trim user text without being asked).
   - Supporting paragraph text should be reasonably short (rules say ≤500 characters); if over, suggest splitting into multiple Level 6 paragraphs instead of silently rewriting.

4. **Keep human content authoritative**
   - Do not overwrite large existing sections in `paper.md` unless the user explicitly asks for a rewrite.
   - Prefer local, additive edits (insert new paragraphs, refine specific sentences, add comments) over full-section regeneration.

## Using the Skills Correctly

Always prefer using the SKILL instructions instead of reinventing workflows.

- **markdown-helper**
  - Use when the user asks to "write" or "improve" `paper.md` content.
  - First read `writingrules.md` and `paper.md`, then follow the SKILL’s rules: do not touch Levels 2–5 headings; add content via Level 6 paragraphs under the right sections.

- **markdown-review**
  - Use when the user asks to review or check the quality of `paper.md`.
  - Output should be "AI Suggestions:" style comments as specified in the SKILL, not silent content changes.

- **latex2markdown** / **markdown2latex**
  - `latex2markdown`: only use when explicitly asked to import from LaTeX (e.g., `main.tex`, `tex/*.tex`). Do not run it opportunistically.
  - `markdown2latex`: only generate LaTeX when requested (full paper or specific sections). Output pure LaTeX as instructed; do not mix markdown in the LaTeX files.
  - Never try to keep LaTeX and `paper.md` "in sync" automatically; follow the user’s direction about which is primary.

- **relatedwork-finder**
  - Use only when the user asks to find related work.
  - It expects to create/update a `relatedwork/` folder with `paper_list.bib`, per-paper `.md` summaries, and a `summary.md`. Do not move or rename these without reason.

- **human-comment-helper**
  - Use when the user wants to add structured reviewer-style comments.
  - Comments must follow the exact HTML comment block format in the SKILL; never present synthetic examples as real data.

## Editing Guidelines for Agents

- Prefer minimal, targeted edits using `apply_patch`.
- Do not introduce new skills or new top-level config files unless the user asks for them.
- This repo may be used as a template; avoid hard-coding anything specific to this particular paper (titles, problem domains) into shared instructions or SKILL files without confirmation.

## Running / Tooling Notes

- There is no package manager or test suite in this repo; do not assume `npm`, `pytest`, etc.
- Treat this as a content repo: verification is by inspection of `paper.md`/LaTeX and conformance to `writingrules.md` and SKILL instructions.
