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
>I added an exact match based on Lab 02. For drafted players the current key uses full name, draft year and overall pick. For undrafted players it uses full name and school and requires the Combine year to equal the NFL rookie season as part of the key. I keep the Combine records with a left join and leave NFL production blank when a match is missing or unclear. I have not run the matching code yet, so I do not have a match count.

- Feedback from Andrew
>Andrew agreed with using an exact key for drafted players and keeping unmatched records separate from players with zero NFL production. For undrafted players, he suggested standardising names and schools, matching on those fields first, and using Combine year and rookie season to check the match. He recommended reviewing unclear cases by hand rather than automatically using fuzzy matching.

- Approach I will take
>I will keep the drafted key. For undrafted players I will make the name and school formats consistent across the two files by standardising capitalisation, punctuation and spacing, and recording any school aliases I verify. I will look for an exact name and school match first. If there is one candidate and the years agree, I will accept it automatically. If the years disagree or there is more than one candidate, I will flag the record for manual review.
>I may use a roster or player reference to confirm an identity and record the source and what it confirmed. The yearly NFL file will still be the source for production. If I cannot confirm the identity, I will leave the production blank.
>I will report how many records matched automatically, how many were reviewed manually, how many were confirmed through review, and how many stayed unmatched. I will also record the matching assumptions and the name and school rules I used.
