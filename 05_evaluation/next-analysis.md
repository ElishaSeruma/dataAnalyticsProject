# **Remaining Analysis**

### *Why do this next?*

The regressions and forest tested how well recorded drills predict production in held-out players. The forest did not improve performance consistently. The original question also asks about drill associations and participation, so those parts still need their own analysis. I will keep the existing models and avoid further test-driven tuning.

### *Drill associations*

I will calculate Pearson correlations separately for RBs, WRs and TEs, comparing each numeric drill score with five-season scrimmage yards and touchdowns. Each pair will use only records with that score and outcome available, and show its sample size. I will not require all six drills to be recorded for a single-drill association.

Scatterplots will show whether a linear pattern is plausible and whether a few players dominate the relationship. A correlation heatmap can summarise each position. A negative correlation for a timed drill means that lower, faster times tend to accompany higher production; jump distances and bench repetitions have the opposite performance direction.

This follows Lab01_intro_to_pandas (1), cells 51 and 57, for Pearson correlation and a heatmap, and Lab02_exploratory_data_analysis (2), cell 40, for scatterplots. Cell numbers include Markdown cells.

The analysis will be descriptive and exploratory on the available production sample. It will not be used to select features or tune the predictive models. Pearson measures linear association, can be sensitive to unusual values, and does not establish causation. Many drill-position-outcome comparisons can produce apparently interesting patterns by chance, so I will not present the largest correlation as proof.

### *DNP mini-analysis*

For each drill and position, I will compare players with a recorded numeric score against players explicitly marked DNP. I will report group counts and the mean and median of yards and touchdowns. Box plots will show spread and unusual observations where the groups are large enough to interpret.

The numeric flag and score fields already distinguish these groups. Missing or unexpected entries will not be treated as DNP. A player can be in different participation groups for different drills. I will also summarise draft status and overall pick among drafted players, keeping undrafted players separate rather than assigning them pick zero.

Group summaries follow Lab02_exploratory_data_analysis (2), cells 105 and 120, and the mean and median example in Lab01_intro_to_pandas (1), cell 40. Box plots follow Analyze Data, cell 27. This is descriptive comparison, not a classifier or a formal significance test. It does not require a prediction split and will not be used to tune the regressions.

DNP does not explain why a player skipped a drill. Group differences could reflect injuries, selection decisions, draft status or other unmeasured factors. These comparisons apply only to accepted identities with qualifying production, not all Combine prospects.

### *Final evaluation*

I will combine the held-out model results, drill associations and participation comparisons to answer the research question. I will report sample sizes, errors against the baseline, inconsistent results and the effect of selecting players with observed production. No causal effects or reliable future-class predictions will be claimed from these analyses.

This document records the planned next work. The association and DNP cells are now implemented but have not been run in Colab. No findings from them are available yet.
