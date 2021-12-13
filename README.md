# NBA All-Star Predictions
## Introduction

Every year the NBA selects 24 of its star players to play in an exhibition "All-Star" game. This game is usually held in the middle of the season and is one of the most anticipated events of the year, serving as the marquee event of the otherwise uniform middle part of the season. 

Being selected as an All-Star is considered a very high honor, and the number of All-Star selections a player has is often one of the first statistics cited to demonstrate a player's greatness. Selection to the All-Star game is not just honorific, however, with many endorsement contracts including substantial bonuses for a player who achieves the rank.

Given the amount of pride and money on the line, it should be no surprise that All-Star selections are one of the most controversial and hotly-debated topics of the NBA season. Fans, players, coaches, and media all participate every year in discussing and arguing about who was snubbed and who doesn't belong. 

Our project aims to answer the question of if player data can be used to predict who will be selected as an All-Star. It will do so by being trained on data from the 2012-2019 seasons and then making predictions for the 2020 season All Star game. This will shed light on how predictive our data is for All-Star selctions, what factors matter the most, and what players, if any, were snubbed in the 2020 season according to historical data.

## Data Collection

Because predicting All-Star selctions is something that is done by many people every year, we decided it was essential that our project include some data sources that are off the beaten path and potentially neglected. As such, in addition to the standard basketball player metrics which we got from Kaggle, we also collected alternative data that we judged could be predictive and relatively unused in standard prediction methodologies.

For example, because the starters in the All-Star game are selected via fan, player, and media voting, we decided it would be important to include data that indicates a player's popularity. The best variable we could find for this was if a player's jersey was in the Top 15 of sales the previous season. This data was collected via various news articles stemming from NBA.com press releases. Data for the previous year was used as that is all that would be available at the time of prediction.

Other variables that we believed might reflect popularity, or a player's aura, was previous All-Star selections. As such we collected data on previous All-Star selections by modifying a script that scrapes data from Basketball-Reference.

We also added a player's NBA 2k video game rank for that year and got that data from an NBA fan website. This was quite a painstaking process as matching the names from the datasets was difficult, but we believed that including 2K rank would help make up for the lack of comprehensive defensive statistics in our player statistic data. We decided to use player rank rather than rating as we observed rating had been inflated over the years.

Our final alternative data source was pace data from Basketball Reference. Pace data allowed us to standardize the statistics from season to season and control for the change of pace in the game (i.e. more points are scored in one season compared to the other). 

## Data Preparation

#### Combining the Data
In order to prep our data for analysis, we combined the datasets so that every row would represent a player and a season. This was quite a complicated process because it was essential that our data only include statistics from before the All-Star game, and this meant we must construct our own summary statistics from every player's individual performance from every game. This was accomplished by merging kaggle datasets and aggregating the results. The main problems we ran into were duplicate data in the original datasets and season's time frames changing because of postponements. These problems were satisfactorily resolved. We decided to keep data only from the 2012 season onward because the 2011 season had a lockout, which might have skewed the results, and the time before that may not have been very relevant due to the change in the style of play. Also, the starting year in the season was chosen as the standard name of the season in order to avoid confusion (2015-2016 is called 2015). We also removed two honorific All-Star selections in 2018, imputed null values, one hot encoded categorial variables, and removed variables not used in the analysis.

#### Feature Engineering to Include Advanced Statistics and Increase Comparability Across Seasons
After combining all the data from all of our sources, we calculated two commonly-used advanced metrics - PER and True Shooting Percentage. PER, or Player Efficiency Rating, combines all the basic statistics on a player using a weighted formula. It is valuable because it accounts for how active a player is in a game and penalizes poor play in addition to awarding good play. True Shooting Percentage is calculated using a formula that accounts for the different values shots have. That way somebody who is taking a lot of difficult 3-pointers is not penalized compared to the player who takes mostly relatively easier 2-point shots.

We also wanted to make the seasons as comparable as possible. We did this by dividing all the relevant metrics by their season's corresponding pace so that the increase in the speed of the game would not confuse the model. Additionally, instead of using games played as a factor, we used games played as a percentage of total games played by that team. That way shortened seasons such as the 2020 season would not have very different games played figures.

## EDA
Exploring the data, we noticed that no All-Star had ever averaged less than 27 minutes per game. Thus, for EDA we filtered the data set by players who average more than 20 minutes per game to make the All-Star and non All-Star groups more comparable and also make the visuals easier to interpret. After applying this filter, we calculated the average statistics for the All-Star and non All-Star groups and found the difference. This can be seen in the table below. As shown in the table, the differences between the groups are quite large, and this encouraged us that we would be able to detect the differences in the classes.

<img src="https://github.com/nland13/final/blob/main/figures/table.png" width="900" height="150">

We then created several scatterplots for variables we though would be important, color-coding by All-Star status. These plots once again support the notion that the classes are divided well on the features we have collected. The points vs team winning percentage plot was especially interesting and revealed that those two features alone nearly divided the classes into entirely seperate groups. That plot also revealed an interesting pattern that shows that in general an All-Star can score less if he is on a team with a higer winning percentage. These scatterplots are shown below.

<img src="https://github.com/nland13/final/blob/main/figures/winpoints.png" width="900" height="150">

<img src="https://github.com/nland13/final/blob/main/figures/pointstsp.png" width="900" height="150">

<img src="https://github.com/nland13/final/blob/main/figures/per2k.png" width="900" height="150">

To further explore the data, we also used PCA to reduce the data to only 2 components. We then plotted this, color-coding by All-Star status. The clear separation in the data using only two components further confirmed our suspicion that our model could potentially be effective at predicting what classes a data point belonged to.

<img src="https://github.com/nland13/final/blob/main/figures/pca.png" width="900" height="150">

## Model Fitting













 
