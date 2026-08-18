---
title: "FX Receivable Exposure and Hedge Framing"
date: 2026-08-17
author: Kathy Chau
scenario: tech-services
---

# FX Receivable Exposure and Hedge Framing

## Decision

I recommend that we look at different hedging options for our €12.5 million foreign currency receivable instead of leaving the full amount unhedged. Since the payment will eventually be converted into U.S. dollars, changes in the EUR/USD exchange rate could affect how much money the company actually receives. Hedging would help reduce some of this uncertainty and make the final USD amount more predictable.

## Exposure and Risk

Our U.S. technology services firm is expecting to receive €12.5 million in the future. The amount we receive in euros will stay the same, but its value in U.S. dollars can change depending on the exchange rate. Using the indicative forward rate of 1.0910 EUR/USD, the receivable would be worth about $13.64 million. However, if the euro weakens to an exchange rate of 1.00, the receivable would only be worth $12.5 million. This is about $1.14 million less, which shows how changes in exchange rates could have a large impact on the company's cash flow and overall profitability. The exact settlement timing was not included in the scenario information provided, so this will need to be confirmed before using final market data.

## Hedge Alternatives

There are three main hedging options that I would consider. A forward contract would allow the company to lock in an exchange rate ahead of time, giving us more certainty about the amount of U.S. dollars we will receive. The downside is that we would not benefit if the euro strengthens. A money-market hedge could also lock in the value of the receivable through borrowing and investing, but it would involve more steps and financing. Lastly, an option would protect the company if the euro weakens while still allowing us to benefit if it strengthens. However, we would have to pay a premium for this flexibility.

## Next Steps

In Phase 2, I will create the model structure and identify the assumptions, formulas, and validation checks needed to compare each alternative. In Phase 3, I will use AI to help build the model based on those specifications and then review the model for possible errors. Phase 4 will replace the placeholder information with current market data. Finally, in Phase 5, I will validate and compare the results before making a final recommendation on which hedging option would be the best choice for the company.