# Data visualization and predictive modeling on movie data
## EDSP, Spring 2016
### Siyang (Lexie) Sun

## Research questions:
1) How's the film industry expanding in recent years? Is there any relationship between movie financials and features?
2) Can we predict the box office of a particular movie given some labels?

## Project timeline
### Part I: data scraping (python),data cleaning and data visualization (R)
#### 1.Data scraping
websites that used to get data:
http://www.boxofficemojo.com
http://www.imdb.com
http://www.rottentomatoes.com
variables included:
-  Continuous
  1. Financials:Worldwide, Domestic, Overseas, Openingweekend, theater, weeks (h
ighly correlated)
  2. Budget
  3. Runtime
  4. Rating related:IMDB rating, rotten tomatoes rating, # of reviews
-  Category
  1. Studio
  2. MPAA
  3. Year
  4. Release_Date --> Period
  5. Genre (Genre 1&2)
- Other
  1. Director
  2. Actor1，2
All of them are stored in the scraping folder.
#### 2. Data cleaning
-clean messy data
-split genre
-transfer opening_weekend data to numerical million level,digit=1 and fixxed ow data.
-transfer date data
-clean director and actor data
-clean title,rating  data
#### 3. Data manipulating
Create some new variables in order to make further statistical analysis.
-Period
  1. summer season(ss):late May- late Aug (5-20,8-31)
  2. holiday season(hs):late Nov- Jan (11-20,1-7)
  3. other: (ns)
-Super hero movies
Create a label to distinguish super hero movie or not ($sh)
https://en.wikipedia.org/wiki/List_of_American_superhero_films
1. BV
  1. marvel's avengers
  2. iron man
  3. guardians of the galaxy
  4. the incredibles
  5. captain america
  6. big hero
  7. ant-man
  8. unbreakable
  9. skyhigh
2. WB
  1. the dark knight
  2. man of steel
  3. batman
  4. superman
  5. watchmen
  6. greenlaten
  7. catwoman
  8. green lantern
3. Sony
  1. spiderman
  2. hancock
4. Fox
  1. deadpool
  2. x_men
  3. fantastic four
5. Par.
  1. ironman
  2. SpongeBob
6.Lion
  1. thor
  2. hulk
-Series movie
Create a label to distinguish a series moive or not ($series)
-Add variables from Rotten tomatoes
  1. rotten tomatoes rating
  2. number of reviews
-Create boxoffice/budget variable:($bb)
-Adjusted for inflation (modeling I)
-Quantify movie awards data
-Deal with missing data

#### 4. Data visualization
necessary R packages
-devtools
-knitr
-ggplot2
-easyGgplot2
-scales
##### Movie industry and market visualization
###### The expansion and globalization
1. The industry grows rapidly in recent decades
For more than 600 movies ranked by global box office revenues over 7 decades, most of them are released after year 2000. There might be several reasons:
![alt tag](https://github.com/edsp2016/LexieProject/blob/master/Rproject/pics/year.jpeg)
(1). There is a price inflation over time
(2). Movie industry keeps booming, watching movie has become one important entertainment in people's daily life.
(3).There is a raid overseas movie markets expansion since 21st century.
In order to prove 1 and 2, we use the adjusted data which eliminates price inflation and only focuses on domestic market to see the bar color changes.
![alt tag](https://github.com/edsp2016/LexieProject/blob/master/Rproject/pics/group_by_year.jpeg)
2.Overseas markets expand after year 1987
![alt tag](https://github.com/edsp2016/LexieProject/blob/master/Rproject/pics/over%20seas%20market.jpeg)
3.The effect of price inflation
![alt tag](https://github.com/edsp2016/LexieProject/blob/master/Rproject/pics/inflation.jpeg)
###### The story behind movies
1.The effect of studio and period

| HolidaySeason | SummerSeason |    Other    |
|:-------------:|:------------:|:-----------:|
|      130      |     227      |     275     |


![alt tag](https://github.com/edsp2016/LexieProject/blob/master/Rproject/pics/studio.jpeg)
2. What type of movies do best during the summer vs holiday vs non-season?
![alt tag](https://github.com/edsp2016/LexieProject/blob/master/Rproject/pics/genre_season.jpeg)
![alt tag](https://github.com/edsp2016/LexieProject/blob/master/Rproject/pics/sum_genre_season.jpeg)
![alt tag](https://github.com/edsp2016/LexieProject/blob/master/Rproject/pics/avg_genre_season.jpeg)
Interesitng plot!
The first graph shows the counts of genres in different seasons;
the second shows the sum revenue and the third represents mean box office,respectively.

There are more movies released in summer.
Animation/Action/Sci-Fi movies are popular, especially in summer;
The total revenue distribution of holiday season movie is different from other two seasons.
But foreign movies and horror movies on average earn a lot in holiday season than other two.
3. Do super hero movies always success?
![alt tag](https://github.com/edsp2016/LexieProject/blob/master/Rproject/pics/superhero.jpeg)
4. Do series movie always success?
![alt tag](https://github.com/edsp2016/LexieProject/blob/master/Rproject/pics/series.jpeg)
5. Movie ratings
![alt tag](https://github.com/edsp2016/LexieProject/blob/master/Rproject/pics/ratings.jpeg)
### Part 2: Data analysis and modeling
Potential outcome variables: adjusted domestic boxoffice ($adjusted)
or boxoffice/budget ($bb)
#### Linear regression models
-assumption checked: normality,correlation matrix
![alt tag](https://github.com/edsp2016/LexieProject/blob/master/Rproject/pics/correlation.jpeg)
-variables transformation
![alt tag](https://github.com/edsp2016/LexieProject/blob/master/Rproject/pics/adjusted.jpeg)
![alt tag](https://github.com/edsp2016/LexieProject/blob/master/Rproject/pics/log%20adjusted.jpeg)
-model selection
| Model name    |    Outcome   | Adjusted R^2|     AIC     |
|:-------------:|:------------:|:-----------:|:-----------:|
|      res1     |log(adjusted) |     61.86%  |   75.009    |
|      res2     |   log(bb)    |       57%   |     117     |
|      res3     |log(adjusted) |     55.97%  |    86.20    |
|      res4     |   log(bb)    |     55.43%  |   118.845   |
|      res5     |   adjusted   |     47.73%  |  1171.801   |
|      res6     |      bb      |     45.89%  |  308.9774   |
The first model has the highest adjusted R^2 and the smallest AIC, in this case I choose res 1.
The log adjusted domestic box office revenue is the outcome.
![alt tag](https://github.com/edsp2016/LexieProject/blob/master/Rproject/pics/residuals.jpeg)
#### Regression Decision tree
Regression decision tree is better when facing some missing values.
-necessary R packages
rpart,tree,rpart.plot
-tree model
![alt tag](https://github.com/edsp2016/LexieProject/blob/master/Rproject/pics/rpart_tree.jpeg)

#### random Forest
-necessary R package: randomForest
![alt tag](https://github.com/edsp2016/LexieProject/blob/master/Rproject/pics/forest.jpeg)

|   Model type    |     Outcome    |      MSE      |    Deviance   |
|:---------------:|:--------------:|:-------------:|:-------------:|
|linear regression|  log(adjusted) |      0.05     |      4.79     |
| decision tree   |adjusted/budget |      12.4     |      12.4     |
| random forest   |adjusted/budget |      1.86     |      1.86     |
#### Conclusion
|   Model type    |     Outcome    |    Positive factor    |    Negative factor    |
|:---------------:|:--------------:|:---------------------:|:---------------------:|
|linear regression|  log(adjusted) | rating,series, dirwin |      genre (war)      |
| decision tree   |adjusted/budget |         release_year,studio,genre             |
| random forest   |adjusted/budget | genre, stnomi, mpaa, stwin,studio,runt,rating




