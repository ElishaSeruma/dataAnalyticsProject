# **Data Preparation**

### *What happened when I matched the players?*

The saved Colab run (commit `27469c7`) kept all **985** Combine records. It matched **358 (36.3%)** automatically and left **627 (63.7%)** unmatched. No records were reviewed manually. One confirmed identity had no qualifying production record, leaving **357 records** with both a confirmed identity and a five-season production total.

This is the result for the CSV used in that run. Colab loaded 6,303 yearly NFL rows instead of the 10,856 seen earlier. I have not confirmed why the input was smaller. Unmatched records are not recorded as zero, and these counts do not show whether Combine performance relates to NFL production.

### *What other source could help identify players?*

I would start with the [nflverse player registry](https://nflreadr.nflverse.com/reference/load_players.html). It lists NFL player identities with GSIS and Pro Football Reference IDs, names, college, and draft information. The [nflverse Combine data dictionary](https://nflreadr.nflverse.com/articles/dictionary_combine.html) also lists a Pro Football Reference ID where available. This gives me a possible link from a verified Combine record to an NFL ID. I would first check that the registry IDs agree with the identity fields and IDs in my yearly file.

For drafted players, I can use draft year and overall pick to find an official team announcement and check the name and school. For example, the [Broncos' 2021 draft announcement](https://www.denverbroncos.com/news/javonte-williams-nfl-draft-running-back-north-carolina) confirms Javonte Williams was the North Carolina running back selected 35th overall. He appeared in the unmatched output, so this confirms his draft details but does not explain why the Colab match failed.

For undrafted players, I would use the [nflverse roster records](https://nflreadr.nflverse.com/articles/dictionary_rosters.html) and official team profiles or signing announcements where available. Roster fields such as name, college, season, team and player ID can help confirm a candidate. Finding a roster record does not prove a player recorded NFL production, and not finding one does not prove zero production.

The reference would confirm identities only. I would keep the yearly NFL CSV as the source for yards and touchdowns, record the source and reason for each manual decision, and leave production missing if the link or outcome cannot be verified.

### *What did the nflverse ID match find?*

The second saved run (commit `57cc8f9`) used the full **10,856-row** yearly file and kept **3,574** yearly rows after filtering. The earlier direct match used **6,303** yearly rows and kept **2,072**. Both runs kept the same 985 Combine records.

| Result | Direct match, smaller yearly input | nflverse ID match, full yearly input |
|---|---:|---:|
| Confirmed with qualifying production | 357 | 651 |
| Confirmed without qualifying production | 1 | 169 |
| Identity unresolved | 627 | 165 |
| **Total** | **985** | **985** |

In the nflverse run, **86** records had no PFR ID found, **70** had no unique registry ID, and **9** need a rookie-year check. Qualifying production was available for **524 of 575 drafted players (91.1%)** and **127 of 410 undrafted players (31.0%)**. The analysis sample will therefore represent drafted prospects more fully.

### *Which method did I select?*

After further feedback from Andrew, I selected the **nflverse PFR-to-GSIS ID method** because it gave the highest usable yield in the completed runs and there is limited time to investigate both methods further. The direct matching code stays commented out in the notebook. The planned same-input comparison has not been run.

The registry method produced **651** confirmed records with qualifying production, compared with **357** in the direct run. The yearly inputs differed, so this is a practical choice based on observed coverage, not proof of higher matching accuracy. The selected method checks unique IDs, agreement with the yearly file where available, and undrafted rookie years. No manual review has been completed.

The linked table retains all 985 prospects. Production comparisons use confirmed identities with qualifying production; the other 334 prospects remain documented as missing outcomes or unresolved identities. Each drill analysis will have a smaller sample where scores are DNP. The difference in coverage between drafted and undrafted players remains a limitation.
