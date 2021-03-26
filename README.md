# Capstone - How to win Warzone?
General Assembly


# Goals and Success Metrics of the of the Project:

- Gaming is a massive industry (valued at c.$150bn -> 3x the size of the music and box office industries combined), which has drastically evolved, especially with the emergence of cloud computing
  - Games are no longer played in the same way i.e. beginning/ ending then onto the next game
  -	Games have now become living ecosystems where players meet socially, shop, attend virtual events, etc.
-	Data scientists have become instrumental in keeping modern games alive and healthy
-	Successful analysis of player and gameplay data can help:
  -	Create successful monetization strategies – this is especially a great concern for free-to-play games, where it is rare to have more than 5% of players paying for in-game products
  -	Reduce player churn
  -	Launch successful upgrades
  - Even save a game, which has struggled to take off
- Warzone has become one of the most popular F2P games in the world since its launch in March 2020
  - It counts c.100m downloads and features Twitch’s top 10 overall most viewed
-	As a result, I thought it would be interesting to analyze the gameplay and see if I could muster a greater understanding of why it has become so popular other than being a Call of Duty franchise?
  -	Is there a strategy to win?
  -	Is the winner predictable before the start?

-	The main goal of this project was to obtain a better understanding of what factors beyond the obvious (i.e. survival time) have a strong say in a player’s chances of winning 
-	I was particularly curious examine if:
  -	Conclusive variables such as kills, player rank and deaths would be as telling as one might perceive
  -	Niche variables like moving time, completing missions or choice of weapon have any influence in the final result
- All in all, a key goal was to be able to suggest some ‘winning strategy’ such as:
   - Make sure to move
   - Complete in-game side missions
   - Avoid confrontation
   - Use this combination of weapons 
-	A successful project was defined as:
  -	Obtaining a dataset – this was deemed unlikely due to new privacy measures with regards to new default data privacy settings
  -	Beating the baseline score
  -	Being able to suggest some sort of success strategy
  -	Spot areas for improvement / new potential modelling ideas

# Data Collection:

Acquiring the data was in itself the biggest challenge posed by this project. This is because Activison changed the default data privacy settings to private. As a result, accessing the in-game data of other players has now become impossible, unless players manually change their privacy settings to public on the Activision website. This means that classic APIs such as tracker.gg or RapidAPI have lost a lot in terms of practicality without a list of public gamer ids, which I am yet to find. 

After initial research, Alexandre le Corre's Warzone API available on RapidAPI was the best option. However, it has two limitations. The first is that without access to a list of 'public' gamer ids the only available data for download is 'Leaderboard'. Unfortunately, this data is not very exciting containing 16 variables such as: Prestige, XP, Time Played, Wins, Losses, Killstreaks, etc. The second problem is that being a Freemium API, I was limited to 500 rows of data a day.

I also came across an API developed by iShot, which was available on Postman. Even though the privacy constraint still existed this API has access to a wealth of public data (79 columns) such as: Missions Completed, Player Stats, Weapons etc. as long as one can provide match ids. By chance, browsing through the API's discussion on Discord I found a pdf file posted by Caedrius with a list of 1000 match ids (each match contains data on c.150 players) in which he played. Even though I was ideally looking for a list of random ids belonging to a range of players, this was the best I could find and would provide more than enough data to explore my question.

So, once I managed to convert the pdf into a JSON, I adapted the Postman API from the platform so that I could collect the data from all the match ids in one go. This is the exercise that has been carried out in this section.


# Data Cleaning, EDA and Feature Engineering:

Data cleaning was a lengthy process as the downloaded data was full of nested lists and dictionaries. Hence, all these lists and dictionaries had to be opened before concatenated into one large dataframe

The data because it was sourced from a single player’s gameplay was full of outliers, imbalances and nulls. These all had to be curated for. A key factor that was observed via EDA was that the data set suffered from multicollinearity. The linking variable was time played. The longer a player plays the higher his ranking. For example, from the data when distance travelled was high, it did not represent a specific strategy it was more illustrative that the player had been alive for longer and therefore, finished well. So, it was important to create new ‘per minute’ variables that would mostly eliminate the importance of time played.



# Modelling:

The modelling was done twice. Once on the core dataset and another on the individual game modes (Quads, Trios, Duos and Solo). This is because during the EDA it was spotted that these different games modes separated themselves in the data

The models used for this multi-class classification problem were:
Random forest > Tensorflow > Adaboost (decision tree) > Logistic > KNN > Bayes

Findings from the models:
1) The CV scores range from 0.43 to 0.62 convincingly beating the baseline estimation!
2) From the coefficients we can deduce that:
•	A player, with a team, who moves a lot whilst completing missions is more likely to earn a higher ranked placement.
•	Surprising features were the negative impact of kills in the logistic regression and the positive impact of damage taken. The relationship of kills could either be true (unlikely) or i.e multicollinearity within the data has distorted the coefficent strength/direction. Damage taken happens to have a strong relationship with distance travelled, which suggests a weakness in the data and that time played has not been completely eliminated from the variables (the longer the time played the more chances of getting hit).
•	Another observations is that the weapon/perk choice seems to be insignificant with regards to the outcome of the game at first glance.
3) The curves and matrices indicate that the models performs better in classifying the first and fifth classes but struggle with the middles classes (especially 2 and 3). This is not surprising as the distinction between the middle classes in the data is much more subtle. There are still a few outliers in the data helping the model differentiate the exterior classes.

# Conclusion and Evaluation:

Overall I am satisfied with the outcome of this project because most my goals were achieved. I managed to obtain a dataset, albeit not the ideal one, and to beat the baseline / considerably improve my scores (from originally high 40s) thanks to feature engineering. I was also able to suggest a winning strategy: play in a team, move a lot and try to complete in game missions.

Unfortunately, the dataset was distorted and complicated to work with due to the extensive multicollinearity. As a result, even though regularization was applied and the scores were high the resulting coefficient strengths and direction were at times suspicious (i.e. damage taken or kills). I was also disappointed that the analysis by game mode did not reveal any feature differences or that the impact of weapon choice was mostly insignificant.

Going forward it would be interesting to run this model with a more complete dataset and ideally obtain other gameplay stats, which are today private.
