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

These are visible examples, not a ranking of all models. The saved results tables were truncated. A full display has been added but has not been run. The models do not establish causal effects of drill performance on NFL outcomes.

### *What will I compare next?*

I will add the fixed random forest specification from Lab 06 as a nonlinear alternative, retaining the linear models and baseline. It will use the same complete-case players and split, with 40 trees and seed 42. It may capture interactions, but can overfit and is harder to interpret than a linear model.

No forest results are available yet. The comparison will be labelled exploratory because the test results have already been inspected. I will compare training and test errors and report OOB R² as a separate training-side diagnostic. The existing player-matching and selection limitations still apply.
