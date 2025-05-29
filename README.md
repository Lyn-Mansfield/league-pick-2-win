# league-pick-2-win

Hello all! First Commit!

## Introduction



## Data Cleaning and Exploratory Data Analysis

	League of Legends is one of the most famous multiplayer online battle arena games developed by Riot Games on October 27, 2009. With close to 120 million monthly active users and 40 million daily users worldwide, the game has become one of the most dominant and enduring games in the MOBA world. We will be working with a dataset developed by Oracle’s Elixir, which contains match data from professional League of Legends esports games played throughout 2025. It is filled with detailed statistics from real esports games, giving us insight into how players perform, how teams strategize, and the circumstances leading to defeat or victory.
	
	One of the main aspects of League of Legends’ competitive scene is the initial ban phase. In professional League of Legends (as well as ordinary ranked play), the ban phase involves both teams selecting champions to be removed from the game's pool, preventing them from being picked by either side. This phase alternates between each team banning a champion, with each team having banning five champions for a total of ten character bans. 
	
	Ban phases in competitive video games add extra dimensions to gameplay, as you cannot count on having access to characters that might be crucial to your strategy. In this way, ban phases are often used as a way to give players agency in balancing the game, since dominant team compositions have to contend with the enemy team being able to ban the characters that make those strategies work. Another common reason that a character could get banned is simply because they are annoying to play against, and are removed in the interest of fun. 
	
	The central question we will be looking at is: What can champion bans and pick rates tell us about their competitive performance in League of Legends? This question aims to explore how the types of champions that are banned or picked—especially based on their archetypes—affect individual and team performance in professional League of Legends matches.
	
	The Oracle’s Elixir dataset provides an extensive number of columns, as well as 60180 rows that consist of a group of 10 rows, 5 for each team's players and then 2 afterwards of the team stats for each team. The columns most relevant to our central question are the following: 
- champion: A champion is a playable character that players control during the game. There are 170 champions with unique abilities, stats, and roles. 
 - ban1-5: These columns represent the champions that they picked to not be able to be used for the match, in chronological order. 
 - pick1-5: These columns represent the champions that they picked to play, which they are locked into playing for the duration of the match.
result: This column is either Won or Lost and represents whether or not the individual player or team won the match. 
 - kills: This column gives a quantity of the number of enemy champions the player, or collective team, defeated in the match. 
 - deaths: This column gives a quantity of the amount of times the enemy team defeats the players, or collective teams champions in a match. 
assists: This column gives us a quantity of the amount of times the player, or collective team, contributed to defeating an enemy champion while not getting the kill themselves. 
 - dpm: This column shows us the average damage a player, or collective team, does to an enemy champion per minute. 
earned gold: This column shows us the amount of gold a champion earned, which is earned by killing or assisting in killing enemy champions. Champions that have killed many enemies without dying are worth more gold. 
 - position: This column tells us the archetype of the champion that the player has chosen, can be Top, Jungle, Mid, Bot (ADC), and Support. 
damage share: This column refers to the percentage of total damage a champion deals to enemy champions within a team fight or over the course of a game.

## Assessment of Missingness



## Hypothesis Testing

Our hypothesis is as follows:

 - Null : All characters perform the same regardless of how often they are banned 
 - Alt. : Characters that are banned more often do better than those that are banned less frequently

	The test statistic that was used was a linear regression analysis test, which produces a p-value corresponding to how likely it is that there is a correlation between ban-rate and win-rate. If there is no correlation, then we would expect an even spread of win-rates regardless of ban-rates, which would produce a very high p-value. Conversely, if there seems to be a pattern in the data that ban-rate. 
	
<iframe
  src="assets/hypothesis_test_first.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/hypothesis_test_final.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Framing a Prediction Problem



## Baseline Model



## Final Model



## Fairness Analysis


