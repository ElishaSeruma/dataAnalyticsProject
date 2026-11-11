# **Data Understanding**

### *What data did I use?*

I used two files to look at how Combine drill results and participation relate to production in the first five NFL seasons.

| File | What it contains |
|---|---|
| `NFL_Combine_Since_2000.csv` | One row per prospect record. It includes the player's name, school, position, Combine year, drill results, and draft details. The file covers 2000–2026. |
| `yearly_player_stats_offense.csv` | Offensive stats by player, team, season, and season type. It includes player IDs, rookie seasons, rushing and receiving yards, and touchdowns. The file covers 2012–2025. |

I used the yearly player file because my question is about each player's production over several seasons. The weekly and team files are not part of this analysis.

**DATA SOURCES:**

- [NFL Combine 2000–2026 — Isaak Sipple](https://www.kaggle.com/datasets/isaaksipple/nfl-combine-2000-2025)
- [NFL Stats 2012–2025 — Philip Hyde](https://www.kaggle.com/datasets/philiphyde1/nfl-stats-1999-2022)

### *What did I find when I looked at the data?*

I looked at the first few rows, the number of rows and columns, and the data types in both files. The NFL file includes regular-season and playoff records. I kept regular-season records for running backs, wide receivers, and tight ends. For NFL players who entered between 2012 and 2021, I looked at the rookie season and the next four seasons. I also looked at Combine records from 2012 to 2021 for those positions.

| Position | Combine records | Yearly NFL rows |
|---|---:|---:|
| Running back (RB) | 305 | 1,165 |
| Wide receiver (WR) | 488 | 1,578 |
| Tight end (TE) | 192 | 831 |
| **Total** | **985** | **3,574** |

These are counts from the two files before matching. An NFL player can have more than one row because they played in different seasons or for different teams.

### *Were there missing values or players who did not take part?*

I found no blank values in the six drill columns or the four rushing and receiving production columns in the filtered rows. There were still drill entries marked **DNP**, which means the player was recorded as not participating in that drill. It does not tell me why they did not take part.

For example, **77** Combine records were marked DNP for the 40-yard dash: 16 running backs, 39 wide receivers, and 22 tight ends. The three-cone drill had **359** DNP records. I kept DNP separate from the recorded scores because drill participation is part of the research question.

### *Can the two files be linked?*

The yearly NFL file has a player ID, but the Combine file does not share it. Names, schools, years, and draft details give me a way to compare records. The counts above do not show how many players match, and an unmatched Combine record does not mean the player had zero NFL production.
