# **Initial Modelling Results**

### *What ran?*

The saved notebook in commit `2ebcddd` contains single-drill and six-drill linear regressions for yards and touchdowns, separately by position. No saved execution errors were found. The split was 80/20 with seed 42.

| Position | Training players | Test players | Complete-case training | Complete-case test |
|---|---:|---:|---:|---:|
| RB | 167 | 42 | 84 | 18 |
| WR | 252 | 64 | 121 | 32 |
| TE | 100 | 26 | 57 | 11 |

The complete-case comparison contains only players with all six scores. Its models and training-mean baseline use identical players. Individual-drill models can use more players, so comparisons across those separate samples need care.

### *What can I say from the saved output?*

The RB dash regression for scrimmage yards had test **R² = 0.020**, **RMSE about 1,921 yards**, and **MAE about 1,554 yards**, on 40 test players. The corresponding training-mean baseline had RMSE about 1,941 yards and MAE about 1,568 yards. This is a small improvement over the baseline and limited predictive power for that example.

The TE six-drill touchdown model had test **R² = 0.071**, **RMSE about 7.61 touchdowns**, and **MAE about 5.86 touchdowns**, on 11 test players. The small test sample makes the score sensitive to individual outcomes.

These are visible examples, not a ranking of all models. The saved results tables were truncated. The full model display has since been run. The models do not establish causal effects of drill performance on NFL outcomes.

### *What will I compare next?*

I will add the fixed random forest specification from Lab 06 as a nonlinear alternative, retaining the linear models and baseline. It will use the same complete-case players and split, with 40 trees and seed 42. It may capture interactions, but can overfit and is harder to interpret than a linear model.

Forest results are now available and are summarised below. The comparison will be labelled exploratory because the test results have already been inspected. I will compare training and test errors and report OOB R² as a separate training-side diagnostic. The existing player-matching and selection limitations still apply.

### *What did the forest show?*

These are held-out RMSE values for identical complete-case samples. Lower is better. Yards and touchdown errors have different units and are not compared with one another.

| Position and target | Mean baseline | Six-drill linear | Random forest | Test players |
|---|---:|---:|---:|---:|
| RB yards | 1,948.57 | 1,857.53 | 1,842.32 | 18 |
| RB touchdowns | 14.03 | 13.28 | 11.15 | 18 |
| WR yards | 1,786.27 | 1,863.79 | 2,100.49 | 32 |
| WR touchdowns | 12.24 | 12.95 | 13.66 | 32 |
| TE yards | 1,214.39 | 1,260.21 | 1,238.55 | 11 |
| TE touchdowns | 7.89 | 7.61 | 8.74 | 11 |

Forest training R² ranged from 0.807 to 0.865. Forest test R² was 0.102 and 0.360 for RB yards and touchdowns, but negative for both outcomes in WRs and TEs. Five of six forest OOB scores were negative. Strong training fit has not translated into consistent held-out performance.

For these three model specifications, RMSE favours the forest for RBs, the baseline for WR outcomes and TE yards, and linear regression for TE touchdowns. MAE does not always give the same preference: RB yards MAE is lower for linear regression than the forest, and TE yards MAE is lower for the forest than the baseline. The metric and target therefore matter. This is not a ranking of every single-drill model.

The test sets have already influenced the decision to try another model. Results are exploratory, and additional test-driven tuning would weaken their interpretation further. No causal claim or claim of reliable future-class prediction follows from these scores.
