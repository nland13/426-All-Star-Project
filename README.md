# NBA All-Star Predictions
## Introduction

Every year the NBA selects 24 of its star players, 12 from each conference, to play in an exhibition "All-Star" game. This game is usually held in the middle of the season and is one of the most anticipated events of the year, serving as the marquee event of the otherwise uniform middle part of the season. 

Being selected as an All-Star is considered a very high honor, and the number of All-Star selections a player has is often one of the first statistics cited to demonstrate a player's greatness. Selection to the All-Star game is not just honorific, however, with many endorsement contracts including substantial bonuses for a player who achieves the rank.

Given the amount of pride and money on the line, it should be no surprise that All-Star selections are one of the most controversial and hotly debated topics of the NBA season. Fans, players, coaches, and media all participate every year in discussing and arguing about who was snubbed and who doesn't belong. 

Our project's research question is if data collected up until the All-Star game each year can be used to predict who will be selected as an All-Star. We will answer this question by training a model on data from the 2012-2019 seasons and then making predictions for the 2020 season All Star game. This will shed light on how predictive our data is for All-Star selections, what factors matter the most, and what players, if any, were snubbed in the 2020 season according to historical data.

## Data Collection

Because predicting All-Star selections is something that is done by many people every year, we decided it was essential that our project include some data sources that are off the beaten path and potentially neglected. As such, in addition to the standard basketball player metrics which we got from Kaggle, we also collected alternative data that we judged could be predictive and relatively unused in standard prediction methodologies.

For example, because the starters in the All-Star game are selected via fan, player, and media voting, we decided it would be important to include data that indicates a player's popularity. The best variable we could find for this was if a player's jersey was in the Top 15 of sales the previous season. This data was collected via various news articles stemming from NBA.com press releases. Data for the previous year was used as that is all that would be available at the time of prediction.

Other variables that we believed might reflect popularity was previous All-Star selections. As such we collected data on previous All-Star selections by modifying a script that scrapes data from Basketball-Reference.

We also added a player's NBA 2k video game rank for that year and got that data from an NBA fan website. This was quite a painstaking process as matching the names from the datasets was difficult, but we believed that including 2K rank would help make up for the lack of comprehensive defensive statistics in our player statistic data. We decided to use player rank rather than rating as we observed rating had been inflated over the years.

Our final alternative data source was pace data from Basketball Reference. Pace data allowed us to standardize the statistics from season to season and control for the change of pace in the game (i.e., more points are scored in one season compared to the other). 

## Data Preparation

#### Combining the Data
In order to prep our data for analysis, we combined the datasets so that every row would represent a player and a season. This was quite a complicated process because it was essential that our data only include statistics from before the All-Star game, and this meant we must construct our own summary statistics from every player's individual performance from every game. This was accomplished by merging kaggle datasets and aggregating the results. The main problems we ran into were duplicate data in the original datasets and season's time frames changing because of postponements. These problems were satisfactorily resolved. We decided to keep data only from the 2012 season onward because the 2011 season had a lockout, which might have skewed the results, and the time before that may not have been very relevant due to the change in the style of play. Also, the starting year in the season was chosen as the standard name of the season in order to avoid confusion (2015-2016 is called 2015). We also removed two honorific All-Star selections in 2018, imputed null values, one hot encoded categorial variables, and removed variables not used in the analysis.

#### Feature Engineering to Include Advanced Statistics and Increase Comparability Across Seasons
After combining all the data from all of our sources, we calculated two commonly used advanced metrics - PER and True Shooting Percentage. PER, or Player Efficiency Rating, combines all the basic statistics on a player using a weighted formula. It is valuable because it accounts for how active a player is in a game and penalizes poor play in addition to awarding good play. True Shooting Percentage is calculated using a formula that accounts for the different values shots have. That way somebody who is taking a lot of difficult 3-pointers is not penalized compared to the player who takes more relatively easier 2-point shots.

We also wanted to make the seasons as comparable as possible. We did this by dividing all the relevant metrics by their season's corresponding pace so that the increase in the speed of the game would not confuse the model. Additionally, instead of using games played as a factor, we used games played as a percentage of total games played by that team. That way shortened seasons such as the 2020 season would not have very different games played figures.

## EDA
Exploring the data, we noticed that no All-Star had ever averaged less than 27 minutes per game. Thus, for EDA we filtered the data set by players who average more than 20 minutes per game to make the All-Star and non-All-Star groups more comparable and also make the visuals easier to interpret. After applying this filter, we calculated the average statistics for the All-Star and non-All-Star groups and found the difference. This can be seen in the table below. As shown in the table, the differences between the groups are quite large, and this encouraged us that we would be able to detect the differences in the classes.

<img src="https://github.com/nland13/final/blob/main/figures/table.png">

We then created several scatterplots for variables we though would be important, color-coding by All-Star status. These plots once again support the notion that the classes are divided well on the features we have collected. The points vs team winning percentage plot was especially interesting and revealed that those two features alone nearly divided the classes into entirely separate groups. That plot also revealed an interesting pattern that shows that in general an All-Star can score less if he is on a team with a higher winning percentage. These scatterplots are shown below.

<img src="https://github.com/nland13/final/blob/main/figures/winpoints.png">

<img src="https://github.com/nland13/final/blob/main/figures/pointstsp.png">

<img src="https://github.com/nland13/final/blob/main/figures/per2k.png">

To further explore the data, we also used PCA to reduce the data to only 2 components. We then plotted this, color-coding by All-Star status. The clear separation in the data using only two components further confirmed our suspicion that our model could potentially be effective at predicting what classes a data point belonged to.

<img src="https://github.com/nland13/final/blob/main/figures/pca.png">

## Model Fitting
The plan to fit and test the model was to use only the data from the 2012-2019 season to fit the model so that we could test how well our model performed on the 2020 season. Thus, the first step in fitting the model was to completely separate the 2012-2019 data and the 2020 data to ensure no data leakage whatsoever, which would taint our results. Then, we split the 2012-2019 data into test and train datasets, using .67 for the train dataset size, so we could evaluate what model worked the best. 

Before fitting the models, we us used BorderlineSMOTE from the imblearn package. This function creates synthetic data from the minority class (All-Stars in our case), focusing on border cases, so that the classes are more balanced. This was appropriate for our data because the vast majority of players are not All-Stars in a given season, and thus we had significant class imbalance. We also scaled the data. Both of these changes to the data improved our model performance.

After making those adjustments to the data, we fit a multinomial Naive Bayes model, a logistic regression model, a decision tree model, a random forest model, and a gradient boosting model on the training data from 2012-2019. We tested all of these models as each has certain strengths and weaknesses and so we wanted to see which would be best suited to our data based on their performance in the train/test split. We then calculated test/training accuracy, F1 Score, AUC, and the confusion matrix for each model. This allowed us to compare the performance of the models, but due to our classes being very imbalanced, we had to be careful with our interpretation. For example, achieving a high accuracy score could be done by predicting all non-All-Stars, something that would not help us answer our research question. So, in addition to these standard metrics, we also created our own unique metric that looked at the percentage of correctly predicted All-Stars in the test-data if we only get the same number of predictions as there are All-Stars in the test data. For example, there happened to be 73 total All-Stars in the test data, so for each model we sorted our predictions by probability of being an All-Star and only kept the top 73. Then, we evaluated how many in our top 73 were All-Stars. This metric was extremely useful for comparing the models because it most closely mimicked how we would assess our overall results when we predicted on the 2020 data.

The model that we selected after evaluating their performance was a logistic regression model, which predicted 81% of the top 73. We picked this model because it performed strongly on the standard metrics and had the highest score on our custom metric. An advantage of this model is that it is easy to work with, quite interpretable, and less inclined to overfitting than a more flexible model. A disadvantage is that it assumes linearity between the dependent and independent variables, but its high performance on our metrics makes us less concerned about this disadvantage. After deciding to use the logistic regression model, we then selected what features we would use. We did this by using a recursive feature elimination function that ranked the importance of each feature in the model. For this part we returned to using all the 2012-2019 data rather than splitting it up into test/train so that we would use more data when deciding what features to keep. Based on the results of this feature ranking and some trial-error testing on the training data, we decided to keep the following features: minutes, field goals made, field goals attempted, 3 pointers made, free throws made, free throws attempted, offensive rebounds, defensive rebounds, assists, blocks, fouls, points, PER, team win percentage, 2K Rank, Prior All-Star appearances, percentage games played, position, and conference. Keeping only these features improved the model's performance. 

Keeping only those features also meant we decided to drop 3 pointers attempted, steals, turnovers, plus minus, true shooting percentage, top 15 jersey sales, and if selected for the prior All-Star game. Out of the dropped features, it makes sense that turnovers and true shooting percentage got dropped because both of these metrics will be harmed when players are required to do a lot. That means players with poor numbers for these features could either be those who simply play poorly or those who are very good and are thus required to do a lot. That makes the metrics not very helpful for predicting All-Stars. It was more surprising, however, that selection for the prior All-Star game and if a player had top 15 jersey sales ended up being unimportant to the model.

After deciding on which features to keep in, predictions were made by simply oversampling/scaling the 2012-2019 data, keeping the relevant features, fitting the model, and then predicting probability on the 2020 data, which was also scaled but not oversampled of course. The 12 players from each conference with the highest predicted probability of being an All-Star were then considered to be the model's predictions. This way we got exactly 12 All-Star predictions for each conference. After making these predictions, no changes were made to the model to ensure that no data leakage or overfitting would occur and the results would be representative of the model's true predictive ability.

## Results

Below is the predicted All-Star team for the Western Conference in the 2020 season (All-Star game held in 2021) ordered by the model's predicted probability of being an All-Star. Overall, the model correctly predicted 11/12 of the players, and the player who was predicted to be an All-Star but did not end up being one had the lowest probability of the 12 predictions. The player that the model failed to select for the 12th spot was Zion Williamson, a player with a lot of potential and hype who was in only his second year at the time. It is possible he was selected for the All-Star team in part because of his future potential rather than his results, something our model could not account for.

| Predicted All-Stars    | Actually All-Star?          |
| ------------- |:-------------:|
| LeBron James| Yes |
| Anthony Davis  | Yes    |
| Damian Lillard | Yes      |
| Luka Doncic | Yes |
| Stephen Curry  | Yes   |
| Paul George | Yes |
| Nikola Jokic  | Yes    |
| Kawhi Leonard | Yes      |
| Donovan Mitchell | Yes |
| Rudy Gobert  | Yes   |
| Chris Paul | Yes |
| DeMar DeRozan  | **No**   |

Failed to Select: Zion Williamson (ranked #19 in model)

Total: 11/12

Below is the predicted All-Star team for the Eastern Conference. Overall, the model correctly predicted 9/12 of the players on the team. It should be noted, however, that Domantas Sabonis, who our model predicted to be an All-Star, was actually chosen as an All-Star after another player contracted COVID but was not technically part of the initial 12 players, so his selection is considered an error by the model. 

| All-Stars     | Actually All-Star?         |
| ------------- |:-------------:|
| Kevin Durant | Yes |
| Giannis Antetokounmpo   | Yes    |
| Joel Embiid | Yes      |
| Trae Young | **No** |
| Kyrie Irving   | Yes   |
| Jayson Tatum | Yes |
| Domantas Sabonis   | **No***    |
| James Harden | Yes      |
| Bradley Beal | Yes |
| Khris Middleton | **No**   |
| Jaylen Brown | Yes |
| Ben Simmons   | Yes   |

Failed to Select: Zach Lavine (ranked #14 in model), Julius Randle (ranked #17 in model), Nikola Vucevic (ranked #20 in model)

Total: 9/12

## Conclusion
Overall, it seems our ability to predict if a player will be an All-Star based on the data we collected is quite good. We were able to correctly predict 20/24 (83%) of the players on the 2020 All-Star team and none of the players we failed to select were out of the model's top 20 ranking for each conference (top 12 are selected). Additionally, one of the players we technically predicted incorrectly did end up getting selected as a replacement. To put this result in context, we found [predictions](https://www.nba.com/news/powells-potential-all-star-field) made by a journalist for nba.com at the end of January 2021, so someone who had the same amount of info as us. This journalist, who has covered the NBA for 25 years, was able to predict 21/24 correctly, so just one better than our model. Additionally, the 2020 season we predicted on was shortened because of postponements, making it an especially difficult year to make predictions on due to the lack of data.

Some of the strengths of our analysis include having a dataset compiled from a variety of sources, including advanced statistics, increasing comparability across seasons, fixing the class imbalance problem, and creating our own relevant metric on which to judge and select a model. Some of the limitations in our analysis include not having better popularity metrics (such as social media followers), not having more advanced metrics, not taking into account that All-Star starters and reserves are selected using a slightly different process in recent years, not taking into account changes in the All-Star selection process, not having any features to reflect a player's momentum (recent improvements), not having any metrics to reflect the media narrative around a player, and not adjusting some of the statistics by usage rate (how often a player has the ball).

A new question that could be sparked based on this analysis is if the features useful for predicting All-Star starters are the same as those useful for predicting All-Star reserves. More analysis could also be done to determine what advanced metrics are most important for predicting All-Star status. Finally, this data and model could be extended to predict other NBA awards, like All-NBA status and MVP, and if the team data was dropped, it could also be used to predict which teams will perform the best going forward. 












 
