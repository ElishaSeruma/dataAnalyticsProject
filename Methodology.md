This file describes the methologies used throughout this project

#Problem
Since we are not evaluating medical, cognitive, playbook testing approcahes done in the combine and are only evaluting on the athletics style tests in a combine, this would null out certain groups of players like QB's, LB's, DL's, OL's, & DB's and we will have to go with the RB's, WR's & TE becasue their production in a reular NFL season statistics and thier combine presense and performance could give us a more substantial result.

>I want to see how combine drill results and participation relate to production in the first 5 NFL seasons. I will look at RB's, WR's & TE's separately because they have different roles and get different chances to run or catch the ball.

#*Method to use*
- Collect initial data
>I will be using NFL_Combine_Since_2000.csv & yearly_player_stats_offense.csv. The combine file has drill results and draft details for drafted and undrafted players. The yearly file has offensive stats by player, team and season.
>i will leave out the weekly and team files because i am using yearly stats for individual players.

- Describe the data
>I looked at the first few rows, the number of rows and columns, and the data types in both files.

- Verify the data quality
>I checked for missing values in the drill and production columns. I counted DNP separately from blanks because DNP means the player did not participate in that drill. It does not tell me why.

- Explore the data
>I checked the seasons covered and counted the records by position and season type. After filtering there were 985 combine records and 3,574 yearly NFL rows. These are row counts from the two files before matching, so they do not tell me how many players are in both files.

#*Data Preparation*
- Filter the records
>I am using regular season stats for RB's, WR's & TE's who entered the NFL from 2012 to 2021. I count the rookie season and the next 4 seasons so each player has the same 5 season window.

- Calculate player production
>I add rushing yards and receiving yards to get scrimmage yards. I also add rushing and receiving touchdowns. I group by player ID and add up their production over the 5 seasons, including stats from different teams if the player changed teams.

- Match the two files
>I added an exact match based on Lab 02. For drafted players the current key uses full name, draft year and overall pick. For undrafted players it uses full name and school and requires the Combine year to equal the NFL rookie season as part of the key. I keep the Combine records with a left join and leave NFL production blank when a match is missing or unclear. This was my initial approach before Andrew’s feedback.

- Feedback from Andrew
>Andrew agreed with using an exact key for drafted players and keeping unmatched records separate from players with zero NFL production. For undrafted players, he suggested standardising names and schools, matching on those fields first, and using Combine year and rookie season to check the match. He recommended reviewing unclear cases by hand rather than automatically using fuzzy matching.

- Approach I will take
>I will keep the drafted key. For undrafted players I will make the name and school formats consistent across the two files by standardising capitalisation, punctuation and spacing, and recording any school aliases I verify. I will look for an exact name and school match first. If there is one candidate and the years agree, I will accept it automatically. If the years disagree or there is more than one candidate, I will flag the record for manual review.
>I may use a roster or player reference to confirm an identity and record the source and what it confirmed. The yearly NFL file will still be the source for production. If I cannot confirm the identity, I will leave the production blank.
>I will report how many records matched automatically, how many were reviewed manually, how many were confirmed through review, and how many stayed unmatched. I will also record the matching assumptions and the name and school rules I used.

- Results from the direct match
>The revised match ran with 985 Combine records. It found 358 automatic matches and left 627 unmatched. I have not reviewed any records manually. One confirmed identity had no qualifying production, so 357 records had both a confirmed identity and production. The yearly CSV loaded 6,303 rows instead of the 10,856 seen earlier, so I need to confirm the input before treating these counts as final.

- Check player identities with another source
>I will look at the [nflverse player registry](https://nflreadr.nflverse.com/reference/load_players.html) and use roster records or official team records for cases that need more evidence. I will use these sources to confirm who the player is. The yearly NFL file stays as my source for production. More detail is in [Data Preparation](03_data_preparation/README.md).

- Results from matching with nflverse IDs
>I used the nflverse Combine reference and player registry to link PFR IDs to NFL GSIS IDs. The run loaded 10,856 yearly NFL rows and kept 3,574 after filtering. It confirmed 820 of 985 Combine identities. Of those, 651 had five-season production and 169 had no qualifying record. Another 165 identities stayed unresolved.

- Decision after further feedback from Andrew
>Andrew advised using the approach with the highest yield because there is not enough time to investigate both approaches further. I selected the nflverse Combine reference and player registry approach. It links PFR IDs to NFL GSIS IDs, then uses the yearly file for production. The direct approach stays commented out in the notebook as a record of the work.

- How I came to this decision
>The direct match gave 358 identities and 357 records with production, but it ran on only 6,303 yearly rows. The nflverse method ran on the full 10,856-row yearly file, confirmed 820 identities and gave 651 records with production. This is the largest usable group from the completed runs. Both runs kept 985 Combine prospects, but the inputs were different, so this is not proof that the second method is more accurate.
>I chose the reference method for its higher observed yield and its checks on unique IDs, agreement with yearly player IDs and undrafted rookie years. I have not completed an independent accuracy check or manual review. This replaces the planned comparison of both methods on the same input; that comparison has not been run.

- Which records I will use
>I will keep all 985 Combine records in the linked table. The production analysis will use the 651 confirmed identities with qualifying five-season production from the latest saved run. The 169 confirmed identities without qualifying production and the 165 unresolved identities will not be given zero production or included in outcome comparisons. The final count for each drill will also depend on whether a score was recorded.
>Production is available for 524 of 575 drafted prospects and 127 of 410 undrafted prospects. This means the results represent drafted players more fully. I will keep this limitation in the report rather than describe the sample as all Combine players.

- Final preparation checks
>The saved Colab results showed no repeated player-team-season-type keys before aggregation and no repeated player IDs in the 651-player sample. The production totals agreed exactly before and after grouping. The final IDs and production totals had no missing values. All missing numeric drill scores were explained by DNP, with no other missing numeric scores.
>I kept the original drill fields and made separate numeric scores and Boolean DNP flags. I did not replace DNP with zero or an average. The unusual-value check found no non-positive drill scores. Negative net yards and large performances were displayed for review, not automatically removed. I have not independently verified every identity or unusual source value.
>The retained sample had faster average dash times than the excluded sample in all three positions. I will explain this selection limitation and the lower coverage of undrafted players in the report. The preparation log is in the notebook and the Data Preparation folder.

#*Modelling*
- Task and targets
>I will use regression to examine whether Combine drill scores help predict five-season production. I will model scrimmage yards and offensive touchdowns separately for RB's, WR's & TE's. Names and IDs are not features.

- Train and test split
>I will use an 80/20 player split within each position with random_state=42, following Lab 04. The same player stays in training or test across models and targets. I will fit models and calculate baseline means on training data only. If I tune settings or compare feature combinations to choose a model, I will use a validation split within training and keep test data for the final evaluation.

- Baseline and models
>My baseline will predict the training group's mean production. The main model is simple linear regression with one drill score. The alternative is multiple linear regression with several drills, if enough complete records remain. I will compare the baseline and both models on the same players and split. Larger single-drill samples will be reported separately so differences in the players are not mistaken for differences between models.
>I will check complete-case training counts before fixing the multiple-regression specification. I will not choose features using test performance. No scaling or position encoding is needed for the initial models, and DNP scores will not be imputed.

- Evaluation and reproducibility
>I will report training and test MAE, RMSE and R², alongside coefficients, residual plots and sample sizes. I will calculate RMSE as the square root of MSE, as in Lab 06. I will record software versions and the split seed in Colab.
>The DNP mini-analysis will use descriptive summaries and box plots by position and drill rather than a classifier. More detail and the exact lab references are in [Modelling Approach](04_modelling/modelling-approach.md). These are planned methods; the models have not been run yet.

- First modelling results
>The linear models ran in Colab. The RB dash model explained about 2% of test variation in yards and only slightly improved on its mean baseline. The TE six-drill touchdown model had test R² about 0.071. These are examples visible in the saved outputs, not the best models across all results. The full metric tables were truncated, so I added a full display before making a ranking.
>The six-drill test samples had 18 RBs, 32 WRs and 11 TEs. Small test groups can make results unstable. Details are in [Initial Modelling Results](05_evaluation/modelling-results.md).

- Add a nonlinear alternative
>I added random forest regression using the class configuration from Lab 06, cell 61: 40 trees, bootstrap sampling, OOB scoring and seed 42. It uses the same complete-case players and splits as the linear comparison. I will compare MAE, RMSE and R² and check the gap between training and test performance.
>I am testing whether a nonlinear model adds useful information, not assuming it will be better. The comparison is exploratory because I have already seen test scores. I will not tune the forest on test performance. The forest and full-results cells have now been run in Colab, and all existing notebook work is kept.

- Results from the forest comparison
>The forest improved test RMSE over the baseline for RB yards and touchdowns, but not for WR or TE outcomes. Training R² was above 0.80 in all six forest fits, while most OOB scores and four of six test R² scores were negative. This suggests overfitting and unstable generalisation in these small samples.
>I will report each target and metric separately because RMSE and MAE do not always favour the same model. I will retain the baseline and linear models rather than claim the forest is best overall. The next analysis can address drill associations and DNP directly without choosing more model settings from test performance.
