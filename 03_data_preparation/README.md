# **Data Preparation**

### *What happened when I matched the players?*

The saved Colab run (commit `27469c7`) kept all **985** Combine records. It matched **358 (36.3%)** automatically and left **627 (63.7%)** unmatched. No records were reviewed manually. One confirmed identity had no qualifying production record, leaving **357 records** with both a confirmed identity and a five-season production total.

This is the result for the CSV used in that run. Colab loaded 6,303 yearly NFL rows instead of the 10,856 seen earlier. I have not confirmed why the input was smaller. Unmatched records are not recorded as zero, and these counts do not show whether Combine performance relates to NFL production.

### *What other source could help identify players?*

I would start with the [nflverse player registry](https://nflreadr.nflverse.com/reference/load_players.html). It lists NFL player identities with GSIS and Pro Football Reference IDs, names, college, and draft information. The [nflverse Combine data dictionary](https://nflreadr.nflverse.com/articles/dictionary_combine.html) also lists a Pro Football Reference ID where available. This gives me a possible link from a verified Combine record to an NFL ID. I would first check that the registry IDs agree with the identity fields and IDs in my yearly file.

For drafted players, I can use draft year and overall pick to find an official team announcement and check the name and school. For example, the [Broncos' 2021 draft announcement](https://www.denverbroncos.com/news/javonte-williams-nfl-draft-running-back-north-carolina) confirms Javonte Williams was the North Carolina running back selected 35th overall. He appeared in the unmatched output, so this confirms his draft details but does not explain why the Colab match failed.

For undrafted players, I would use the [nflverse roster records](https://nflreadr.nflverse.com/articles/dictionary_rosters.html) and official team profiles or signing announcements where available. Roster fields such as name, college, season, team and player ID can help confirm a candidate. Finding a roster record does not prove a player recorded NFL production, and not finding one does not prove zero production.

The reference would confirm identities only. I would keep the yearly NFL CSV as the source for yards and touchdowns, record the source and reason for each manual decision, and leave production missing if the link or outcome cannot be verified.
