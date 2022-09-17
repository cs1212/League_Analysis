# League_Analysis
league of legends win analysis for blue team

# Data Exploration
In the histograms below, I plotted out the values of each variable for blue team when they won and lost. This allows us to see the differences in blue team's performance when they win or lose. From the histogram, we can see that there is a clear difference in blueGoldDiff for a win vs loss whereas blueWardsPlaced does not show as much of a difference. We can use this to make an initial guess as to which variables should be kept for determining blue team win.
![feature_histograms](/images/feature_histograms.png)

Besides the histogram, we can also look at the correlations and get rid of correlated variables since they contain redundant information. Some highly correlated variables are redKills, redDeaths, and redExperienceDiff. This makes sense because the information from those variables is captured by their blue counterparts, blueDeaths, blueKills, and blueExperienceDiff respectively.
![correlations](/images/feature_correlations.png)


# Results
## Logistic Regression Feature Importances
![logreg_importance](/images/logistic_importance.png)

## XGBoost Feature Importances
![xgboost_importance](/images/xgboost_importance.png)


# Conclusion
