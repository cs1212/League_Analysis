# League of Legends Win Analysis
What does it take for a team to win in a game of League of Legends? In a game of League of Legends, games usually last 40 minutes and there are 2 teams, blue and red. The objective is to destroy enemy towers and go deep into the enemy base to destroy their Nexus.

In this repo, I look at data from the first 10 minutes of 10k high ranked league games and determine what game objectives players on blue team should focus on to improve their chances of winning. In order to do so, I created a logistic regression and XGBoost model to visualize what features the models found important enough to use for predicting blue team win. Besides those two models, I also built a neural network to try and predict the game winner based off of data from the first 10 minutes of a game.

For a more in-depth look at the variables and models, please refer to the notebook [here](/league.ipynb). Analysis was done in google colab and exported to .ipynb.

# Dataset
The data was taken from Kaggle and can be found [here](https://www.kaggle.com/datasets/bobbyscience/league-of-legends-diamond-ranked-games-10-min). It contains data from the first 10 minutes of 9879 high ranked League games (Diamond 1 to Master)
## Terminology
* Ward
  * Item that grants vision of the nearby area
* Assist
  * Given when a player contributes to killing an enemy player but did not deal the finishing blow
* Elite Monsters
  * Neutral monsters that give buffs to the team that kills it. The 2 elite monsters are Dragon and Herald
* Dragon
  * Elite monster that gives various buffs to the team that kills it
* Herald
  * Elite monster that gives players an item that summons Herald to help destroy towers and minions
* Tower
  * Structures that protect the team's base. Enemies need to kill towers to get deeper into the base and destroy the Nexus, ending the game.
* CS
  * Creep Score. A number value that counts how many minions/jungle minions a player has killed so far.
* Minions
  * monsters that players kill for experience and gold
* Jungle Minions
  * monsters that spawn in the jungle, away from minions, that players can kill for experience and gold.

# Data Exploration
In the histograms below, I plotted out the values of each variable for blue team when they won and lost. This allows us to see the differences in blue team's performance when they win or lose. From the histogram, we can see that there is a clear difference in blueGoldDiff for a win vs loss whereas blueWardsPlaced does not show as much of a difference. We can use this to make an initial guess as to which variables should be kept for determining blue team win.
![feature_histograms](/images/feature_histograms.png)

Besides the histogram, we can also look at the correlations and get rid of correlated variables since they contain redundant information. Some highly correlated variables are redKills, redDeaths, and redExperienceDiff. This makes sense because the information from those variables is captured by their blue counterparts, blueDeaths, blueKills, and blueExperienceDiff respectively.
![correlations](/images/feature_correlations.png)


# Results
## Logistic Regression Feature Importances
After training the logistic regression model on the dataset, we can look at the coefficients and determine how 1 unit change affects the odds of winning. From the plot below, we can see that big factors of winning include killing elite monsters, killing the enemy team, and destroying enemy towers. Factors for losing are reversed. Blue team wants to prevent red team from killing elite monsters, killing blue team members, and destroying blue towers.

![logreg_importance](/images/logistic_importance.png)

## XGBoost Feature Importances
After training the XGBoost model, we can plot the importance of each feature that was used in the model. At first glance, there are some differences in the features selected from XGBoost compared to logistic regression. For example, XGBoost placed more importance on gold and experience difference between the two teams while logistic did not. Besides that, they share some similarities such as the importance of killing elite monsters and the enemy team and the unimportance of CSPerMin.

![xgboost_importance](/images/xgboost_importance.png)

## Feature Plots
Given these features from our two models, let's take a look at some of them.
#### Blue Gold Diff
At the 10 minute mark, blue team should aim for a 1200 gold lead for a higher chance of winning.
![bluegold_diff](/images/blue_golddiff.png)
#### Blue EXP Diff
At the 10 minute mark, blue team should aim for a 900 experience lead for a higher chance of winning.
![blueexp_diff](/images/blue_expdiff.png)
#### Blue Elite Monsters
Given that the only elite monsters to spawn in the first 10 minutes would be 1 dragon and 1 herald, it appears that most games have 0 elite monsters killed in the first 10 minutes but if blue team manages to more elite monsters, then their chances of winning increases.

![blueelite](/images/blue_elitemonsters.png)
#### Blue CSPerMin
We don't see much of a difference in average CS per minute for a win vs loss which explains why our models didn't find this feature important.

![blueCS](/images/blue_cspermin.png)
#### Winrate Per Dragon Kill
If blue team manages to kill 1 dragon, then there is a 64% chance of winning 
![dragon_winrate](/images/winrate_dragon.png)

## Model Results
The logistic regression model was the baseline and had an accuracy of 72%. XGBoost and the neural network could not improve the accuracy and they both also had 72% accuracy.

# Conclusion
Based on the feature importances, the important game objectives that players on blue team should focus on should be enemy kills, elite monsters (dragon over herald), gaining gold lead, and gaining experience lead.
* Enemy Kills
  * On average, blue team wins by getting at least 7 kills in 10 minutes.
* Elite Monsters
  * Blue team wins focused more on dragons over heralds. 67% of elite monsters killed when blue wins were dragons so the team should prioritize killing dragon over herald.
* Gold Difference
  * On average, the winning team had a 1270 gold lead 10 minutes into the game. If we split this across all 5 team members, this is 254 extra gold per member which is roughly worth one small equipment. Players should aim for having bought 1 more equipment than the opposing team by 10 minutes to maximize their odds of winning.
* Experience Difference
  * Players should aim for a 900 experience difference from the enemy team. This can come from killing more minions/monsters or getting enemy kills. For example, this equates to killing 18 more minions than the enemy (3 waves).
