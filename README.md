<style>
  iframe, img {
    display: block;
    margin: 0 auto;
  }
</style>

# League pick-2-win

### An analysis of <u>League of Legends</u> pick-rates and win-rates, as seen in current competitive play

## Introduction

<u>League of Legends</u> is one of the most famous multiplayer online battle arena games developed by Riot Games on October 27, 2009. With close to 120 million monthly active users and 40 million daily users worldwide, the game has become one of the most dominant and enduring games in the MOBA world. We will be working with a dataset developed by Oracle’s Elixir, which contains match data from professional <u>League of Legends</u> esports games played throughout 2025. It is filled with detailed statistics from real esports games, giving us insight into how players perform, how teams strategize, and the circumstances leading to defeat or victory.
  
One of the main aspects of <u>League of Legends’</u> competitive scene is the initial ban phase. In professional <u>League of Legends</u> (as well as ordinary ranked play), the ban phase involves both teams selecting champions to be removed from the game's pool, preventing them from being picked by either side. This phase alternates between each team banning a champion, with each team having banning five champions for a total of ten character bans. 
  
Ban phases in competitive video games add extra dimensions to gameplay, as you cannot count on having access to characters that might be crucial to your strategy. In this way, ban phases are often used as a way to give players agency in balancing the game, since dominant team compositions have to contend with the enemy team being able to ban the characters that make those strategies work. Another common reason that a character could get banned is simply because they are annoying to play against, and are removed in the interest of fun. 
  
The central question we will be looking at is: **What can champion bans and pick rates tell us about their competitive performance in <u>League of Legends</u>?** This question aims to explore how the types of champions that are banned or picked—especially based on their archetypes—affect individual and team performance in professional <u>League of Legends</u> matches.
  
The Oracle’s Elixir dataset provides an extensive number of columns, as well as 60180 rows that consist of a group of 10 rows, 5 for each team's players and then 2 afterwards of the team stats for each team. The columns most relevant to our central question are the following: 
- champion: A champion is a playable character that players control during the game. There are 170 champions with unique abilities, stats, and roles. 
 - ban1-5: These columns represent the champions that they picked to not be able to be used for the match, in chronological order. 
 - pick1-5: These columns represent the champions that they picked to play, which they are locked into playing for the duration of the match.
 - result: This column is either Won or Lost and represents whether or not the individual player or team won the match. 
 - kills: This column gives the number of enemy champions the player, or collective team, defeated in the match. 
 - deaths: This column gives the amount of times the enemy team defeats the players, or collective teams champions in a match. 
 - assists: This column gives the amount of times the enemy team defeats the players, or collective teams champions in a match. 
assists: This column gives us the amount of times the player, or collective team, contributed to defeating an enemy champion while not getting the kill themselves. 
 - dpm: This column shows us the average damage a player, or collective team, does to an enemy champion per minute. 
earned gold: This column shows us the amount of gold a champion earned, which is earned by killing or assisting in killing enemy champions. Champions that have killed many enemies without dying are worth more gold. 
 - position: This column tells us the archetype of the champion that the player has chosen, can be Top, Jungle, Mid, Bot (ADC), and Support. 
damage share: This column refers to the percentage of total damage a champion deals to enemy champions within a team fight or over the course of a game.

## Data Cleaning and Exploratory Data Analysis

The first thing one would notice when looking at the Oracle dataset is that it is enormous: not only did it have over 60,000 rows, but it also contained 135 columns, which is an overwhelming amount of information. We ended up splitting the dataset into two distinct datasets, as many of the laters rows simply gave in-game statistical information 5 minutes into games, then at the 10 minute mark, etc. Since we were more concerned with the outcome rather than the course of a game, we kept it separate so that our main dataset was more focused on our central question.

Another key issue was that every element in the dataset was given as a string, even if they were meant to only contain numeric values. Cells that were supposedly missing values in fact contained empty strings instead. As such, we had to go column-by-column and cast them into their correct data-types, as well as correctly filling in any missing values with special values signifying as much, either np.nan for columns that could handle floating point numbers or -1 for integer columns.

One tricky consideration was that many of the columns had boolean True/False values represented as either 1’s or 0’s. For example, in the ‘firstbloodkill’ column, players that had gotten the first kill of the game were given a corresponding 1 and everyone else was given a 0. All such columns were cast to boolean values except for the ‘result’ column in which each 1 was instead labelled as “Won” and each 0 labelled as “Lost”.

![Cleaned Data DataFrame Head](/assets/cleaned_data_head.png)

Even after cleaning and removing extraneous columns, we were still left with 70 columns. However, here we can see both NaN and -1 values signifying missing data. 

### Univariate Analysis

<iframe 
  src="assets/univariate_analysis.html" 
  width="620"
  height="420"
  frameborder="0"
></iframe>

Here we can see that champions are not chosen very evenly. In fact, the most popular champions dominate, whereas a great many unpopular champions see nearly no play at all.

### Bivariate Analysis
  
The first bivariate analysis we performed was on the deaths column and resulted in the dataset to see the difference in average deaths in the team that wins the game vs. lose.
<iframe 
  src="assets/bivariate_1.html" 
  width="620"
  height="420"
  frameborder="0"
></iframe>
 
Here we can see that losing teams tend to die much more on average than winning teams, which suggests that wins tend to be very one-sided rather than close matches.

The next bivariate analysis we performed was the average damage mitigated per minute between the 5 different position types. 
<iframe 
  src="assets/bivariate_2.html" 
  width="620"
  height="820"
  frameborder="0" 
></iframe>

Here we can see how each position on the map varies in how much damage is done and how much damage is mitigated there. This suggests that teams are more defense-oriented in the jungle, whereas the mid and bot lanes see a lot more actual combat. 

### Interesting Aggregates

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>firstblood</th>
      <th>False</th>
      <th>True</th>
      <th>All</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Lost</th>
      <td>2903</td>
      <td>2121</td>
      <td>5024</td>
    </tr>
    <tr>
      <th>Won</th>
      <td>2119</td>
      <td>2895</td>
      <td>5014</td>
    </tr>
    <tr>
      <th>All</th>
      <td>5022</td>
      <td>5016</td>
      <td>10038</td>
    </tr>
  </tbody>
</table>

This pivot table looks at team rows and sees how first blood relates to the result of the game. Here we can see that teams that get the first kill of the game are much more likely to win, which suggests that gaining the advantage at the beginning of the game is important for victory.

## Assessment of Missingness

### NMAR Analysis

Two rows in particular that would likely be NMAR are "monsterkillsownjungle" and "monsterkillsenemyjungle," as many of the rows that are labelled as complete actually do not have data for these columns in particular. This alongside the obscure nature of the data these columns are collecting seem to suggest that they are not missing due to any other factors, seeing as the missingness of these columns seemingly did not contribute to whether the person making the Oracle dataset considered that row to be complete or not.

### Missingness Dependency

We are testing whether the missing values in the teamid column are related to other columns—in this case, league and side. We're using permutation tests with a significance level of 0.05 and Total Variation Distance (TVD) as our test statistic.
To start, we tested the relationship between teamid and league. The results indicate that the missingness in teamid does depend on the league column.
Null Hypothesis: The distribution of league is the same regardless of whether teamid is missing.


Alternative Hypothesis: The distribution of league differs depending on whether teamid is missing.


Below are the observed distributions of the league column, split by whether teamid is missing or not.

![League Missingness Distribution](/assets/missing.png)

After performing 1000 permutations of our test, the test statistic we found was around .812 and the p-value turned out to be 0.0. Below we see the plotly of the empirical distribution of our test statistic.

<iframe 
  src="assets/ED_league.html" 
  width="620"
  height="420"
  frameborder="0" 
></iframe>

Based on our tests, we would reject the null hypothesis due to the fact that our p-value is below the 0.05 significance level we set. 

Furthermore, we performed a second test to assess whether the missingness of teamid depends on the side as well. 

Null Hypothesis: The distribution of side is the same regardless of whether teamid is missing.


Alternative Hypothesis: The distribution of side differs depending on whether teamid is missing.
Below are the observed distributions of the side column, split by whether teamid is missing or not.

![Side Missingness Distribution](/assets/missing_result.png)

After performing 1000 permutations of our test, the test statistic we found was around **0.014** and the p-value turned out to be **0.128**. Below we see the plotly of the empirical distribution of our test statistic.

<iframe 
  src="assets/ED_result.html" 
  width="620"
  height="420"
  frameborder="0" 
></iframe>

Based on our tests, we would fail to reject the null hypothesis due to the fact that our p-value is above the 0.05 significance level we set. 

## Hypothesis Testing

Our hypothesis is as follows:

 - H<sub>0</sub> : All characters perform the same regardless of how often they are banned 
 - H<sub>1</sub> : Characters that are banned more often do better than those that are banned less frequently

The test statistic that was used was a linear regression analysis test, which produces a p-value corresponding to how likely it is that there is a correlation between ban-rate and win-rate. If there is no correlation, then we would expect an even spread of win-rates regardless of ban-rates, which would produce a very high p-value. Conversely, if there seems to be a pattern in the data that ban-rate. 
  
<iframe
  src="assets/hypothesis_test_first.html"
  width="620"
  height="420"
  frameborder="0"
></iframe>

This model gives a p-value of **0.257**, which is not very good. We can see from the graph that there is a lot of variance in win-rates for champions that are rarely banned. 

![Hypothesis Pivot Table](/assets/hypothesis_pt.png)

Upon investigating further, we can see that there are a lot of champions that rarely ever see play. We can see that since Heimerdinger has only been played once and lost that game, he has a 0% win-rate! To remedy these outliers, let us focus on champions that are chosen at least 0.1% of the time, which ends up shaving off roughly 40% of all champions.

<iframe
  src="assets/hypothesis_test_final.html"
  width="620"
  height="420"
  frameborder="0"
></iframe>

This model gives a p-value of **0.024**, which is much better. At an alpha-level of 0.05, we can safely conclude that, for champions that see widespread competitive play, there does appear to be a positive correlation between ban-rate and win-rate, which suggests that professional team are picking their bans based on the strength of a character, as expected.

## Framing a Prediction Problem

Now that we can see that win-rates are pretty strongly correlated to ban-rates, it seems safe to assume that the presence of certain champions have a dramatic effect on the outcome of a game. After all, some champions seem be a lot stronger than others, and many have higher than average win-rates. This begs the question: **how well can we predict whether a team will win or lose based on other game statistics?**

To build off of our hypothesis testing, we will begin the prediction with the champions players pick at the beginning of the game. As such, our inputs will be the five champions that each team chooses, and the outputs will classify whether the model thinks those picks will lead to a win or a loss. We do this to see if we can predict if a team will win or lose purely based off of the champions they choose, or must there be some skill and performance taken into account. 

## Baseline Model

For our baseline model, we used the 5 categorical columns of the champions being picked by each team: pick1, pick2, pick3, pick4, pick5. For our classifier we used a Random Forest Classifier and transformed our categorical data with the one-hot encoder in order to represent these picks numerically making certain that every category is handled as a separate item without suggesting a connection between them. 

After utilizing the cross_val_scores function and fitting the model to the training data, our accuracy score on the training data was **0.5077** meaning that our model can predict whether a team will win or lose 50.77% of the time. This accuracy score is not very good, as our model does not have much more predictive power than a coin toss. This makes sense, though, as it does not include any information from what happens during the duration of the game. This suggests that there is more to <u>League of Legends</u> than simply choosing champions. As such, adding features that take into account the players' actual performances would likely greatly improve our model. Also, we will fine-tune hyperparameters to make sure we are using the best parameters possible. 

## Final Model
For the final model we decided to add 2 quantitative features, kills and assists, to see if it is the fact that performance needs to be taken into account when predicting if a team will win or lose. When thinking about winning a MOBA style game, there are so many statistics that could come into account to help a team win or lose. Though repeatedly, the most common ones that people say directly contribute to a win are kills and assists. Because of this, we can assume that these 2 features will provide the lackluster base model with a boost in accuracy in predicting the outcome of <u>League of Legends</u> matches.  

As it is what we used in the baseline model we will continue using a Random Forest Classifier for the final model. To transform our 2 new features we decided to use a StandardScaler Transformer on kills and a QuantileTransformer on assists. We used GridSearchCV to utilize the best_estimator and best_params features to find the hyperparameters. The hyperparameters we chose where max_depth, min samples split, and n estimators. For the max depth we tested None, 5, or 10. For minimum samples split we tested 2 and 5. For number of estimators, 5, 10, 25, 50, 100. After using grid search, it found that the best hyperparamters were 'classifier__max_depth': None, 'classifier__min_samples_split': 5, 'classifier__n_estimators': 100. 

With these new features, the cross value accuracy score bumped up to **0.84396** which means that our model can predict whether a team will win or lose 84.4% of the time. This is a 33.63% increase from our baseline model, that is a large gap! With this increase, there is a very good chance for our model to correctly guess the outcome of a <u>League of Legends</u> match whereas before it was a coin flip. 


## Fairness Analysis

For our fairness model, we are going to be answering the question: “Does my model preform different for teams that have less than or equal the median amount of kills compared to teams with greater than the median amount of kills?"

Group X in our model is the teams with less than or equal to the median number of kills in the dataset. Group Y in the teams with strictly greater than the median number of kills in the dataset. 

Hypothesis:

Null Hypothesis: The model we created is fair. The accuracy for teams with less than or equal to the median number of kills is the same as the accuracy for teams with greater than the median number of kills. 

Alternative Hypothesis: The model we created is not fair. The accuracy for teams with less than or equal to the median number of kills is significantly different than the accuracy for teams with greater than the median number of kills. 

<iframe
  src="assets/fairness_hist.html"
  width="620"
  height="420"
  frameborder="0"
></iframe>

Our test statistic for this permutation test will be the absolute difference in average accuracy and the significance level will be set at 0.05. The resulting p-value of the permutation tests was **0.0**, which is less than the 0.05 significance level that we set so we reject the null hypothesis. This outcome implies that our model predicts teams from both groups at a different accuracy, our model shows bias towards group X showing a significantly better accuracy than group Y; in particular, our accuracy for Group X was far lower than it was for Group Y, which suggests that our model is less accurate for teams with higher kills.