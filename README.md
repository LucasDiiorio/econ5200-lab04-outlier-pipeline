[README.md](https://github.com/user-attachments/files/33128632/README.md)
# Outlier Detection on California Housing

## Objective
I compared three outlier-detection methods on the California Housing data to decide which one should guide how flagged areas are handled in funding decisions.

## Methodology
- I diagnosed and fixed three bugs in an existing outlier-detection pipeline.
- I used an `OutlierDetector` class that supports three methods (modified Z-score, Tukey fences, and Isolation Forest), checks its settings before it runs, and reports a summary of what it flagged.
- I ran the modified Z-score and Tukey fences on MedInc (median income) only, since both methods look at one column at a time.
- I ran Isolation Forest on all 9 columns so it could pick up unusual combinations of values across variables.
- I compared the flagged rows from each method to find where they overlapped.
- I wrote a method-selection memo recommending which method to use and how to treat flagged rows.
- I built an interactive outlier method explorer.

## Key Findings
The three methods gave very different counts. The modified Z-score on MedInc flagged 400 rows, Tukey fences on MedInc flagged 681, and Isolation Forest on all 9 columns flagged 1032. All three agreed on 322 rows.

In my memo, I recommend Isolation Forest as the primary method, with the modified Z-score on MedInc as a secondary check. Funding decisions depend on relationships across variables, such as income compared with location, house age, and room count. Isolation Forest can catch those patterns, but Tukey fences and the Z-score only flag extreme values in a single column, so they would miss them.

Between the two single-column methods, I chose the Z-score as the secondary check because MedInc is right-skewed. Tukey's fixed 1.5×IQR rule over-flags the high end, which puts legitimate high-income areas at risk of being discarded. Using Tukey alone would over-flag those areas and still miss every multivariate anomaly.

I also concluded that "outlier" should not mean "remove." A flag can point to a data error, or to a real extreme that matters for policy, such as a high-poverty area. Dropping flagged rows automatically could erase the communities this funding is meant to find, so I recommend sending flagged rows to review instead of deleting them.
