#**Business Understanding**
##***What decisions or questions actually matter***

###*How is the NFL combine operated?*

The **NFL Scouting Combine** is an annual, invitation-only evaluation held in Indianapolis before the NFL Draft. It brings roughly 300 draft-eligible prospects to one place so all 32 NFL clubs can gather medical information, interview players, and assess their athletic ability under similar conditions. National Football Scouting (NFS) coordinates the event and distributes centrally collected information to the clubs. Each club uses that information to make its own draft decisions.

**1. Prospects are selected and invited.** NFL teams provide input on draft-eligible players, and a Player Selection Committee votes on which prospects to invite by position. The invitation process means the Combine is already a selected sample: it does not include every draft-eligible player. For example, the NFL listed 319 invitees in 2026. Being invited is also distinct from attending or completing a particular drill.

**2. Prospects go through medical evaluations and team interviews.** A medical advisory committee reviews players’ medical histories before they arrive. Prospects undergo standard examinations, and medical staff can request additional tests based on a player’s history, such as imaging for a previous knee injury. NFS compiles the findings into electronic medical records available to all 32 clubs. Teams also meet with prospects individually to ask about their football background, discuss how they read plays, and learn how they communicate and respond to feedback. These evaluations are a substantial part of the event even though fans mostly see the workouts.

**3. Prospects are measured and work out in position groups.** Staff record body measurements such as height and weight. On-field sessions are scheduled by position group at Lucas Oil Stadium. The standard athletic drills give teams a common way to compare particular abilities; position-specific drills let evaluators see how prospects move while performing football tasks. For example, a receiver’s 40-yard dash measures straight-line speed, while route-running and catching drills let scouts observe how the receiver changes direction and handles the ball.

| Athletic test | What it helps assess |
|---|---|
| 40-yard dash | Straight-line acceleration and speed; lower times are better |
| Vertical jump | Upward explosiveness |
| Broad jump | Horizontal explosiveness from a standing start |
| Three-cone drill | Ability to change direction while moving around a set course |
| 20-yard shuttle | Short-distance acceleration, braking, and change of direction |
| Bench press | Upper-body muscular endurance, measured in repetitions at 225 pounds |

**4. A player can attend without doing every drill.** Some prospects choose to skip individual tests or the entire on-field workout. Others may miss a test because of an injury. A player can still meet with teams in Indianapolis and work out later at a college **pro day**, a separate event hosted by the school. For an example relevant to offensive playmakers, wide receiver Malik Nabers skipped the 40-yard dash at the 2024 Combine and ran it later at LSU’s pro day. His lack of a Combine time did not mean scouts had no opportunity to see him run.

**5. Teams combine the results with their existing scouting work.** Clubs use the event to check what they have seen on college game film and investigate questions they still have about a player. A fast 40 may reinforce what scouts already saw from a wide receiver, while the medical exam or interview may answer a different question altogether. The Combine does not produce a league-wide score that determines draft order. Teams weigh the evidence alongside college performance, film, and other pre-draft evaluations.

###*What kind of data would be required to make an analyis on this question?*
To answer this question, I would need data on **NFL Combine performance, drill participation, and NFL production** for the same players.

The Combine data would need to include each player’s name, college, position, and Combine year, along with their results in drills such as the 40-yard dash, vertical jump, and broad jump. It would also need to show when a player **did not participate (`DNP`)** in a drill, since that is different from having a recorded score or a missing record. If I wanted to identify players who *chose* to skip a drill, I would need a separate source that confirms their reason.

 The NFL data would need to include each player’s entry year and regular-season statistics for the **first five seasons after entering the league**. I would use rushing and receiving yards to measure offensive production, with touchdowns as an additional measure. I would also need a reliable way to match players across the two datasets, ideally through a shared player ID or a verified match using details such as name, college, and year.

Finally, I would need enough information to distinguish a player who **recorded zero production** from one whose NFL record **could not be found or matched**. Otherwise, treating every unmatched player as having zero production could distort the results.

**DATA SOURCES:**
>https://www.kaggle.com/datasets/philiphyde1/nfl-stats-1999-2022

>https://www.kaggle.com/datasets/isaaksipple/nfl-combine-2000-2025

###*What specific group of players would this project be able to analyse?*

>This project would analyse **running backs (RBs), wide receivers (WRs), and tight ends (TEs) listed in the NFL Combine dataset** who entered the NFL between **2012 and 2021**. The NFL statistics end in 2025, so these entry years give each player a full five-season observation window, even if they did not play in all five seasons. For each drill, the project would include players with recorded results and examine those marked `DNP` separately.

I would focus on these three positions because **scrimmage yards (rushing yards plus receiving yards)** and **rushing and receiving touchdowns** are meaningful measures of production for all three. Those measures would miss much of what quarterbacks contribute through passing or offensive linemen contribute through blocking.

I would **analyse each position separately** because players have different roles and opportunities to accumulate yards. Running backs can gain yards through carries and catches, while wide receivers and tight ends gain most of their yards through catches. Combining the groups could make a drill appear related to production simply because one position tends to score differently on that drill and gets different opportunities on the field. Separating the groups lets me ask, for example, **whether faster wide receivers tend to produce more than other wide receivers**.

###*Why Quaterbacks, Offensive & Defensive linemen, Linebackers & Defensive Backs are not accounted for*

### Why are quarterbacks, offensive and defensive linemen, linebackers, and defensive backs excluded?

This project focuses on **running backs, wide receivers, and tight ends** because the available data lets me measure a meaningful part of their offensive production using **scrimmage yards** (rushing plus receiving yards) and **rushing and receiving touchdowns**. Those measures would miss the main contributions of the other positions.

| Position group | Why it is excluded |
|---|---|
| **Quarterbacks** | The dataset includes passing statistics, but passing is a quarterback’s primary contribution. Measuring quarterbacks by rushing and receiving production would overlook most of their impact, so they would need a separate analysis using passing outcomes. |
| **Offensive linemen** | Their primary contributions are pass protection and run blocking. Yards and touchdowns do not measure that work, and the available files do not provide enough individual blocking data to evaluate it. |
| **Defensive linemen** | Their contributions include pressuring quarterbacks and stopping the run. Evaluating them would require defensive measures such as pressures, sacks, and tackles for loss. |
| **Linebackers** | Their responsibilities can include run defence, pass coverage, and rushing the passer. Evaluating them would require defensive data that reflects those different roles. |
| **Defensive backs** | Their primary contribution is pass coverage. Measures such as targets and completions allowed, passes defended, and interceptions would be more relevant than offensive production. |

The Combine dataset contains players from these positions, but **having their drill results is only half of the analysis**. I also need NFL outcomes that reflect what each position does on the field. For the three groups included, scrimmage yards and touchdowns give me a consistent measure of **production with the ball**. They do not measure every contribution. For example, they do not capture a tight end’s blocking—so the findings will apply to that specific part of performance.

The same reason applies to special teams players **kickers, punters, holders, Long snappers.** Although due to gameday squad size restictions most NFL teams dont necessarily have dedicated Kickers and Punters on active game day rosters and usually delegate these tasks to WR3/WR4 to perform return actions.