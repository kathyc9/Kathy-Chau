# Stage 3 Build Audit

## Finding 1 — Market Inputs

I checked the inputs used in the model and noticed that some of the market data was still based on the template. The €12.5 million receivable and 1.0910 forward rate match my assigned scenario, but the spot rate, interest rates, strike price, and option premium still need to be updated with current market data. I left these as placeholders for now because they will be updated in Stage 4.

## Finding 2 — Money-Market and Forward Check

I reviewed the validation checks on the Hedge Model tab. The forward formula and foreign-currency borrowing check both passed. However, the parity check showed a 33.29% difference between the forward and money-market hedge. This is because the current placeholder spot and interest-rate inputs do not match the assigned forward rate. I did not change the numbers just to force the check to pass because the correct market inputs still need to be sourced.

## Finding 3 — Sensitivity Analysis

I checked the sensitivity table to make sure the settlement spot rate changes from -5% to +5%. The no-hedge results change with the exchange rate, while the forward result stays constant. The option also provides downside protection while allowing the proceeds to increase when the euro strengthens. The sensitivity chart follows the same pattern as the table.

## Changes Made

I reviewed the workbook after it was generated and simplified some of the wording in the Notes section. I also made sure the assigned €12.5 million receivable and 1.0910 forward rate were included correctly. The remaining placeholder market inputs are clearly labeled so they can be replaced with sourced values in Stage 4.