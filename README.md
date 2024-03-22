# The Battle for Carry: An Analysis of ADC vs Mid Players and Post-Game Role Predictive Model

by Katelyn Villamin (svillamin@ucsd.edu) & Nanch Shen (nshen@ucsd.edu)

This is a project for DSC80 at the University of California, San Diego. We performed Data Cleaning and Exploratory Data Analysis, Assessments for Missingness, Hypothesis Testing, and Predictive Model. This website stands as a report of our findings.


---

## Introduction

Our dataset is on the game statistics of 2022 professional League of Legends games. With a massive dataset of over 150,000 rows, we decided to focus our research on answering the question: **Which role carries more often: Mid or ADC?** 

Although every role plays an integral part in a game's success, the overall deciding factor is the ability of the high-damage output heroes to out-damage the opponents. With differing opinions on the star of the team, fellow League of Legends players may be interested in putting a conclusion to this community debate. 

The columns that help us answer, **"Which role carries more often: Mid or ADC?"** are: `position`, `kills`, `total damage dealt`, `result`, and `first blood kill`. As doing as much damage is important for these roles, these columns showed us: Based on the game result, who ended up doing the most amount of damage. After cleaning the data, we narrowed it to 12 columns and 144804 rows for our analysis' DataFrame `league` and 8 columns and 48268 rows for our predictive model's DataFrame `mid_adc`, a subDataFrame of `league`.

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
| `'dmpm'` | Average damage mitigated from enemy champions per minute by player|
| `'first blood kill'` | Whether the player got the first kill |
| `'monster kills'` | The number of monsters a player killed in a game |
| `'wards placed'` | The number of wards placed by player|
| `'towers'` | The total number of towers taken by a team|
| `'exp at 10'` | The experience level gained by a player at minute-10|
| `'result'` | Whether or not the team won that game |




---

## Cleaning and Exploratory Data Analysis

### Data Cleaning
We first isolated columns we would need for our analysis and predictive modeling, keeping only relevant rows. Since we noticed that some of the 2022 data had overlaps to 2023, we filtered out for 2022 only before dropping the `year` column as it was not relevant to our question or predictive model. 

For clarity, we decided changed all the `position` values of 'bot' with 'adc'. We also decided to rename the columns for more appropriate labels:

```py league.rename(columns={
    "damagetochampions": "total damage dealt", 
    "firstbloodkill": "first blood kill",
    'wardsplaced': 'wards placed', 
    'monsterkills': 'monster kills', 
    'damagetakenperminute': 'dtpm',
    'damagemitigatedperminute': 'dmpm',
    'xpat10' : 'exp at 10'
    }, inplace=True)
```

Finally we changed the values of `result` and `first blood kill` into boolean values.

Below is the head of our DataFrame `league`. We used this dataframe for our **predictive model**:

| league   | position   | champion   |   kills |   doublekills |   total damage dealt |     dtpm |    dmpm | first blood kill   |   monster kills |   wards placed |   exp at 10 |   towers | result   |
|:---------|:-----------|:-----------|--------:|--------------:|---------------------:|---------:|--------:|:-------------------|----------------:|---------------:|------------:|---------:|:---------|
| LCKC     | top        | Renekton   |       2 |             0 |                15768 | 1072.4   | 777.793 | False              |              11 |              8 |        4909 |      nan | False    |
| LCKC     | jng        | Xin Zhao   |       2 |             0 |                11765 |  944.273 | 650.158 | False              |             115 |              6 |        3484 |      nan | False    |
| LCKC     | mid        | LeBlanc    |       2 |             0 |                14258 |  581.646 | 227.776 | False              |              16 |             19 |        4556 |      nan | False    |
| LCKC     | adc        | Samira     |       2 |             0 |                11106 |  463.853 | 218.879 | False              |              18 |             12 |        3103 |      nan | False    |
| LCKC     | sup        | Leona      |       1 |             0 |                 3663 |  475.026 | 490.123 | True               |               0 |             29 |        2161 |      nan | False    |

As our analysis is only interested in the positions mid and adc, we decided to create a subdataframe of the original `league` dataframe called `mid_adc` for our **analysis**. We filtered the `position` to only include rows where the position is "mid" or "adc". 

Below is the head of our DataFrame `mid_adc`:

| league   | position   |   kills |   doublekills |   total damage dealt |    dtpm |    dmpm | first blood kill   |   towers | result   |
|:---------|:-----------|--------:|--------------:|---------------------:|--------:|--------:|:-------------------|---------:|:---------|
| LCKC     | mid        |       2 |             0 |                14258 | 581.646 | 227.776 | False              |      nan | False    |
| LCKC     | adc        |       2 |             0 |                11106 | 463.853 | 218.879 | False              |      nan | False    |
| LCKC     | mid        |       6 |             2 |                20690 | 531.629 | 426.935 | False              |      nan | True     |
| LCKC     | adc        |       8 |             3 |                26687 | 385.009 | 186.655 | False              |      nan | True     |
| LCKC     | mid        |       2 |             0 |                23082 | 395.449 | 366.698 | False              |      nan | False    |





### Univariate Analysis
Our plot shows the distribution of kills for ADC versus mid players. Here we see that both graphs follow a similar curve, but more mid players on average have 5 kills in a game, but more ADC players have more than 5 kills in a game.

<iframe
  src="assets/univariate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis
This plot shows the distribution of a player getting first blood kill, given their position is mid or ADC. First Blood Kill is defined as the first kill a team scores within a match, giving a great early advantage to the successful team.

<iframe
  src="assets/bivariate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Interesting Aggregates
This table shows the mean statistics for ADC and mid players in dataset based on the result of the game (where False means that they lost, and True means that they won). Finding the means of these columns allows us to see which role carries more often.

|   kills |   doublekills |   total damage dealt |    dtpm |    dmpm |   first blood kill |
|--------:|--------------:|---------------------:|--------:|--------:|-------------------:|
| 2.57627 |      0.27522  |              15940.8 | 451.842 | 284.466 |          0.0904797 |
| 5.93634 |      1.04109  |              20146.4 | 390.162 | 277.94  |          0.142976  |
| 2.24774 |      0.181854 |              15630.7 | 552.9   | 372.75  |          0.0707598 |
| 4.73999 |      0.644739 |              19188.5 | 491.346 | 367.038 |          0.106589  |





---

## Assessment of Missingness

### NMAR Analysis
Yes, there are a lot of columns in our dataset that is Not Missing at Random (NMAR) due to the data collection. Specifically for the ones relevant to our question, we saw that `dmpm` (Damage Mitigated per Minute) may be NMAR. For this data, only certain leagues tracked data on the damage mitigated per minute, while others did. To make this column Missing at Random (MAR) dependent, we may need to obtain `total damage mitigated`, making it possible for us to compute the `dmpm`.  As such, we speculate that it is NMAR dependent on  the `league` column.

### Missingness Dependency

Here, We wanted to determine if `league` and `doublekills` were Missing at Random or Missing Completely at Random.

Here is the observed distribution when `doublekills` is missing and not missing:

| league     |   double_missing = True |   double_missing= False |
|:-----------|------------------------:|------------------------:|
| CBLOL      |             0.0162841   |             0.0193577   |
| CBLOLA     |             0.0152275   |             0.0181017   |
| CDF        |             0.00673324  |             0.00800414  |
| CT         |             0.00188531  |             0.00224116  |
| DCup       |             0           |             0.0055167   |
| DDH        |             0.0158283   |             0.0188159   |
| EBL        |             0.012472    |             0.0148261   |
| EL         |             0.00950941  |             0.0113043   |
| ESLOL      |             0.0154347   |             0.0183479   |
| EUM        |             0.0181694   |             0.0215989   |
| GL         |             0.0121613   |             0.0144567   |
| GLL        |             0.014088    |             0.0167471   |
| HC         |             0.0126378   |             0.0150232   |
| HM         |             0.0109182   |             0.012979    |
| IC         |             0.00540731  |             0.00642794  |
| LAS        |             0.0184594   |             0.0219437   |
| LCK        |             0.0255656   |             0.0303911   |
| LCKC       |             0.0280517   |             0.0333465   |
| LCL        |             0.000745836 |             0.000886612 |
| LCO        |             0.0151239   |             0.0179785   |
| LCS        |             0.0158076   |             0.0187913   |
| LCSA       |             0.0348264   |             0.0413999   |
| LDL        |             0           |             0.0730716   |
| LEC        |             0.0146681   |             0.0174367   |
| LFL        |             0.0139015   |             0.0165255   |
| LFL2       |             0.0156626   |             0.0186189   |
| LHE        |             0.016367    |             0.0194562   |
| LJL        |             0.0134872   |             0.0160329   |
| LJLA       |             0.0028176   |             0.00334942  |
| LLA        |             0.0113533   |             0.0134962   |
| LMF        |             0.0199511   |             0.0237169   |
| LPL        |             0           |             0.0665698   |
| LPLOL      |             0.0145024   |             0.0172397   |
| LVP SL     |             0.0139844   |             0.016624    |
| MSI        |             0.00505511  |             0.00600926  |
| NEXO       |             0.00789343  |             0.00938331  |
| NLC        |             0.0268915   |             0.0319673   |
| PCS        |             0.0184802   |             0.0219683   |
| PGC        |             0.0456203   |             0.0542311   |
| PGN        |             0.00878429  |             0.0104423   |
| PRM        |             0.0259592   |             0.030859    |
| SL (LATAM) |             0.0118919   |             0.0141365   |
| TAL        |             0.0143366   |             0.0170427   |
| TCL        |             0.0152275   |             0.0181017   |
| UL         |             0.015186    |             0.0180524   |
| UPL        |             0.0375404   |             0.0446261   |
| VCS        |             0.0233281   |             0.0277313   |
| VL         |             0.0113533   |             0.0134962   |
| WLDs       |             0.00836993  |             0.0113289   |


<iframe
  src="assets/mar.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Observed Statistic: 0.0030311228214282494
P-Value: 1.0
Conclusion: We reject the null at 5% significance. **Missingness of `doublekills` does depend on `towers`. (Missing Completely at Random)**

Here, We wanted to determine if `towers` and `doublekills` were Missing at Random or Missing Completely at Random.

Here is the observed distribution when `doublekills` is missing and not missing:

|   towers |   double_missing = False |   double_missing = True |
|---------:|-------------------------:|------------------------:|
|        0 |                0.0470336 |               0.0439802 |
|        1 |                0.0824551 |               0.0852117 |
|        2 |                0.111729  |               0.114349  |
|        3 |                0.108655  |               0.117372  |
|        4 |                0.0631343 |               0.0629467 |
|        5 |                0.0373731 |               0.0351842 |
|        6 |                0.0281518 |               0.0241891 |
|        7 |                0.0617681 |               0.0538758 |
|        8 |                0.0912861 |               0.0940077 |
|        9 |                0.13832   |               0.153106  |
|       10 |                0.124415  |               0.127543  |
|       11 |                0.105679  |               0.0882353 |

Observed Statistic: 0.03472890994914897

P-Value: 0.0195

Conclusion: We fail to reject the null at 5% significance. **Missingness of `doublekills` does not depend on `towers`. (Missing Completely at Random)**

<iframe
  src="assets/mcar.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>



---

## Hypothesis Testing

Null Hypothesis (H0): The distribution of games won where ADC players had more damage on average than mid players is equal to the distribution of games won where mid players had more damage than ADC players

Alternative Hypothesis (H1): The distribution of games won where ADC players had more damage on average than mid players is not equal to the distribution of games won where mid players had more damage than ADC players

Since we are looking at two samples from the same population, we decided to conduct a permutation test at 5% significance level as it is typically a standard metric for testing. We chose to test this through *difference in group means* as both distributions have the same center and similar shapes.

Observed Difference: 545.821638930971

P-Value: 0.0006

Conclusion: We reject the null at 5% significance level.  

<iframe
  src="assets/hypotest.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>



---

## Framing a Prediction Problem
Our prediction problem is: *Given a dataset of post-game stats, we want to predict which role did they play.* To predict the response variable (`position`), we will be using a *multiclass classification.* We chose the position as the response variable because there are clear defined criterias for post game statistics for us to draw our model from.

To evaluate our model, we chose precision. As we are predicting non-binary classification using `RandomForestClassifier`, we cannot use accuracy or F1-score. We believe precision is a better metric 

---

## Baseline Model

For our baseline model, we first took the `league` DataFrame and narrowed it down to only rows where `position` was not `team`. We decided to narrow our focus only to two features: `champion` (nominal) and `kills` (quantitative). As `champion` has no inherent ordering, we decided to use `OneHotEncoder` to turn it into quantitative columns. One-Hot Encoding the champion allows the model to capture the association between certain champions and their corresponding roles. Finally, we tried using `LogisticRegression(multi_class= 'multinomial')` to predict `position`. 

Our model performed with a precision score of: 0.0.9334108441113438
We think that while this current model is not good as it fell short in a couple of points:

- It relied on too few features and found difficulty in finding the difference between "mid" and "ADC" players, positions in which they often had interchangeable champions and similar number of kills.

- While LogisticRegression() had a high accuracy, it was way slower than it needed to be.

- Finally, as we had not done a k-fold validation, the data seemed almost too precise, being only trained for the seen data. 



---

## Final Model

To improve upon our baseline moel,  we decided to add a few more features that were a greater indicator on certain roles: `wards placed`, `monster kills`, `dtpm`, and `exp at 10`. Below are the indicators for each position:

| `position` | Description|
| --- | --- |
| adc | ADC players often have high total damage dealt, high number of kills, but low number of exp at 10, as they share it with the support. |
| mid | Mid players often have high total damage dealt, high number of kills, and high number of exp at 10.|
| top | Top, being the defensive hero, often have the highest dtpm. |
| jng | Junglers often have the highest number of monster kills. |
| sup | Supports often have low total damage dealt, low dtpm, and the highest number of wards placed. |

We believe that these added features improved the model's performance significantly because:


- Binarization of 'Monster Kills': Binarizing this feature can help distinguish between players who primarily jungle in camps versus those who focus on other aspects of the game, aiding in position prediction.

- Transformation of 'Wards Placed' by finding Max Wards Placed: This function transformer used a custom function `max_of_five` which finds the players with the most number of wards placed for each team, with the player at max being (1) and else (0) This allows the model to identify the support players who fulfill these roles based on their warding behavior. 

- Discretizing Exp at 10: By discretizing experience at 10 minutes into bins, the model can capture role-specific patterns in player progression and identify players who exhibit behavior consistent with certain roles. This allows us to differentiate between ADC and mid, who are very similar in statistics, as ADCs lane with a support, while mid players gain solo-experience. 

As we were performing a multiclass classification which was not linear, we decided on using `RandomForest Classifier` instead of `LogisticRegression`. Here, it performed faster than `LogisticRegression` with a better prediction score. To select the best hyperparameters for our modeling algorithm, we decided to tune `max-depth` and `criterion`. Having no maximum depth of trees may lead to worse results. On the other hand, choosing the criterion, we face a trade off between using Gini vs. Entropy, with the expenses of computation and results. To find this, we used `GridSearchCV` and found that a max depth of 2 and using gini would give us the best overall results.

Our model performed with a precision score of: 

---

## Fairness Analysis

To test for fairness, we chose to perform a permutation test on two individual groups we defined below:
- carries: positions with more than 3 kills
- non-carries: positions with less than 3 kills

We chose 3 as our threshold as that is the mean number of kills a player gets in a game. From here, we created a new binary column which sorts the rows based on whether they are considered a carry vs. a non-carry. 

Null Hypothesis (H0): Our model is fair. Its precision for carries and non-carries are roughly the same, and any differences are due to random chance.

Alternative Hypothesis (H1): Our model is unfair. Its precision for carries is lower than its precision for non-carries.

Similar to our reasons in Framing the Prediction Problem, We chose precision as our evaluatoin metric. We are predicting non-binary classification using `RandomForestClassifier`, we cannot use accuracy or F1-score. We believe precision is a better metric Furthermore, weighing the trade-off between recall and precision, we believe that having a non-carry labeled as a carry (False Positive) is worse than having a carry labeled as a non-carry (False Negative).

We chose difference in precision as our test statistic as we are trying to find the difference in distribution between carries vs non carries.

Observed: -0.009959146629159088

P-Value: 1.0

Conclusion: We reject the null at 5% significance level. Our model is unfair and does not achieve precision parity.

<iframe
  src="assets/fairness.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


---