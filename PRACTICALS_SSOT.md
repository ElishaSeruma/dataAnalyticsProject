# Practicals SSOT: approaches covered in class

The single source of truth for every technique used in the DBMS71008 in-class practicals, organised by CRISP-DM phase. Use it to check that a method in the CA2 notebook is one we have covered, and to reuse the same idiom the labs used.

Source notebooks live in `DA-practical notes/` (kept local, not committed).

| Lab | Notebook | Dataset | Focus |
|---|---|---|---|
| L1 | `Lab01_intro_to_pandas` | California housing | pandas basics, descriptive stats, first plots |
| L2 | `Lab02_exploratory_data_analysis` | Housing + addresses | EDA, filtering, merging, groupby |
| SS | `Example - Summary Statistics and Basic Plotting` | Random numbers | Summary stats, missing values, basic plots |
| AD | `Analyze Data` | Pima Indians Diabetes | 7 data-understanding recipes, uni/multivariate plots |
| L3 | `Lab03_CRISP-DM Data Preparation` | Housing (merged) | Feature engineering, cleaning, imputation, encoding |
| L4 | `Lab04 - Regression` | Restaurant ratings, airline costs | Simple and multiple linear regression |
| L5 | `Lab05_Logistic_Regression_and_Classification` | Telco churn | Logistic regression, baseline, pipeline, metrics, ethics |
| L6 | `Lab06_Random_Forest` | Housing (processed) | Decision tree → random forest, bagging, OOB, tuning |
| UC | `Unsupervised_Clustering` | Toy 2D points, Breast Cancer Coimbra | k-means |

---

## Project question (for context)

> Among RBs, WRs and TEs in the NFL Combine dataset, how are drill results and drill participation associated with regular-season offensive production over the first five seasons after entering the NFL?

- **Main analysis:** per position, association between drill results and five-year production (primary: rushing + receiving yards; secondary: touchdowns).
- **Mini-analysis:** per drill, players with a recorded result vs players marked **DNP** ("did not participate"), compared on five-year production and draft position. DNP is never described as "did not attend the combine".

The **Project use** column in each table below says where a technique fits this question.

---

## 0. Setup and conventions

| Approach | How the labs did it | Lab | Project use |
|---|---|---|---|
| Standard imports | `pandas as pd`, `numpy as np`, `seaborn as sns`, `matplotlib.pyplot as plt` in one cell at the top | L1, L2, L3 | Same single imports cell at top of the notebook |
| Plot styling | `sns.set(color_codes=True)`; `sns.set_palette(sns.color_palette("muted"))` or `sns.set_theme(style="whitegrid", palette="muted")` | L1, L5 | Same |
| File paths | `DATA = Path("./data")` with `pathlib`, then `pd.read_csv(DATA/'file.csv')` | L1, L2 | `DATA = Path("data")`: relative paths are a brief requirement |
| Fixed seed | `RANDOM_STATE = 42` passed to every split/model | L4, L5, L6 | Same; record it in the notebook |
| Shell from notebook | `!ls {DATA}` | L1 | Optional sanity check that data files exist |

## 1. Business Understanding

| Approach | How the labs did it | Lab | Project use |
|---|---|---|---|
| State the business question before modelling | Churn: who leaves, what action follows, cost of each error | L5 | Who uses drill-vs-production evidence (scouts, analysts) and what decision it informs |
| Identify unit of analysis, target, positive class | "What does one row represent?" exercise | L5 | One row = one combine participant (RB/WR/TE) |
| Cost of errors / practical success | False positive vs false negative consequences; €5 contact / €60 retention value exercise | L5 | Practical criteria: a weak correlation can still be uninformative for scouting |
| CA transfer table | Unit, target type, baseline, key error, key metric, imbalance, leakage, ethics, generalisation | L5 | Fill this table in for our project in `01_business_understanding/` |
| CRISP-DM phase framing | Every exercise labelled with its CRISP-DM phase | L2 | Notebook structured by the six phases |

## 2. Data Understanding

### 2.1 Inspect structure

| Approach | Code idiom | Lab | Project use |
|---|---|---|---|
| Peek at rows | `df.head()`, `df.head(10)`, `df.tail()`, `df.head().T` | L1, AD, L5 | Check combine and stats files load cleanly |
| Random sample | `df.sample(n=5, random_state=42)` | L1 | Spot-check name formats for matching |
| Shape | `df.shape`, `len(df)` | L2, AD | Row counts before/after every filter and join |
| Types and non-nulls | `df.info()`, `df.dtypes` | L1, AD, L3 | Drill columns load as `object` because of `"DNP"` strings |
| Column list | `df.columns.tolist()` | L2, L3 | The yearly stats file has many columns, so select early |
| Unique counts | `df[col].nunique()` | L2 | Positions, schools, seasons |
| Frequencies | `df[col].value_counts()`, `.value_counts(normalize=True)`, `.nlargest(10)` | L1, L2, L5 | Players per position; DNP share per drill |
| Handover check | Shape + columns + dtypes + `describe(include="all").T` in one cell | L3 | Start of Data Preparation |

### 2.2 Descriptive statistics

| Approach | Code idiom | Lab | Project use |
|---|---|---|---|
| Summary table | `df.describe()` | L1, AD, SS | Drill results and production per position |
| Single-column stats | `.mean()`, `.median()`, `.std()`, `.var()`, `.min()`, `.max()` | L1, SS | Compare mean vs median of production (expected heavy right skew) |
| Display precision | `pd.set_option("display.precision", 3)` | AD | Note: lab used old `set_option('precision', 3)`, which errors in current pandas |
| Class balance | `df.groupby('class').size()`, `value_counts(normalize=True)` | AD, L5 | Recorded vs DNP group sizes per drill |
| Skew | `df.skew()` | AD | Production totals are likely highly skewed and include many zeros |
| Group summaries | `df.groupby(key).agg({col: 'sum'})`, `.sort_values(...)` | L2 | Production and DNP rates by position / combine year |

### 2.3 Visualisation

| Approach | Code idiom | Lab | Project use |
|---|---|---|---|
| Histogram | `sns.histplot(series, kde=False)`, `df.hist()`, varying `bins` | L1, AD, SS | Distribution of each drill and of five-year production |
| Density plot | `df.plot(kind='density', subplots=True, layout=(3,3), sharex=False)` | AD | Drill distributions by position |
| Box plot | `df.plot(kind='box', subplots=True, ...)`, `df.boxplot()` | AD, SS, L1 | Production: recorded vs DNP; outlier candidates |
| Bar plot | `sns.barplot(x=..., y=..., data=...)` | L2, L3 | DNP rate per drill; missing-value fractions |
| Count plot | `sns.countplot(data=df, x=col)` | L5 | Players per position |
| Scatter | `sns.scatterplot(..., alpha=0.1, hue=...)` | L1, L2 | Drill result vs production, `hue="Position"` |
| Hexbin | `sns.jointplot(x=..., y=..., kind='hex')` | L1 | Dense drill vs production scatter |
| Pair plot | `sns.pairplot(df[cols].dropna())`; with `kind='reg'` | L1, L4 | Drills vs production per position |
| Scatter matrix | `pandas.plotting.scatter_matrix(df)` | AD | Alternative to pairplot |
| Multi-panel figure | `fig, (ax0, ax1) = plt.subplots(1, 2)`, `fig.tight_layout()` | L3 | Before/after cleaning comparisons |

### 2.4 Correlation

| Approach | Code idiom | Lab | Project use |
|---|---|---|---|
| Pearson matrix | `df.select_dtypes(include=['number']).corr()`; `df.corr(method='pearson')` | L1, AD | **Core of main analysis:** drill vs production, per position |
| Rank against target | `corr_matrix['target'].sort_values(ascending=False)` | L1, L3 | Rank drills by association with five-year yards |
| Heatmap | `sns.heatmap(corr, annot=True, fmt='.2f', cmap="coolwarm", vmin=-1, vmax=1)`; `ax.matshow(corr)` | L1, L4, AD | One heatmap per position |
| Pearson caveat | Pearson assumes roughly normal, linear relationships; 0 means no *linear* relationship | L1, AD | Production is skewed and zero-heavy, so Spearman (`method='spearman'`) is the natural extension. It uses the same `.corr()` call with a different method |

### 2.5 Data quality checks

| Approach | Code idiom | Lab | Project use |
|---|---|---|---|
| Missing counts | `df.isnull().sum()`, `df.isna().sum()` | L3, L5 | Missing after converting `"DNP"` → `NaN` |
| Missing fractions + plot | `(df.isnull().sum()/len(df)).sort_values(ascending=False)` → `sns.barplot` | L3 | DNP rate per drill per position |
| Text that should be numeric | `pd.to_numeric(col, errors="coerce")`, then count new NaNs | L5 | Drill columns: `"DNP"` → NaN, while keeping a separate DNP flag |
| Capped values | Count and % of rows at a cap (house value ≥ 500k) | L1, L2, L3 | Check for artefacts such as zero-production players and the stats window ending in 2025 |
| Null semantics | `isnull` / `notnull`; NaN skipped in stats by default (`skipna`) | SS | Unmatched players vs genuinely zero production must not both become NaN or 0 silently |

## 3. Data Preparation

| Approach | Code idiom | Lab | Project use |
|---|---|---|---|
| Rename columns | `df.rename(columns={...}, inplace=True)` | L2 | Harmonise combine (`First`, `Last`, `Year`) and stats (`player_name`, `draft_year`) |
| Build a join key | Concatenate columns as string: `a.astype(str) + ',' + b.astype(str)` | L2 | Normalised `name + draft year (+ school)` key |
| Merge | `pd.merge(left, right, how="left", on=key)`; know `left/right/inner/outer` | L2 | Left-join combine → five-year production so non-matches stay visible |
| Duplicate-key warning | Many-to-one keys inflate rows; `drop_duplicates()` before merge | L2 | Validate cardinality; check row count before/after the join |
| Filter rows (masks) | `df.loc[df[col] > x]`, `(a) & (b)`, `.str.contains(...)` | L2 | Keep RB/WR/TE; `season_type == "REG"`; eligible combine years |
| Record removal impact | `n_rows_raw = len(df)` then `1 - len(df)/n_rows_raw` | L3 | Each selection step in the preparation log |
| Drop columns | `df.drop(columns=[...])` | L2, L3 | Drop the many unused stats columns |
| Feature engineering | New column from a ratio/sum: `df['a_per_b'] = df.a / df.b` | L3 | `total_yards = rushing_yards + receiving_yards`; `total_td = rush_td + receiving_td`; five-season sums |
| Aggregate per entity | `groupby(key).agg({...})` | L2 | Sum player-season rows into one row per player |
| Strings → categories | `astype('category')`; ordered categories via `cat.set_categories([...], ordered=True)` | L3 | `Position`; `Drafted` |
| Reusable cleaning functions | `convert_strings_to_categories(df)`, `fill_missing_values_with_median(df)` | L3 | Write functions for name normalisation and DNP flagging |
| Drop rows with missing | `df.dropna(subset=[col])` | L3, L4 | Per-drill analysis: drop only that drill's DNPs for the correlation |
| Drop features | Drop columns where imputing makes no sense | L3 | N/A so far |
| Median imputation | `col.fillna(col.median())` | L3 | **Avoid for drills:** imputing DNP would erase the participation signal the mini-analysis studies |
| Leakage-safe imputation | `SimpleImputer` inside a `Pipeline`, fitted on training data only | L5 | Only if a train/test model is used. L3 imputed before splitting, which the brief warns against |
| Integer codes | `astype('category').cat.codes + 1` | L3 | Avoid for nominal categories (implies an order) |
| One-hot encoding | `pd.get_dummies(df)`; `OneHotEncoder(handle_unknown="ignore")` | L3, L5 | `Position` if pooled in a model |
| Scaling | `StandardScaler()` in a pipeline | L5 | Needed for logistic regression / k-means; not for trees or correlation |
| Save prepared data separately | `df.to_csv(DATA/'..._processed.csv', index=False)` | L2, L3 | Raw files untouched; write prepared data to a separate file |

## 4. Modelling

### 4.1 Linear regression (L4)

- Simple: `y = β0 + β1·x`; multiple: `y = β0 + β1·x1 + … + βn·xn`. Also calculated by hand from ΣX, ΣY, ΣXY, ΣX².
- Idiom: `X = df[["feature"]]` (double brackets), `train_test_split(X, y, test_size=0.2, random_state=42)`, `LinearRegression().fit(X_train, y_train)`.
- Interpret `intercept_` and `coef_` in context and judge whether the intercept is meaningful.
- Coefficients table: `pd.Series(model.coef_, index=features)`.
- **Project use:** per-position regression of five-year yards on drill result(s). This goes beyond pairwise correlation by holding other drills or draft pick constant.

### 4.2 Logistic regression / binary classification (L5)

- Target mapped to 0/1, with the positive class stated explicitly.
- Identifier and sensitive columns excluded from features but kept for auditing.
- Stratified split: `train_test_split(..., stratify=y)`.
- **Baseline:** `DummyClassifier(strategy="most_frequent")`.
- **Pipeline:** `ColumnTransformer` (numeric: median imputer + `StandardScaler`; categorical: most-frequent imputer + `OneHotEncoder`) → `LogisticRegression(max_iter=2000)`.
- `predict()` gives classes; `predict_proba()[:, 1]` gives probabilities. Thresholds explored at 0.3 / 0.5 / 0.7.
- Coefficients read as log-odds and described with "is associated with … holding other features constant", never as causes.
- **Project use:** optional, e.g. whether a player "reached a production threshold" if a binary framing is approved.

### 4.3 Decision tree → random forest (L6)

- Regression tree: leaves predict the mean target; classification tree: leaves predict a class.
- `RandomForestRegressor(n_estimators=10, n_jobs=-1, random_state=42)` as baseline.
- Single interpretable tree: `n_estimators=1, max_depth=3, bootstrap=False` → `plot_tree(model.estimators_[0], feature_names=...)`.
- Unrestricted depth gives train R² = 1, which is overfitting. Always compare train vs validation.
- Bagging (bootstrap + aggregate); R² vs number of trees plot.
- **OOB score:** `oob_score=True` → `model.oob_score_`, useful when data is small.
- Tuning: `n_estimators` (10/20/40/80), `min_samples_leaf` (3/5/10/25), `max_features` (0.5, 1.0, 'log2', 'sqrt').
- Scoring helper `print_rf_scores` reports train/valid RMSE, R², OOB.
- **Project use:** a non-linear alternative to linear regression, compared against it. RF handles NaN poorly in older sklearn, so DNP handling must be explicit.

### 4.4 k-means clustering (UC)

- Choose k → assign points to nearest centroid → recompute centroids → repeat to convergence.
- `KMeans(n_clusters=k).fit(X)`; `cluster_centers_`, `labels_`.
- Evaluated against known labels in the Coimbra exercise.
- **Project use:** not central to the question; possible exploratory profiling of athletic types.

## 5. Evaluation

| Approach | How the labs did it | Lab | Project use |
|---|---|---|---|
| Regression metrics | `r2_score`, `mean_absolute_error`, RMSE (`np.sqrt(mean_squared_error(...))`) on train **and** test | L4, L6 | Yards in original units: MAE/RMSE interpretable as "yards off" |
| **RMSE gotcha** | L4's `metrics()` labels plain `mean_squared_error` as "RMSE" (comment: "Removed squared=False"). That value is MSE | L4 | Use `np.sqrt(mean_squared_error(...))` as L6 does, or `root_mean_squared_error` |
| Residual diagnostics | Residuals vs fitted scatter with `axhline(0)`; check linearity and constant variance | L4 | Expect funnel shape from skewed production; consider log transform |
| Train vs validation gap | Compare R² on train vs valid to detect overfitting | L6 | Report both for every model |
| Baseline comparison | Majority-class dummy vs model in one table | L5 | Regression equivalent: predict the training mean/median (`DummyRegressor`) |
| Confusion matrix | `ConfusionMatrixDisplay.from_predictions(...)`; `confusion_matrix(...).ravel()` → tn, fp, fn, tp | L5 | Only if a classification framing is used |
| Precision / recall / F1 | `classification_report(..., digits=3)`; manual calculation from counts | L5 | Same |
| Accuracy is not enough | Imbalanced data: a majority baseline scores high accuracy with zero recall | L5 | Brief stresses this too |
| Threshold analysis | `metrics_at_threshold()` table + line plot | L5 | Same |
| Cluster vs labels | Accuracy of clusters against true labels | UC | N/A |
| Cautious wording | "Associated with", never "causes" | L5 | Required throughout: our design is observational |
| Ethics checklist | Purpose of data, sensitive proxies, subgroup error rates, harm, challenge route, human review, drift monitoring | L5 | Evaluation + Deployment sections |
| Subgroup audit | Compare recall / FP between groups of an excluded attribute | L5 | Compare results across positions and draft rounds |
| "Improve the analysis, not the score" | Document change, relevance, evidence, practical meaning, limitation | L5 | Template for each iteration in the notebook |

## 6. Deployment

Covered only conceptually in the labs: L2 lists Deployment as "communicating or implementing results", L4 as "communicating findings or integrating the model into decision-making", and L5's ethics checklist includes monitoring for drift. No lab builds a deployed system, which matches the brief (not required).

---

## Gaps: things the question likely needs that no practical covers

These are not covered in the labs, so each would need lecturer approval or clear justification and citation:

- **Spearman rank correlation.** Same `.corr()` call, but the rationale (skewed, zero-heavy outcome) must be explained.
- **Significance tests / confidence intervals for correlations.** E.g. `scipy.stats.spearmanr`, bootstrap CIs.
- **Two-group comparisons for the DNP mini-analysis.** E.g. Mann-Whitney U, difference in medians, effect size. The labs only cover descriptive group comparisons (`groupby`, box plots).
- **Fuzzy / normalised name matching** for joining combine to stats (no shared ID). L2 only joins on an exact key.
- **Multiple-comparison awareness.** 6 drills × 3 positions × 2 outcomes = 36 correlations, so some will look "significant" by chance.

## Deprecated code in the practicals

| Lab code | Problem | Use instead |
|---|---|---|
| `sns.distplot(...)` | Removed in recent seaborn | `sns.histplot(...)` |
| `set_option('precision', 3)` | Errors in pandas ≥ 1.4 | `pd.set_option('display.precision', 3)` |
| `df2.ix['a']` | `.ix` removed | `.loc['a']` |
| `ndarray.ptp()` | Removed in NumPy 2 | `np.ptp(arr)` |
| `mean_squared_error(..., squared=False)` | Removed in sklearn 1.6 | `root_mean_squared_error` or `np.sqrt(mean_squared_error(...))` |
