# Stage 1 review — tech services · Treasury sign-off

Kathy — this is a clean framing memo, and the thing I want to lead with is the sentence most people in this cohort did not write:

> *"The exact settlement timing was not included in the scenario information provided, so this will need to be confirmed before using final market data."*

You were handed an incomplete brief and you said so, in the document, rather than quietly inventing a date to make the memo read smoothly. That instinct — name the gap, carry it forward, resolve it before it touches live data — is the single most valuable habit in this whole project, and it shows up again in your Stage 3 audit. Keep doing it.

| Criterion | Earned |
|---|---|
| Exposure framing | 25 / 25 |
| Hedge families & trade-offs | 25 / 25 |
| Next steps | 25 / 25 |
| Professionalism | 25 / 25 |
| **Final** | **100 / 100** |

**What you did well**

- **You sized the risk instead of describing it.** "$13.64 million … if the euro weakens to an exchange rate of 1.00, the receivable would only be worth $12.5 million. This is about $1.14 million less." A CFO can act on a number. €12.5M × 1.0910 = $13,637,500, and $13.6375M − $12.5M = $1.1375M — both tie out.
- **All three hedge families, each with its own cost.** Forward buys certainty and gives up the upside; money market replicates it but adds borrowing and steps; the option keeps the upside but you pay for it. One sentence each, no padding, and every one of them names the trade-off rather than just the mechanism.
- **Your next steps are a real plan.** Stage 2 specifies, Stage 3 builds and audits, Stage 4 replaces placeholders with sourced data, Stage 5 validates and recommends. You understood the arc of the project before you started it.
- **Frontmatter, canonical path, 423 words.** `docs/decisions/2026-08-17-Chau-tech-services-hedge-framing.md` is exactly where a reviewer looks. That sounds trivial until you see how much time gets lost on projects where it isn't true.

**Two things to sharpen**

**1. The quote convention is backwards in one place.** You write "the indicative forward rate of 1.0910 EUR/USD." 1.0910 is *USD per EUR* — dollars you receive for one euro — so it is USD/EUR. Your arithmetic uses it correctly, which is what matters here, but in FX the convention *is* the substance: a memo that inverts the pair is a memo whose reader has to stop and re-derive which direction "the euro weakens" points. State it once at the top ("all rates quoted USD per EUR; a higher quote means a stronger euro") and then you never have to think about it again.

**2. Your downside scenario is a round number, not an argument.** "If the euro weakens to an exchange rate of 1.00" gives a clean $1.14M figure, but 1.00 was chosen because it is tidy. Anchor the scenario to something defensible instead — a percentage move (−5%, the grid you go on to build in Stage 3), a recent trading range, or an implied-volatility band. The question a CFO asks next is always *"how likely is that?"*, and a round number has no answer to it.

**One thing to carry forward**

Your memo commits to "look at different hedging options … instead of leaving the full amount unhedged," which is the right posture for Stage 1 — you should not pick the winner before you have built the model. Just make sure Stage 5 actually closes it. The memo that opens a question has to be the memo that answers it.

**Where the rest of your project stands**

Stages 2 and 3 are reviewed in separate PRs. Read the **Stage 2** one next — there is a file problem there that is almost certainly a two-minute fix, and it is worth 4.2 points of your semester grade.

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
