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

In order to prep our data for analysis, we combined the datasets so that every row would represent a player and a season. This was quite a complicated process because it was essential that our data only include statistics from before the All-Star game, and this meant we must construct our own summary statistics from every player's individual performance from every game. This was accomplished by merging kaggle datasets and summarizing the results. The main problems we ran into were duplicate data in the original datasets and season's time frames changing because of postponements. These problems were satisfactorily resolved. We decided to keep data only from the 2012 season onward because the 2011 season had a lockout, which might have skewed the results, and the time before that may not have been very relevant due to the change in the style of play. Also, the starting year in the season was chosen as the standard name of the season in order to avoid confusion (2015-2016 is called 2015). We also removed two honorific All-Star selections in 2018 and imputed null values.

In addition to the data added from the alternative data sources discussed previously, we also standardized all relevant player statistics by pace, calcultated True Shooting Percentage and Player Efficiency Rating, added % of Games Played (because 2020 season started late and this makes a comparable games played metric), and added Team win percentage. All of this data is as of just before the All-Star game.

This data preparation left us with the following potential predictors: season, percentage games played, position, 2K rank, conference, team win percentage, jersey sales, if in previous All-Star game, previous All-Star game selections, and per game average minutes played, field goals made/attempted, 3-pointers made/attempted, free throws made/attempted, offensive/defensive rebounds, blocks, steals, turnovers, assists, fouls, points, +/-, PER, and true shooting percentage,











 