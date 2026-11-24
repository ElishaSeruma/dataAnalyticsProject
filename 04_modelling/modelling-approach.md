# **Modelling Approach**

### *What question will the models answer?*

I will examine whether recorded Combine drill scores are associated with, and help predict, offensive production over the first five NFL seasons. I will analyse RBs, WRs and TEs separately. This is a **regression task**, supported by descriptive association analysis. I am not classifying players as successful or unsuccessful.

### *What data will I use?*

The prepared sample has **651 players**: 209 RBs, 316 WRs and 126 TEs. Each row represents one player with an accepted identity and both production totals. The input features are recorded numeric drill scores. The two targets are modelled separately:

- **Scrimmage yards:** rushing yards plus receiving yards over the five-season window, in yards.
- **Offensive touchdowns:** rushing plus receiving touchdowns over that window, in touchdown counts.

Names and IDs identify records but are not model features. Position defines the separate groups. Draft status is retained for coverage reporting, not used as a predictor in the initial models. No outcome or outcome-derived field is an input feature.

DNP is not a zero score. Each single-drill model uses players with that score recorded, after the player split. Multiple regression uses complete records for its selected drills. I will report the resulting training and test counts because they may be much smaller than 651.

### *How will I split the data?*

Within each position, I will split player rows into **80% training and 20% test**, using **random_state=42**. Each player stays on the same side across drills, targets and comparison models. I will record the player IDs in each split.

The training portion is used to fit coefficients and calculate baseline means. The held-out test portion assesses performance. The initial model specifications are fixed before inspecting test scores. If I tune parameters or choose feature combinations based on performance, I will create a validation split inside the training portion and keep the test set untouched until those choices are final.

This random split evaluates held-out players from the observed cohorts. It is not a test of prediction for future draft classes. Known full-sample summaries and earlier data exploration mean the analysis is exploratory rather than a preregistered confirmatory study.

### *What is the baseline?*

For each comparison sample and target, the baseline predicts the **training target mean** for every player. This gives the regression models a simple reference point. The mean is calculated from that model's training records only, not from the test data or the full dataset.

### *What is the main model?*

I will fit **simple linear regression**, using one drill score to predict one target within one position. The form is:

> predicted production = intercept + coefficient × drill score

This makes the relationship easy to explain in source units. For example, a dash coefficient describes the fitted change in yards per extra second, not the effect of running faster on a player's career. I will report associations rather than causes.

### *What is the alternative?*

I will compare simple regression with **multiple linear regression** using several drills together. The candidate specification uses the six numeric drill scores. Complete-case training counts will determine whether that specification is feasible; if not, any smaller, substantively justified feature set will be recorded before test evaluation. I will not search combinations using test scores.

For a fair comparison, the baseline, single-drill models and multiple regression will be fitted and evaluated on the **same complete-case players and the same split** in the comparison table. Separate single-drill analyses can use their larger available samples, but their scores will not be treated as directly comparable model rankings across different samples.

Random forest was initially optional and is now included as the fixed nonlinear alternative described below. Classification and clustering are not needed to answer the current question.

### *What settings and preparation are needed?*

The 20% test allocation follows the regression practical and preserves most observations for fitting. The fixed seed makes the split repeatable. Ordinary linear regression estimates coefficients by least squares; there is no regularisation parameter to tune in this initial setup.

Scores remain in their original units. Linear regression with an intercept does not require standardisation here, and original units make coefficients easier to explain. Positions are separate, so position encoding is unnecessary. No drill or missing production values will be imputed. Any later learned preparation will be fitted on training data only.

### *How will I assess the models?*

I will report **training and test MAE, RMSE and R²** for the baseline and models. MAE and RMSE are in yards or touchdowns; lower is better. R² can be negative on test data, meaning the model is worse than a constant prediction based on the test-set mean. This is distinct from the fitted training-mean baseline used for model comparison.

RMSE will be calculated as the square root of MSE, following Lab 06. I will also inspect coefficients and residual plots, compare training and test performance, and report each sample size. A model that does not improve on the baseline is still an informative result.

### *How does the DNP mini-analysis fit?*

I will use grouped production summaries and box plots to compare players with a recorded result and those marked DNP, separately by position and drill. This is descriptive analysis, not a fitted classifier, so it does not require its own prediction split. These comparisons will not be used to tune the regression specifications.

### *What are the limitations?*

Only 651 of 985 Combine records have accepted identities and qualifying production. Coverage is much higher for drafted than undrafted prospects. Retained players also had faster mean dash scores than excluded prospects within each position. The results therefore describe the selected sample rather than every Combine prospect.

Production is skewed, drill scores may be correlated, and complete-case selection can reduce sample size further. Linear regression may miss nonlinear patterns, have non-constant residual variance, or predict negative touchdowns. Predictions will be evaluated as fitted; any later constraint or alternative will be documented and assessed without tuning on the test set. Small test groups can produce unstable metrics, and checking many drills increases the chance of apparently interesting patterns.

### *Where does this come from in the practicals?*

Cell numbers count from the top, including Markdown cells.

| Method | Practical source |
|---|---|
| 80/20 split, seed 42, simple regression and training/test metrics | Lab04 - Regression, cell 5 |
| Multiple regression, coefficients and residual diagnostics | Lab04 - Regression, cell 9 |
| Baseline comparison principle | Lab05_Logistic_Regression_and_Classification, cell 26 |
| Mean calculation for the regression baseline | Lab01_intro_to_pandas (1), cell 40 |
| Correct square-root RMSE calculation | Lab06_Random_Forest, cell 27 |
| Pearson correlation and heatmap | Lab01_intro_to_pandas (1), cells 51 and 57 |
| Grouped summaries and box plots | Lab02_exploratory_data_analysis (2), cell 105; Analyze Data, cell 27 |

The training-mean baseline adapts the classification baseline principle to a continuous target; it is not the same classifier used in Lab 05. Software versions for Python, pandas, NumPy, scikit-learn and plotting libraries will be recorded when the modelling cells run in Colab. The first linear models have now been run in Colab; their saved results are documented in the Evaluation folder.

### *Why add random forest now?*

The visible held-out linear results show modest explanatory performance in some examples: RB dash regression has test R² about 0.020 for yards, and the TE six-drill model has test R² about 0.071 for touchdowns. The saved metric tables are truncated, so these examples do not establish the strongest or weakest model across the full set.

Random forest can represent nonlinear patterns and interactions between drills without specifying them in a linear equation. I will test the Lab 06, cell 61 configuration: 40 trees, bootstrap sampling, out-of-bag scoring and seed 42. Forty trees reproduce the class specification and provide a fixed starting comparison; the choice is not claimed to be optimal. Other parameters use the class estimator defaults, with unrestricted tree growth posing an overfitting risk in these small samples.

I will use the same complete-case training and test players as the six-drill regression, with the same targets and training-mean baseline. OOB R² provides an additional training-side check. Train/test MAE, RMSE and R² remain the main reported metrics. The models use the existing helper with square-root RMSE from Lab 06, cell 27.

The comparison is exploratory because test results were viewed before proposing this alternative. No forest settings will be selected using test scores. Any later tuning needs validation within training. The existing linear models remain in the notebook because they are simpler to interpret and provide a useful reference. The forest has not been run yet.
