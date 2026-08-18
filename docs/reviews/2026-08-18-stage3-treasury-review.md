# Stage 3 review — AI-assisted build + audit · Treasury sign-off

Kathy — your audit note is the strongest thing in your project so far, and Finding 2 is the reason:

> *"The parity check showed a 33.29% difference between the forward and money-market hedge… I did not change the numbers just to force the check to pass because the correct market inputs still need to be sourced."*

A validation check went red, and you left it red. That is the whole point of building one. The easy move — and I see it every semester — is to nudge an input until the cell turns green, which converts a working control into a decoration. You wrote down what failed, said why, and handed it to Stage 4. That is how a control function is supposed to behave, and it is a harder instinct to teach than any formula in this course.

| Criterion | Earned |
|---|---|
| Contract compliance | 21.2 / 50 |
| Structure & presentation | 16.2 / 25 |
| Audit note | 25 / 25 |
| **Raw total** | **62.4 / 100** |
| Floor adjustment | **+12.6** (lifted to the 75% stage floor) |
| **Final** | **75 / 100** |

**Your parity check is not broken — it is right, and it is more precise than you gave it credit for**

You attributed the 33.3% gap to "placeholder spot and interest-rate inputs [that] do not match the assigned forward rate." Correct. But run the parity forward yourself and it tells you something sharper. Covered interest parity says the forward implied by *your own inputs* is:

```
F_implied = S0 × (1 + R_USD × T/BASIS_USD) / (1 + R_FC × T/BASIS_FC)
          = 1.4600 × 1.0608 / 1.0650
          = 1.4543
```

Against your assigned `F0_in` of 1.0910, that is a **33.30% gap** — the exact number in cell `C19`. So the check is not reporting a modelling error or a rounding problem. It is reporting that your spot rate and your forward rate are quoting *different currency pairs*: 1.4600 is the template's GBP-era placeholder, and 1.0910 is your assigned EUR forward. Nothing in between them can reconcile.

That matters because it tells you exactly what Stage 4 has to fix and what it does not. You do not need to touch a single formula. Replace `S0_in` with a sourced EUR/USD spot — which will be somewhere near 1.15, not 1.46 — along with real USD and EUR one-year rates, and the parity gap should collapse to well inside your own 0.05% tolerance. If it does not, *then* you have a model problem worth hunting.

**Two things to watch when you repopulate**

1. **`K_PUT` has to move with the spot.** Your strike is 1.4600, inherited from the same placeholder. Drop the spot to ~1.15 and leave the strike at 1.46 and your put is 27% in the money on day one — it will look like free money and dominate every scenario. An at-the-money put (`K_PUT = S0_in`) is the honest default unless you deliberately choose otherwise and say so.
2. **`R_USD` 6.00% and `R_FC` 6.50% are placeholders too**, and the sign of the differential drives the whole shape of the answer. If USD rates end up *above* EUR rates, your forward will sit above spot, and the "forward premium" is compensation for the rate gap — not evidence that hedging is cheap.

**What you did well**

- **91% of your calculated cells are formulas.** Every hedge outcome derives from the Inputs tab: `B6 = Inputs!$C$4*Inputs!$C$6`, `B7 = Inputs!$C$4/B5`, `B9 = B8*B4`. Change one input and the whole workbook moves. That is the difference between a model and a picture of a model, and a real number of submissions failed it.
- **You future-valued the put premium.** `B11 = −B10*B4` carries the $187,500 premium forward at the USD rate to $198,906.25 before netting. The premium is paid at inception and the proceeds arrive at settlement; comparing them undiscounted is the single most commonly botched line in this project, and you got it right.
- **Your put payoff is correctly specified.** `MAX(S_T, K_PUT) × FC_AMT + FV_PREM_PUT` — floor below the strike, full participation above it, premium netted. Exactly right.
- **You split the day-count basis.** `BASIS_USD = 360`, `BASIS_FC = 365`, applied to the correct legs. The template exposes a single `BASIS` and flags the split as a rigorous-build improvement; you implemented it. Small thing, real thing.
- **Your sensitivity grid is driven, not typed.** `STEP_FRAC = 0.01` feeds a ±5% grid, and the money-market column is flat across every scenario — which is correct: a money-market hedge locks the USD amount, so it *should* be a horizontal line.

**What cost the points — one item is most of it**

**Zero named ranges.** This is worth roughly 29 of the 37.6 points you lost, and it is the most recoverable thing in your repo. Your formulas address cells (`Inputs!$C$7`) where the contract calls for names (`R_USD`). Compare:

```
=1+Inputs!$C$7*Inputs!$C$13/Inputs!$B$16     ← what you have
=1+R_USD*T_DAYS/BASIS_USD                    ← what the spec asks for
```

The second one is readable by someone who has never opened your workbook, survives a row insertion, and — the reason the whole cohort is on the same ten names — makes every model in the class interchangeable. To fix it: select `Inputs!C4`, type `FC_AMT` into the Name Box left of the formula bar, press Enter. Repeat for all ten. Then rewrite the Hedge Model and Sensitivity formulas to use them.

The ten names are `FC_AMT`, `S0_in`, `F0_in`, `R_USD`, `R_FC`, `T_DAYS`, `K_PUT`, `K_CALL`, `PREM_PUT`, `PREM_CALL`. Keep `BASIS_USD`, `BASIS_FC` and `STEP_FRAC` as names too — you have already earned them.

**No Legend/Key tab.** You have Cover, Inputs, Hedge Model, Sensitivity and Notes. The missing one documents the colour convention: which cells are inputs a reader may change, which are formulas they must not. You are already using three fill colours — the legend is what makes them mean something.

**Repo hygiene — five minutes, and it is not really about the points**

- **`.DS_Store` is committed twice** (root and `docs/`), and your history has six commits that do nothing but touch it. Add a `.gitignore` with `.DS_Store` and remove them:

```bash
printf '.DS_Store\n' > .gitignore
git rm --cached .DS_Store docs/.DS_Store
git add .gitignore && git commit -m "Ignore .DS_Store" && git push
```

- **No `LICENSE`.** MIT at the repo root; GitHub adds it for you under **Add file → Create new file → type `LICENSE`**.
- **`docs/README.md` is missing** while every other directory has one.

**Where this leaves you**

Stage 3 is a working model with a working control on top of it, held back by a naming convention and a missing tab. Both are mechanical. The named ranges are worth doing before Stage 4 rather than after — you are about to change every input in the workbook, and doing that against names instead of cell addresses is materially less error-prone.

Read the **Stage 2** PR next if you have not. Your specification and this workbook belong to each other, and right now only one of them is in the repository.

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
