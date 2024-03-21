# The Battle for Carry: An Analysis of ADC vs Mid Players and Post-Game Role Prediction Model

by Katelyn Villamin (svillamin@ucsd.edu) & Nanch Shen (nshen@ucsd.edu)

This is a project for DSC80 at the University of California, San Diego. We performed Data Cleaning and Exploratory Data Analysis, Assessments for Missingness, Hypothesis Testing, and Prediction Modeling. This website stands as a report of our findings.


---

## Introduction

Our dataset is on the game statistics of 2022 professional League of Legends games. With a massive dataset of 75 columns and 150,000 rows, we decided to focus our research on answering the question: **Which role carries more often: Mid or ADC?** 

Although every role plays an integral part in a game's success, the overall deciding factor is the ability of the high-damage output heroes to out-damage the opponents. With differing opinions on the star of the team, fellow League of Legends players may be interested in putting a conclusion to this community debate. 

The columns that help us answer, **"Which role carries more often: Mid or ADC?"** are: `position`, `kills`, `total damage dealt`, `result`, and `first blood kill`. As doing as much damage is important for these roles, these columns showed us: Based on the game result, who ended up doing the most amount of damage. After cleaning the data, 

### Analysis Column Descriptions

| Column | Description|
| --- | --- |
| `'year'` | the year the games took place in|
| `'league'` | The league in which the players and teams play in |
| `'team name'` | The team each player plays for |
| `'position'` | The role of the player in the game |
| `'kills'` | Total number of kills a player has in a game |
| `'deaths'` | Total number of times a player dies in a game |
| `'assists'` | Total number of assits a player has in a game |
| `'doublekills'` | Total number of times a player kills an two opponents consecutively |
| `'total damage dealt'` | Total damage dealt to enemy champions by player|
| `'dpm'` | Average damage dealt to enemy champions per minute by player|
| `'total gold earned'` | Total gold earned by player in a game (excludes starting gold and inherent gold generation) |
| `'gpm'` | Average gold earned per minute by player (excludes starting gold and inherent gold generation) |
| `'first blood kill'` | Whether the player got the first kill |
| `'result'` | Whether or not the team won that game |



---

## Cleaning and Exploratory Data Analysis

### Data Cleaning
We first filtered out the columns we needed 

### Univariate Analysis

---

## Assessment of Missingness

Here's what a Markdown table looks like. Note that the code for this table was generated _automatically_ from a DataFrame, using

```py
print(counts[['Quarter', 'Count']].head().to_markdown(index=False))
```

| Quarter     |   Count |
|:------------|--------:|
| Fall 2020   |       3 |
| Winter 2021 |       2 |
| Spring 2021 |       6 |
| Summer 2021 |       4 |
| Fall 2021   |      55 |

---

## Hypothesis Testing


---
