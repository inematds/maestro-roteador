# maestro-roteador

> English version of [README.md](README.md) (Portuguese is the primary document). Skill translation: [skills/maestro-roteador/SKILL.en.md](skills/maestro-roteador/SKILL.en.md).

![Skill guides, orchestrator decides, agent executes](guia/assets/hero-maestro.jpg)

A **model-and-effort triage** skill for Claude Code: it takes a raw problem and decides which model (haiku / sonnet / opus / fable) and which reasoning effort (low → max) to use for each part of the work, before dispatching subagents or workflows.

**Full guide (landing + step-by-step, in Portuguese):** https://inematds.github.io/maestro-roteador/guia/
**Beginner FAQ with analogies and a step-by-step (Portuguese):** [docs/guia-explicativo.md](docs/guia-explicativo.md)

---

## Why this skill exists

With no rule at all, agents collapse every task into "sonnet + medium" — the generic middle ground. That gets it wrong in both directions at once:

- **Overpays** on mechanical tasks (renaming 80 files needs neither sonnet nor medium);
- **Underpays** on tasks with real risk (a production migration needs more verification, not "medium but careful") and on taste-driven tasks (brand-voice copy needs repertoire, which sonnet doesn't have).

The skill replaces the middle ground with **two independent questions**, answered separately.

## The core idea: two axes that don't convert

| Axis | The question it answers | What it buys |
|---|---|---|
| **Model** | What level of repertoire/judgment does the **hardest step** of the task demand? | Repertoire (taste, subtlety, judgment with no answer key) |
| **Effort** | How much deliberation and verification do **ambiguity + cost of error** pay for? | Deliberation (thinking ahead, comparing paths, checking one's own work) |

**Effort buys deliberation, never repertoire. Model buys repertoire, never verification.** That's why "crossed" combinations are legitimate and frequent:

- `fable + low` → a script in your brand voice: the hard part is **taste** (model axis), but the decision is straightforward (low effort).
- `sonnet + high` → a migration with production at risk: the required capability is **standard** (model axis), but the error is expensive — the risk demands **verification**, not intelligence.

Whoever picks a "balanced sonnet+medium" is dodging both questions, not answering them.

## The procedure (always in this order)

1. **Isolate the hardest step.** Of the whole task, which step demands the finest judgment?
2. **Model = the SMALLEST one that handles that hardest step well.**
   - Mechanical with a clear answer key (rename, extract, format) → `haiku`
   - Standard implementation, verifiably right-or-wrong → `sonnet`
   - Judgment with no answer key (brand voice, architecture, obscure diagnosis) → `opus`/`fable`
3. **Effort = ambiguity + cost of error.**
   - Single solution + cheap error → `low`
   - A choice between paths OR an error annoying to fix → `medium`
   - Several plausible paths OR an expensive error (production, lost data, publication) → `high`+
   - **Named risk = high, minimum.** If you yourself wrote "production at risk" in the justification, "medium but careful" does not exist.
   - **Evidence ladder (cheap errors only):** if the error is reversible and immediately verifiable, start at the lowest plausible effort and **only climb with evidence** of an insufficient result — never "high just in case". The ladder does NOT apply to irreversible risk: there, the "evidence of failure" would be the damage itself.
4. **Volume:** N identical items → test 1 item on the smallest model; if it passes, the whole batch goes to it.

![Router flow: request → analysis → the right agent → result](guia/assets/roteador-agentes.jpg)

## Quick table

| Task | Model | Effort | Why |
|---|---|---|---|
| Batch rename/extract/format | haiku | low | Clear answer key, cheap error |
| Clear feature, localized refactor | sonnet | medium | Standard, verifiable |
| Data migration with production at risk | sonnet | high | Standard capability; the risk demands verification, not intelligence |
| Script/copy with brand voice | fable | low | Hardest step is taste (repertoire); the decision is straightforward |
| Mysterious bug, architecture decision | fable | high | Hard AND ambiguous |

## Traps — each one explained

Rationalizations seen in real testing, and why they're wrong:

1. **"sonnet+medium balances cost and quality."** The middle ground is dodging both questions. Answer each axis; the result is almost never the center of the matrix.
2. **"A short script doesn't need heavy reasoning" → downgrades the model.** Axis confusion: not needing *deliberation* justifies low effort — it does not justify a smaller model when the hardest step is brand judgment. The answer is `fable+low`.
3. **"Medium is enough if I'm careful" (with production at risk).** You named the risk yourself. Care is exactly what effort buys — named risk = high.
4. **"I'll take the bigger model just in case."** If the hardest step is within the smaller model's capability, the bigger one delivers the same while charging more. Insurance is bought with effort/verification, not with model.
5. **"Small model + max effort comes out cheap."** Worst possible combination: you pay for deliberation in someone who lacks the repertoire to use it, and the reasoning tokens pile up.
6. **"I'll raise effort so it comes out prettier/more polished."** Effort buys deliberation, not taste. Finish is a **model**-axis matter (see "taste tie" below). Measured in a real test with 12 effort levels across 2 providers: from high to max, the difference was a favicon — at 2–5× the tokens.
7. **"Extra effort doesn't help, but it doesn't hurt either."** It hurts — and overthinking is the **excess of the effort axis itself**, not a model defect. Effort buys deliberation; when deliberation is left over on a simple task, the model re-explores already-settled paths and over-engineers the solution. The result can come out **worse**, not just more expensive — like the student who reviews the exam so much they swap the right answer for the wrong one. The two errors of the axis are symmetric: too little effort on an ambiguous task fails for lack of verification; too much effort on a simple task fails from noise. The right effort is the **smallest that covers the risk**.

## Anti-overhead: triage has a cost too

- **A task cheaper than the triage itself gets no triage** — "fix this typo" goes straight to the turn's default, with no dispatch YAML.
- **Triage runs inline in the main turn, never in a subagent** — dispatching an agent just to decide model/effort costs more than the decision is worth.
- **Fragmenting has a cost too:** only dispatch a part to a subagent if its work outweighs the spawn overhead.

## Taste tie → the choice belongs to the user (cost × quality)

**Correctness is non-negotiable; efficiency is.** When the model choice depends only on **finish** (copy quality, visual polish) and not on correctness, the decision is a budget one — and the budget belongs to the user. The triage presents the pair and does not decide alone:

```yaml
options:
  cost: sonnet+low         # correct structure, functional finish
  quality: fable+low       # same structure, superior copy/polish
decides: user
```

Tie signal: a template/skill has already guaranteed the structure (correctness comes from the answer key) and all that's left for the big model is voice/finish. **Never** offer the pair when the risk is correctness — there, the model/effort that guarantees correctness is the minimum, not a choice.

## Cache: the hidden cost of switching models

Anthropic's prompt cache is **per model** and depends on an identical context prefix (`tools → system → messages`). Reading from cache costs ~0.1× the input price; writing costs 1.25× (5-min TTL) or 2× (1-h TTL). Losing the cache of a 100k-token context makes the next request **~12.5× more expensive**. Three rules in the triage:

1. **Switch the main turn's `/model` only at a work boundary** (end of phase, handoff) — never mid-block. On a large context, the switch can cost more than the smaller model saves. Don't alternate `opus → sonnet → opus` within the same block.
2. **Subagents do NOT pay this cost.** Each Agent/Workflow has its own context: routing a part to a smaller model via subagent doesn't touch the main cache. It's the preferred way to apply the matrix.
3. **Never an artificial keepalive** ("still there?") to hold the cache — Claude Code manages the cache on its own and the ping costs more than it saves. Long pause? Lean handoff and let it expire.

Cache-preserving routine: continuous work blocks, a stable context prefix (don't toggle tools and MCPs mid-block), related tasks concentrated in the same session.

## Harness > effort: where results actually come from

The model is a brain in a jar — tools, files, terminal, skills and instructions (the **harness**) are its limbs. In a real test running the same task at 12 effort levels across 2 providers, the functional results were nearly identical; the higher levels added cosmetic differences (a favicon, shadows, a donut chart) at 2–5× the tokens.

The lesson: **a clear spec at low effort delivers what max effort tries to guess.** Before raising effort, improve the prompt, the definition of "done", and the available tools.

![The right agent for the right task, every time](guia/assets/roteador-futuro.jpg)

## Output format (dispatch plan)

```yaml
task: <summary>
parts:
  - what: <subtask>
    hardest_step: <which and why>
    model: haiku|sonnet|opus|fable
    effort: low|medium|high|xhigh|max
    risk: <cost of error in 1 line>
main_turn: <a /model recommendation if switching is worth it — the switch belongs to the user>
```

**Limit:** for subagents/workflows the decision is automatic (`model`/`effort` parameters of Agent/Workflow calls); for the main conversation's model the skill only recommends — the switch belongs to the user via `/model`, preferably at a work boundary (see Cache).

## Validation (skill TDD)

The skill was written against a measured baseline: without it, agents collapse every task into "sonnet + medium". With it, the three test scenarios went to the right corner of the matrix:

| Scenario | Without skill | With skill |
|---|---|---|
| Batch-rename 80 files | sonnet + medium | haiku + low |
| Script with brand voice | sonnet + medium | fable + low |
| Data migration with production at risk | sonnet + medium | sonnet + high |

## Structure

```
skills/
  maestro-roteador/
    SKILL.md       # the skill (Portuguese, operational): principle, procedure, table, traps, cache, output
    SKILL.en.md    # English translation of the skill
guia/
  index.html       # landing + usage guide (GitHub Pages, Portuguese)
  assets/          # guide images
docs/
  guia-explicativo.md  # beginner FAQ: analogies, Q&A, step-by-step (Portuguese)
README.en.md       # this file
```

## Install

```bash
git clone https://github.com/inematds/maestro-roteador.git
ln -sfn "$(pwd)/maestro-roteador/skills/maestro-roteador" ~/.claude/skills/maestro-roteador
```

Or copy the `skills/maestro-roteador/` folder into `~/.claude/skills/` (user skills) or a project's `.claude/skills/`.

## Usage

Ask for the triage directly — "triage this", "which model and effort for this?" — or just ask for the work: when dispatching subagents/workflows, the agent applies the matrix and distributes each part with the appropriate `model` and `effort`.

## Changelog

- **v1.2.3** — English versions of the skill (SKILL.en.md) and README (README.en.md).
- **v1.2.2** — overthinking trap made explicit (excess of the effort axis itself; the right effort is the smallest that covers the risk) + docs/guia-explicativo.md (educational FAQ with analogies and a step-by-step).
- **v1.2.1** — evidence ladder (cheap errors start at low and climb only with evidence), 2 new traps (polish is not effort; overthinking makes it worse), cache section (switch only at boundaries, subagents are cache-free, no keepalive); guide got an image hero + Cache and Harness sections; full educational README.
- **v1.1.1** — guia/index.html (INEMA-standard landing+guide) + anti-overhead rule in the skill.
- **v1.1.0** — taste tie offers the cost (sonnet+low) × quality (fable+low) choice.
- **v1.0.0** — maestro-roteador skill: model and effort triage (two-axis matrix).

## License

MIT — INEMA research/education project.
