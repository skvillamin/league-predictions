# The Battle for Carry: An Analysis of ADC vs Mid Players and Post-Game Role Predictive Model

by Katelyn Villamin (svillamin@ucsd.edu) & Nanch Shen (nshen@ucsd.edu)

This is a project for DSC80 at the University of California, San Diego. We performed Data Cleaning and Exploratory Data Analysis, Assessments for Missingness, Hypothesis Testing, and Predictive Model. This website stands as a report of our findings.


---

## Introduction

Our dataset is on the game statistics of 2022 professional League of Legends games. With a massive dataset of over 150,000 rows, we decided to focus our research on answering the question: **Which role carries more often: Mid or ADC?** 

Although every role plays an integral part in a game's success, the overall deciding factor is the ability of the high-damage output heroes to out-damage the opponents. With differing opinions on the star of the team, fellow League of Legends players may be interested in putting a conclusion to this community debate. 

The columns that help us answer, **"Which role carries more often: Mid or ADC?"** are: `position`, `kills`, `total damage dealt`, `result`, and `first blood kill`. As doing as much damage is important for these roles, these columns showed us: Based on the game result, who ended up doing the most amount of damage. After cleaning the data, 

### Analysis Column Descriptions

| Column | Description|
| --- | --- |
| `'league'` | The league in which the players and teams play in |
| `'position'` | The role of the player in the game |
| `'champion'` | The hero of the player in the game |
| `'kills'` | Total number of kills a player has in a game |
| `'doublekills'` | Total number of times a player kills an two opponents consecutively |
| `'total damage dealt'` | Total damage dealt to enemy champions by player|
| `'dtpm'` | Average damage taken from enemy champions per minute by player|
| `'first blood kill'` | Whether the player got the first kill |
| `'wards placed'` | The number of wards placed by player|
| `'towers'` | The total number of towers taken by a team|
| `'exp at 10'` | The experience level gained by a player at minute-10|
| `'result'` | Whether or not the team won that game |

| league   | position   | champion   |   kills |   doublekills |   total damage dealt |     dtpm | first blood kill   |   wards placed |   exp at 10 |   towers | result   |
|:---------|:-----------|:-----------|--------:|--------------:|---------------------:|---------:|:-------------------|---------------:|------------:|---------:|:---------|
| LCKC     | top        | Renekton   |       2 |             0 |                15768 | 1072.4   | False              |              8 |        4909 |      nan | False    |
| LCKC     | jng        | Xin Zhao   |       2 |             0 |                11765 |  944.273 | False              |              6 |        3484 |      nan | False    |
| LCKC     | mid        | LeBlanc    |       2 |             0 |                14258 |  581.646 | False              |             19 |        4556 |      nan | False    |
| LCKC     | adc        | Samira     |       2 |             0 |                11106 |  463.853 | False              |             12 |        3103 |      nan | False    |
| LCKC     | sup        | Leona      |       1 |             0 |                 3663 |  475.026 | True               |             29 |        2161 |      nan | False    |


---

## Cleaning and Exploratory Data Analysis

### Data Cleaning
We first filtered out the columns we needed 

### Univariate Analysis

### Bivariate Analysis

### Interesting Aggregates
This table shows the mean statistics for ADC and mid players in dataset based on the result of the game (where False means that they lost, and True means that they won). Finding the means of these columns allows us to see which role carries more often.

|   year |   kills |   deaths |   assists |   doublekills |   total damage dealt |     dpm |   total gold earned |     gpm |   first blood kill |   dragons |
|-------:|--------:|---------:|----------:|--------------:|---------------------:|--------:|--------------------:|--------:|-------------------:|----------:|
|   2022 | 2.57627 |  3.50684 |   3.38512 |      0.27522  |              15940.8 | 488.548 |             8264.87 | 257.337 |          0.0904797 |       nan |
|   2022 | 5.93634 |  1.57621 |   7.33759 |      1.04109  |              20146.4 | 631.253 |            10769.4  | 342.727 |          0.142976  |       nan |
|   2022 | 2.24774 |  3.60916 |   3.65134 |      0.181854 |              15630.7 | 483.95  |             7407.34 | 231.989 |          0.0707598 |       nan |
|   2022 | 4.73999 |  1.68678 |   8.12532 |      0.644739 |              19188.5 | 603.827 |             9504.98 | 302.957 |          0.106589  |       nan |


---

## Assessment of Missingness
We believe that the column `dtpm` is NMAR. 
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

## Framing a Prediction Problem
Our prediction problem is: Given a dataset of post-game stats, we want to predict which role did they play. To predict the response variable (position), we will be using a multiclass classification. We chose the position as the response variable because there are clear defined criterias for post game statistics for us to draw our model from. 

To evaluate our model, we chose precision. As we are predicting categorical variables using `RandomForestClassifier`, we cannot use accuracy or F1-score. Furthermore, weighing the trade-off between recall and precision, we believe that it is better to have a 

---

## Framing a Prediction Problem


---

## Baseline Model


---

## Final Model


---

## Fairness Analysis


---