### The Pre-Snap Puzzle: Attached TE's Route or Block?

[Mike Meissner](https://www.linkedin.com/in/mcm1020/) | Metric Track

---

# Introduction

> "The offensive coordinators didn't even really understand the weapons that they had because they were just so brainwashed into thinking, 'Oh, they're just a glorified offensive lineman.' When I got in the league [in 2003], it was a lot of bigger guys that could go catch a 5-yard stick route and get about 45 catches a season. That was a Pro Bowl season back then." - *Former All-Pro TE Dallas Clark, 2024*

The Tight End position in the NFL has significantly evolved over the years. Names such as Kelce, Kittle, Gates, Gonzalez and Gronkowski are synonymous with scoring touchdowns and being do-it-all receivers and run blockers. Modern tight ends can be found split out wide or in the slot, creating mismatches against smaller defensive backs and slower linebackers.

Yet in today's NFL, the attached tight end - lined up directly next to an offensive tackle - remains one of the offense's most versatile weapons. In a single snap, they can either:
- Provide crucial pass protection
- Create an extra gap in the run game
- Release into a pass route, often catching defenses off guard

For defensive coordinators, the question isn't just "Will they block or run a route?" It's about allocating defensive resources in real-time. Commit an extra defender to rush, and you risk leaving that TE uncovered on a pass route. Drop into coverage, and you might give the offense a crucial blocking advantage.

The examples below showcase this pre-snap dilemma. From identical formations, the attached tight end Tucker Kraft for the Packers executes two completely different assignments:


<div align="center">
  <p><strong>Attached TE (Tucker Kraft #85) Blocking</strong></p>
  <img src="https://github.com/mikemeissner1/NFL-Big-Data-Bowl-2025/blob/main/clip1_packers-ezgif.com-cut.gif?raw=true" width="800" />
  
  <p style="margin-top: 20px;"><strong>Same Formation, TE Releases into Crossing Route</strong></p>
  <img src="https://github.com/mikemeissner1/NFL-Big-Data-Bowl-2025/blob/main/clip2_packers-ezgif.com-video-to-gif-converter.gif?raw=true" width="800" />
</div>


This analysis of NFL tracking data aims to solve this pre-snap puzzle that defensive coordinators face on every play. By examining over 4,000 instances of attached tight ends during the first nine weeks of the 2022 NFL season, we sought to identify the key factors that influence whether a tight end will stay in to block or release into a route. Using precise measurements from player tracking data combined with formation, situation, and player-specific features, we developed a Logistic Regression and RandomForest model that can help predict these pre-snap decisions.

# Defining the "Attached" Tight End

A crucial first step in the analysis was precisely defining what makes a tight end "attached" to the formation. While football observers can easily identify this alignment visually, creating a consistent, data-driven definition required careful consideration of both distance and alignment relative to the offensive tackles.

Using NFL tracking data's precise x,y coordinates, attached tight ends were identified based on two key measurements:
- Within 1.75 yards lateral distance from the nearest tackle
- Within 0.35 yards depth from the nearest tackle

These thresholds proved effective at capturing true attached alignments while excluding tight ends in wing or flexed positions.

<div align="center">
  <h3>Field Diagram with Attached TE</h3>
  <img src="https://github.com/mikemeissner1/NFL-Big-Data-Bowl-2025/blob/main/field.png?raw=true" width="1000" />
  
  <h3 style="margin-top: 40px;">Field Diagram with Split TE</h3>
  <img src="https://github.com/mikemeissner1/NFL-Big-Data-Bowl-2025/blob/main/field_2.png?raw=true" width="1000" />
</div>

Applying these criteria across the dataset revealed interesting patterns in how teams deploy their tight ends. Below are the NFL's most frequently "attached" tight ends from this analysis period, showing the number of snaps where they lined up in this traditional alignment:



| **Player Name**                  | **Team** | **Attached Snaps** |
|------------------------------|:------------:| ------------------- |
| Geoff Swaim                    | <span style="color:blue;font-weight:bold;">Tennessee Titans</span> | 141
| Eric Tomlinson            | <span style="color:orange;font-weight:bold;">Denver Broncos</span> | 134
| Jake Ferguson         | <span style="color:silver;font-weight:bold;">Dallas Cowboys</span> | 113
| Tyler Higbee       | <span style="color:blue;font-weight:bold;">Los Angeles Rams</span> | 112
| Dallas Goedert       | <span style="color:green;font-weight:bold;">Philadelphia Eagles</span> | 108
| Josh Oliver | <span style="color:purple;font-weight:bold;">Baltimore Ravens</span> | 106
| O.J. Howard    | <span style="color:red;font-weight:bold;">Houston Texans</span> | 102
| Ian Thomas                | <span style="color:lightblue;font-weight:bold;">Carolina Panthers</span> | 96
| Tyler Conklin                   | <span style="color:darkgreen;font-weight:bold;">New York Jets</span> | 93
| Cole Kmet  | <span style="color:navy;font-weight:bold;">Chicago Bears</span> | 92
| Adam Trautman  | <span style="color:black;font-weight:bold;">New Orleans Saints</span> | 83
| Parker Hesse   | <span style="color:red;font-weight:bold;">Atlanta Falcons</span> | 83
| Dalton Schultz  | <span style="color:silver;font-weight:bold;">Dallas Cowboys</span> | 81
| Johnny Mundt  | <span style="color:purple;font-weight:bold;">Minnesota Vikings</span> | 77
| Chris Manhertz  | <span style="color:teal;font-weight:bold;">Jacksonville Jaguars</span> | 72


This measurement approach provided a consistent framework for identifying these alignments across different formations and game situations, establishing a solid foundation for analyzing pre-snap behavior patterns.

# What Tells The Story?

While the ability to identify attached tight ends provided the foundation, predicting their behavior required understanding a complex web of factors. Through careful feature engineering and analysis, the analysis revealed several key elements that influence whether an attached tight end stays in to block or releases into a route:

## Formation and Play Design: The Strongest Signals

<div align="center">
  <img src="https://github.com/mikemeissner1/NFL-Big-Data-Bowl-2025/blob/main/formation.png?raw=true" width="1000" />

Formation choice proved to be highly indicative of tight end behavior. Empty formations saw tight ends running routes 67.5% of the time, while in jumbo packages they stayed in to block on 87.3% of plays. 

Perhaps most telling was the impact of play action:

  <img src="https://github.com/mikemeissner1/NFL-Big-Data-Bowl-2025/blob/main/playaction.png?raw=true" width="1000" />
</div>

On play action plays, attached tight ends ran routes nearly half the time (46.7%), compared to just 13.8% on non-play action plays.

## Game Situation Impact

Down and distance showed clear patterns:
- Early downs saw more blocking (80.1% on first down)
- Third downs increased route probability (21.4%)
- Fourth downs heavily favored blocking (85.9%)

The score situation also influenced tight end usage:
- Teams protecting leads blocked more frequently (81.8% when up by multiple scores)
- Teams trailing showed higher route percentages (23.4% when down big)

## Pre-Snap Indicators

Motion proved to be another key tell:
- Static tight ends: 21.2% route probability
- Tight ends in motion: 40% route probability

These patterns demonstrate how offensive coordinators use attached tight ends differently based on situation and scheme, which is all apart of the pre-snap puzzle defensive coordinators are trying so hard to solve for.

# The Prediction Model

The approach to predicting attached tight end behavior progressed methodically from a simple baseline model to a sophisticated solution that could help inform defensive decision-making. 

## Starting Simple: The Baseline Model
Beginning with logistic regression, which provided clear insights into basic relationships in our data. This baseline model achieved 72% overall accuracy with particularly strong performance identifying routes (85% recall). Key formation features showed strong predictive power:
- Empty formations strongly indicated routes (coefficient: 1.86)
- Play action plays suggested route likelihood (coefficient: 0.96)
- Shotgun formations hinted at routes (coefficient: 0.81)

## Building a More Sophisticated Solution
Building on these insights, I developed a random forest model that could better capture the complex interactions between pre-snap features. The analysis revealed several key predictors:
- Play action was by far the strongest signal (~25% importance)
- Formation type, particularly shotgun (~10%)
- Physical alignment measurements (~7%)
- Field position (~6%)
- Player physical traits (~5.6%)

<div align="center">
  <img src="https://github.com/mikemeissner1/NFL-Big-Data-Bowl-2025/blob/main/featureimp.png?raw=true" width="1000" />
</div>

## The Power of Prediction Confidence
Perhaps the most valuable aspect of the final model was its ability to express confidence in its predictions. This proved crucial for practical application:

- On high-confidence predictions (>80% certainty):
  * 311 predictions made
  * 97.7% accuracy
- On low-confidence predictions (<60% certainty):
  * 342 predictions made
  * 58.2% accuracy

This stark difference in accuracy based on confidence level provides defensive coordinators with crucial context for their pre-snap decisions. When the model shows high confidence, its predictions can be trusted. When confidence is low, it reflects genuine pre-snap ambiguity that defenses must account for.

<div align="center">
  <img src="https://github.com/mikemeissner1/NFL-Big-Data-Bowl-2025/blob/main/conf.png?raw=true" width="1000" />
</div>

The final model achieved 79% overall accuracy, with particularly strong performance identifying blocking plays (89% recall, 85% precision). While route prediction proved more challenging (44% recall, 51% precision), this reflects the inherent difficulty of predicting when an attached tight end will break tendency and release into a route.

# Football Applications & Insights

This analysis of over 4,000 attached tight end plays has revealed clear patterns that could provide valuable pre-snap insights for defensive coordinators. While the raw probabilities of route vs. block are informative, the real value comes from understanding how different factors combine to influence tight end behavior.

## Key Pre-Snap Signals
The model identified several reliable indicators:
- Play action is the strongest tell, dramatically increasing route probability
- Formation matters significantly - empty sets suggest routes while jumbo packages favor blocks
- Motion can be revealing - tight ends in motion are nearly twice as likely to run routes
- Score and situation provide context - teams protecting leads block more frequently

## Practical Applications
For defensive coordinators, these insights could influence several key pre-snap decisions:
- Personnel matching
- Blitz timing
- Coverage assignments
- Pressure packages

## Limitations and Future Work
While the model provides valuable insights, several factors suggest room for improvement:
- The inherent imbalance between routes and blocks (21% vs 79%) reflects real football tendencies but makes it difficult for the model to predict routes as successfully
- Additional tracking data could help rectify the class imbalance and provide the model more occurrences of TE's running routes from the attached alignment
- Team-specific tendencies could be incorporated with more seasons of data

## The Pre-Snap Edge

At its core, this project tackled a specific but crucial defensive challenge: predicting whether an attached tight end will block or run a route. While this might seem like a narrow focus, it represents the type of split-second decision that can determine the success of a defensive play.

The model doesn't just provide predictions - it offers a framework for defensive preparation. When the model shows high confidence (>80%), its near-perfect accuracy can directly inform defensive play-calling. In less certain situations, the model's hesitation itself is valuable information, suggesting the need for more flexible defensive responses.

The NFL is often described as a game of inches, but it's equally a game of information. With how smart and deceptive offensive coordinators are in their play design, like was shown at the beginning of the analysis, the smallest key or tendency recognition can lead to something as large as a Super Bowl for some team. By combining player tracking data with machine learning, this analysis has demonstrated that these pre-snap puzzles, while complex, can be tackled.
