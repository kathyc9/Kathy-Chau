---
title: "FX Receivable Exposure and Hedge Framing"
date: 2026-08-17
author: Kathy Chau
scenario: tech-services
---

# FX Receivable Exposure and Hedge Framing

## Decision

I recommend that we evaluate hedging alternatives for our €12.5 million foreign-currency receivable rather than leave the full exposure unhedged. Because the payment will ultimately be converted into U.S. dollars, changes in the EUR/USD exchange rate could materially affect the amount of USD the firm receives.

## Exposure and Risk

Our U.S. technology services firm expects to receive €12.5 million in a future settlement. The euro amount is fixed, but its U.S. dollar value is not. Using the indicative forward rate of 1.0910 EUR/USD, the receivable is approximately $13.64 million. If the euro weakens before settlement, however, the dollar value of the receivable will fall. For example, at an exchange rate of 1.00, the receivable would be worth only $12.5 million, roughly $1.14 million less than at 1.0910. This uncertainty could affect cash flow, budgeting, and profitability.

## Hedge Alternatives

Three hedge families should be evaluated. A forward contract would lock in an exchange rate and provide certainty about the future USD proceeds, but the firm would give up the benefit of favorable exchange-rate movements. A money-market hedge could create a similar locked-in result through borrowing and investing, but it requires financing transactions and may be more operationally complex. An option could protect the firm against an unfavorable decline in the euro while preserving upside if the euro strengthens, although this flexibility requires paying an option premium.

## Next Steps

In Phase 2, I will specify the model structure, assumptions, formulas, and validation checks needed to compare these alternatives. Phase 3 will use AI to build the model from that specification, followed by an audit of its calculations and structure. Phase 4 will replace placeholder assumptions with sourced, current market data. Finally, Phase 5 will independently validate the results and compare the alternatives before making a final hedge recommendation. This process will give management a transparent basis for deciding whether and how to hedge the receivable.