# Sources and design provenance

This package contains newly written guidance based on user requirements and
generalized research engineering lessons. It contains no private conversation
transcripts, project artifacts, checkpoints, credentials, or machine paths.

References consulted on 2026-10-07:

- OpenAI, [Rethinking skills and prompts for GPT-6 Astra](https://developers.openai.com/blog/rethinking-skills-and-prompts-for-gpt-6-astra):
  precise short descriptions, progressive disclosure, decision boundaries,
  proportional instructions, and completing authorized work.
- OpenAI, [Package your plugin](https://developers.openai.com/plugins/build/plugins):
  portable manifest, Codex compatibility overlay, and marketplace layout.
- Anthropic, [Publish plugins](https://code.claude.com/docs/en/plugins/publish),
  [manifest reference](https://code.claude.com/docs/en/plugins/manifest-reference),
  and [marketplace reference](https://code.claude.com/docs/en/plugins/marketplace-reference):
  Claude Code discovery and a marketplace pointing to the same root plugin.
- [sidiangongyuan/codex-skills-library](https://github.com/sidiangongyuan/codex-skills-library),
  especially `experiment-planner`, `paper-review-panel`, and `paper-visual-craft`:
  pilot-first research, comparison contracts, impact-based checks, and evidence
  interpretation. Those files declare MIT licensing. This package re-expresses
  selected workflow ideas rather than vendoring their skill text or installer.
  Inspected revision: `41f5a211b1a8d210023f51f9d56088311a3dae79`.

Vendored on 2026-10-08:

- [blader/humanizer](https://github.com/blader/humanizer) by Siqi Chen, MIT:
  `skills/humanizer/` vendors a modified copy of that skill, which distils
  Wikipedia's ["Signs of AI writing"](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing)
  (WikiProject AI Cleanup) and reviews of AI-generated text. This package adds
  pattern §9 (colon-led elaboration on repeat) observed during manuscript
  editing, and renumbers the following patterns accordingly; the upstream MIT
  licence text is preserved at `skills/humanizer/LICENSE`. The catalog text now
  lives at `skills/humanizer/references/general-track.md`, unchanged at v3.2.0;
  base version v3.1.0.

Vendored on 2026-10-31:

- [AIScientists-Dev/academic-humanizer](https://github.com/AIScientists-Dev/academic-humanizer)
  v0.3.3, MIT: `skills/humanizer/references/academic-track.md` vendors its skill
  text verbatim with the frontmatter removed. It contributes the academic track:
  academic AI tells, the scholarly constructs to preserve, claim-to-evidence
  discipline, voice and venue matching, and NSF/NIH proposal mode. The upstream
  MIT licence text is preserved at `skills/humanizer/LICENSE-academic-humanizer`.
  `skills/humanizer/SKILL.md` is rewritten around this package's own track
  routing and precedence rules; it is not a copy of either upstream.

The exploratory single-seed policy, later multi-seed confirmation, economical
training budgets, asynchronous job management, and limited defensive checks
are explicit design choices of this package. They are not claims that OpenAI
prescribes those research policies. This project is independent and is not
endorsed by OpenAI, Anthropic, or the referenced skill library. Its workflows are
model-neutral; the Astra article informs instruction design, not model selection.
