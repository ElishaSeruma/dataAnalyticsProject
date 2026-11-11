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
>For drafted players i match on full name, draft year and overall pick. For undrafted players i match on full name and school, and compare combine year with NFL rookie year. These are exact matches so differences in names, schools or years can stop a match.
>I keep the combine records even if i cannot match them to the NFL file. If the match is unclear or missing, i leave the production blank instead of calling it zero. The matching code has not been run yet so i do not have a match count.
