# Stage 2 review — model specification · Treasury sign-off

Kathy — I think you committed the wrong file, and I want to explain why I believe that before I explain the grade, because the two are connected.

`docs/specs/2026-08-17-Chau-tech-services-spec.md` is the blank template I handed out. Not "based on" it — 409 of its 415 lines are byte-for-byte identical to `_templates/template-spec.md`. The only differences are two cells in the *example* column of the inputs table:

| | Template | Your file |
|---|---|---|
| `FC_AMT` example | 10,000,000 GBP | 12,500,000 EUR |
| `F0_in` example | 1.4400 | 1.0910 |

Everything else is the shell. The title is still `# [COMPANY NAME] — FX Transaction Hedge Model · Technical Specification`. The metadata block still reads `Created by | [name]`, `Date Created | [YYYY-MM-DD]`, `Version | [0.0]`. And §1 Problem Statement is still my instruction to you —

> *"Briefly restate the exposure, timing, and objective in professional terms (3–5 sentences)."*

— with nothing written underneath it.

| Criterion | Earned |
|---|---|
| Named-range contract & tab architecture | 0 / 30 |
| Calculation flow | 0 / 30 |
| Validation & sensitivity plan | 0 / 20 |
| Reproducibility & prompt log | 0 / 20 |
| **Final** | **0 / 100** |

**Why I think this is a mix-up and not a missed assignment**

Your own `prompt-log.md` describes a specification you drafted and then corrected:

> *"The initial AI draft assumed a 90-day settlement period even though my assigned scenario did not provide an exact settlement date. I corrected the specification by identifying the settlement timing as an indicative placeholder that must be confirmed and replaced with the correct information before using live market data."*

That correction is not in the committed file — the version in your repo says `T_DAYS = 365 unless otherwise noted`, which is the template's own text, and says nothing about 90 days or about an indicative placeholder. So the spec you are describing in your log is a document that exists somewhere and did not make it into `docs/specs/`.

There is corroborating evidence too: your Stage 3 workbook has a `T_DAYS` input, a `STEP_FRAC` input, and split `BASIS_USD` / `BASIS_FC` cells. Those are choices somebody made deliberately. They did not come from nowhere.

**What to do**

Find the spec you actually wrote and commit it over the top:

```bash
git mv docs/specs/2026-08-17-Chau-tech-services-spec.md docs/specs/_template-backup.md   # optional
cp /path/to/your/real/spec.md docs/specs/2026-08-17-Chau-tech-services-spec.md
git add docs/specs/ && git commit -m "Replace template with completed Stage 2 specification" && git push
```

Reply in this thread when it is pushed and **I will re-grade this stage from scratch.** Nothing about the 0 is locked in.

If the real file is genuinely gone, rewrite it — and it will go faster than you expect, because you have already built the thing it describes. Your Stage 3 workbook *is* the specification, in Excel form. Open it next to the template and fill in each section from what you built:

- **§1 Problem Statement** — five sentences: US tech services firm, €12,500,000 receivable, settlement timing to be confirmed, objective is protecting USD value, decision sits with treasury.
- **§2 Inputs** — you already have this on your Inputs tab. `FC_AMT`, `S0_in`, `F0_in`, `R_USD`, `R_FC`, `T_DAYS`, `K_PUT`, `K_CALL`, `PREM_PUT`, `PREM_CALL`, plus your `BASIS_USD` / `BASIS_FC` split. Replace the template's GBP examples with your own values and mark which are placeholders.
- **§4 Calculation Flow** — read it off your Hedge Model tab. `Forward = FC_AMT × F0_in`. `MM = FC_AMT / (1 + R_FC·T/BASIS_FC) × S0_in × (1 + R_USD·T/BASIS_USD)`. `Put = MAX(S_T, K_PUT) × FC_AMT − FV_PREM_PUT`. Those are your formulas, in cells B6, B9 and the Sensitivity column F.
- **§4 validation** — your tolerance is already in the workbook: `ABS(MM − Forward) / Forward ≤ 0.05%`. Write the number down.
- **§7 Sensitivity Plan** — ±5% around `S0_in` in 1% steps, which is what your `STEP_FRAC` cell drives.

**A note on the mechanics, because it bit you here**

A spec is graded on what is *in the repository*, not on what you meant to put there. That sounds harsh and it is genuinely how it works everywhere: the artifact is the deliverable. Two habits that prevent this permanently —

1. **`git status` before every commit.** It would have shown you an unmodified template staged for commit.
2. **Open the file on GitHub after you push.** Ten seconds, and you would have seen `[COMPANY NAME]` at the top of the page.

**One thing that is genuinely yours in all this**

The two cells you *did* change — `FC_AMT` to 12,500,000 EUR and `F0_in` to 1.0910 — are your assigned scenario, correctly transcribed, and they are the two values that then flowed correctly into your Stage 3 workbook. You were working from the right numbers. The document just did not come with you.

— Treasury

---

### How to work this review — professional workflow

Treat this PR the way an analyst treats feedback from Treasury — a review is a proposal to engage with, not a checklist to rubber-stamp:

1. **Read it yourself first.** Understand each point and form your own view before changing anything. Disagreeing *with a documented reason* is a legitimate, senior response.
2. **Stress-test it with an LLM (pushback pass).** Paste this review and your spec into your AI assistant and ask it to (a) explain anything you're unsure of more deeply, and (b) argue the *other side* — where might the reviewer be wrong, and what would you give up by making each change. You're building judgment, not just executing edits.
3. **Decide, then draft the changes with the LLM.** For the points you accept, have the AI help implement them — you specify exactly what and why. Your spec is the prompt; precise in, correct out.
4. **Verify — non-negotiable.** Re-run your own checks (`scripts/recalc.py`, the parity tie-out, sensitivity continuity, no error cells) and confirm the numbers before you commit. An AI will hand you a confident wrong edit; verification is what makes the result *yours*.
5. **Close the loop on the PR.** Reply in the thread with what you changed, what you pushed back on and why, then commit and push. Writing down the reasoning is exactly how this works on a real team.

*This is the same human-in-the-loop discipline the whole project is built on: the LLM drafts, you edit and verify, and you own the result.*
