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

| league   | position   | champion   |   kills |   doublekills |   total damage dealt |     dtpm | first blood kill   |   wards placed |   exp at 10 |   towers | result   |
|:---------|:-----------|:-----------|--------:|--------------:|---------------------:|---------:|:-------------------|---------------:|------------:|---------:|:---------|
| LCKC     | top        | Renekton   |       2 |             0 |                15768 | 1072.4   | False              |              8 |        4909 |      nan | False    |
| LCKC     | jng        | Xin Zhao   |       2 |             0 |                11765 |  944.273 | False              |              6 |        3484 |      nan | False    |
| LCKC     | mid        | LeBlanc    |       2 |             0 |                14258 |  581.646 | False              |             19 |        4556 |      nan | False    |
| LCKC     | adc        | Samira     |       2 |             0 |                11106 |  463.853 | False              |             12 |        3103 |      nan | False    |
| LCKC     | sup        | Leona      |       1 |             0 |                 3663 |  475.026 | True               |             29 |        2161 |      nan | False    |

As our analysis is only interested in the positions mid and adc, we decided to create a subdataframe of the original `league` dataframe called `mid_adc` for our **analysis**. We filtered the `position` to only include rows where the position is "mid" or "adc". 

Below is the head of our DataFrame `mid_adc`:

| league   | position   |   kills |   doublekills |   total damage dealt | first blood kill   |   towers | result   |
|:---------|:-----------|--------:|--------------:|---------------------:|:-------------------|---------:|:---------|
| LCKC     | mid        |       2 |             0 |                14258 | False              |      nan | False    |
| LCKC     | adc        |       2 |             0 |                11106 | False              |      nan | False    |
| LCKC     | mid        |       6 |             2 |                20690 | False              |      nan | True     |
| LCKC     | adc        |       8 |             3 |                26687 | False              |      nan | True     |
| LCKC     | mid        |       2 |             0 |                23082 | False              |      nan | False    |




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

|   year |   kills |   deaths |   assists |   doublekills |   total damage dealt |     dpm |   total gold earned |     gpm |   first blood kill |   dragons |
|-------:|--------:|---------:|----------:|--------------:|---------------------:|--------:|--------------------:|--------:|-------------------:|----------:|
|   2022 | 2.57627 |  3.50684 |   3.38512 |      0.27522  |              15940.8 | 488.548 |             8264.87 | 257.337 |          0.0904797 |       nan |
|   2022 | 5.93634 |  1.57621 |   7.33759 |      1.04109  |              20146.4 | 631.253 |            10769.4  | 342.727 |          0.142976  |       nan |
|   2022 | 2.24774 |  3.60916 |   3.65134 |      0.181854 |              15630.7 | 483.95  |             7407.34 | 231.989 |          0.0707598 |       nan |
|   2022 | 4.73999 |  1.68678 |   8.12532 |      0.644739 |              19188.5 | 603.827 |             9504.98 | 302.957 |          0.106589  |       nan |


---

## Assessment of Missingness
*State whether you believe there is a column in your dataset that is NMAR. Explain your reasoning and any additional data you might want to obtain that could explain the missingness (thereby making it MAR). Make sure to explicitly use the term “NMAR.”*

*Present and interpret the results of your missingness permutation tests with respect to your data and question. Embed a plotly plot related to your missingness exploration; ideas include: The distribution of column  Y when column X is missing and the distribution of column Y when column X is not missing, as was done in Lecture 8. The empirical distribution of the test statistic used in one of your permutation tests, along with the observed statistic.*

MAR Pivoted Table:

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

MCAR Pivoted Table:

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

<iframe
  src="assets/mcar.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---

## Hypothesis Testing
*Clearly state your null and alternative hypotheses, your choice of test statistic and significance level, the resulting p-value, and your conclusion. Justify why these choices are good choices for answering the question you are trying to answer. Optional: Embed a visualization related to your hypothesis test in your website.Tip: When making writing your conclusions to the statistical tests in this project, never use language that implies an absolute conclusion; since we are performing statistical tests and not randomized controlled trials, we cannot prove that either hypothesis is 100% true or false.*

---

## Framing a Prediction Problem
*Our prediction problem is: Given a dataset of post-game stats, we want to predict which role did they play. To predict the response variable (position), we will be using a multiclass classification. We chose the position as the response variable because there are clear defined criterias for post game statistics for us to draw our model from.*

To evaluate our model, we chose precision. As we are predicting categorical variables using `RandomForestClassifier`, we cannot use accuracy or F1-score. Furthermore, weighing the trade-off between recall and precision, we believe that it is better to have a 

---

## Baseline Model
*Describe your model and state the features in your model, including how many are quantitative, ordinal, and nominal, and how you performed any necessary encodings. Report the performance of your model and whether or not you believe your current model is “good” and why.*

*Tip: Make sure to hit all of the points above: many projects in the past have lost points for not doing so.*

---

## Final Model
*State the features you added and why they are good for the data and prediction task. Note that you can’t simply state “these features improved my accuracy”, since you’d need to choose these features and fit a model before noticing that – instead, talk about why you believe these features improved your model’s performance from the perspective of the data generating process. Describe the modeling algorithm you chose, the hyperparameters that ended up performing the best, and the method you used to select hyperparameters and your overall model. Describe how your Final Model’s performance is an improvement over your Baseline Model’s performance. Optional: Include a visualization that describes your model’s performance, e.g. a confusion matrix, if applicable.*

---

## Fairness Analysis
*Clearly state your choice of Group X and Group Y, your evaluation metric, your null and alternative hypotheses, your choice of test statistic and significance level, the resulting p-value, and your conclusion. Optional: Embed a visualization related to your permutation test in your website.*

---