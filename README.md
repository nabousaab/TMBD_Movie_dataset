## Project: TMBD Movie dataset
This data is related to movie industry, and is one of nano-degree projects. 
The objective is to analyze the data, clean it, and drive conclusions and attributes using the available information



## Table of Contents
Introduction
Data Wrangling
Exploratory Data Analysis
Conclusions

## Introduction & worksheet needed
Movie Data: In this worksheep we will try to investigate the different corollation of variables and information available. We will try to investigate relationships patterns, attributes if they are available and some findings from this excersize.

````

Importing the required data
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline


````

## Data Wrangling
Objectives: In this section of the report, My intentions are to understand the data before wrangling it, and then check if we need to do any cleaning or drop some irrilevant or NaN information for this work sheet

## General Properties
Understand & Process Underrstand the information, the shape of the data, any missing info, find the mean to measure any corollation relevane between the data

Load the data to undrstand its information

````

df_movies = pd.read_csv('tmdb-movies.csv')
df_movies.head()

````


Checking number of rows and columns

````

df_movies.shape
(10866, 21)
Looking for any missing data
df_movies.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 10866 entries, 0 to 10865
Data columns (total 21 columns):
id                      10866 non-null int64
imdb_id                 10856 non-null object
popularity              10866 non-null float64
budget                  10866 non-null int64
revenue                 10866 non-null int64
original_title          10866 non-null object
cast                    10790 non-null object
homepage                2936 non-null object
director                10822 non-null object
tagline                 8042 non-null object
keywords                9373 non-null object
overview                10862 non-null object
runtime                 10866 non-null int64
genres                  10843 non-null object
production_companies    9836 non-null object
release_date            10866 non-null object
vote_count              10866 non-null int64
vote_average            10866 non-null float64
release_year            10866 non-null int64
budget_adj              10866 non-null float64
revenue_adj             10866 non-null float64
dtypes: float64(4), int64(6), object(11)
memory usage: 1.7+ MB

````


Reading different data matrix to understand any top line correlation

````

df_movies.describe()

id	popularity	budget	revenue	runtime	vote_count	vote_average	release_year	budget_adj	revenue_adj
count	10866.000000	10866.000000	1.086600e+04	1.086600e+04	10866.000000	10866.000000	10866.000000	10866.000000	1.086600e+04	1.086600e+04
mean	66064.177434	0.646441	1.462570e+07	3.982332e+07	102.070863	217.389748	5.974922	2001.322658	1.755104e+07	5.136436e+07
std	92130.136561	1.000185	3.091321e+07	1.170035e+08	31.381405	575.619058	0.935142	12.812941	3.430616e+07	1.446325e+08
min	5.000000	0.000065	0.000000e+00	0.000000e+00	0.000000	10.000000	1.500000	1960.000000	0.000000e+00	0.000000e+00
25%	10596.250000	0.207583	0.000000e+00	0.000000e+00	90.000000	17.000000	5.400000	1995.000000	0.000000e+00	0.000000e+00
50%	20669.000000	0.383856	0.000000e+00	0.000000e+00	99.000000	38.000000	6.000000	2006.000000	0.000000e+00	0.000000e+00
75%	75610.000000	0.713817	1.500000e+07	2.400000e+07	111.000000	145.750000	6.600000	2011.000000	2.085325e+07	3.369710e+07
max	417859.000000	32.985763	4.250000e+08	2.781506e+09	900.000000	9767.000000	9.200000	2015.000000	4.250000e+08	2.827124e+09

````

````

df_movies.hist(figsize=(15,15));

pd.plotting.scatter_matrix(df_movies, figsize=(16,16));

````

Checking what 'keywords' contains


````

df_movies.keywords.unique()

array(['monster|dna|tyrannosaurus rex|velociraptor|island',
       'future|chase|post-apocalyptic|dystopia|australia',
       'based on novel|revolution|dystopia|sequel|dystopic future', ...,
       'car race|racing|formula 1', 'car|trolley|stealing car',
       'fire|gun|drive|sacrifice|flashlight'], dtype=object)

````


## Some findings from data wrangling
The dataframe need to be cleaned since there are too many NaN and zero cells. I do not wish to replace the missing numbers with the mean since the missing information is more than half of the data available; adding the mean will surely disturb and scew the analisys towards one end.

I have also notcied from the dataframe charts provides so far limitted rlevant information (the year where more of the movies were released), or probably this information is been stored more acurately after year 2010. Other relations between budget, vote, and revenue is not clear (we will revisit this once we clean the data at a later stage)

So in the next chapter, I will drop some rows and columns to make my workframe more focused

Dropping unwanted columns, and then checking result


````

df_movies.drop(['id','imdb_id','homepage','tagline','keywords', 'overview'], axis=1, inplace=True)
df_movies.head()

````


Chekcing missing and duplicate data in the rows and treating them

````

df_movies.info(), df_movies.shape



<class 'pandas.core.frame.DataFrame'>
RangeIndex: 10866 entries, 0 to 10865
Data columns (total 15 columns):
popularity              10866 non-null float64
budget                  10866 non-null int64
revenue                 10866 non-null int64
original_title          10866 non-null object
cast                    10790 non-null object
director                10822 non-null object
runtime                 10866 non-null int64
genres                  10843 non-null object
production_companies    9836 non-null object
release_date            10866 non-null object
vote_count              10866 non-null int64
vote_average            10866 non-null float64
release_year            10866 non-null int64
budget_adj              10866 non-null float64
revenue_adj             10866 non-null float64
dtypes: float64(4), int64(5), object(6)
memory usage: 1.2+ MB
(None, (10866, 15))

````

````

sum(df_movies.duplicated())
1

````

````

df_movies.drop_duplicates(inplace=True)


````

````

sum(df_movies.duplicated())
0

````


## I have noticed too many missing data and decided to drop them instead of getting their mean. Missing data is more than half of the available data.


````

df_movies.isnull().sum()

popularity                 0
budget                     0
revenue                    0
original_title             0
cast                      76
director                  44
runtime                    0
genres                    23
production_companies    1030
release_date               0
vote_count                 0
vote_average               0
release_year               0
budget_adj                 0
revenue_adj                0
dtype: int64

````


# Discover the zero number rows to drop next steps
# there are more than 7,000 raws with zero value.
# best option is to drop this and not to replace them with the mean, as this will provide a substantial misslead in results

````

df_movies.query('budget == "0"')+df_movies.query('revenue =="0"')
df_movies.head()

````


Replacing the zero numbers with NaN todrop them later on.

````

df_movies.dropna(inplace=True)

````


checking the cleaning process, and it is done

````

df_movies.info()

<class 'pandas.core.frame.DataFrame'>
Int64Index: 9772 entries, 0 to 10865
Data columns (total 15 columns):
popularity              9772 non-null float64
budget                  9772 non-null int64
revenue                 9772 non-null int64
original_title          9772 non-null object
cast                    9772 non-null object
director                9772 non-null object
runtime                 9772 non-null int64
genres                  9772 non-null object
production_companies    9772 non-null object
release_date            9772 non-null object
vote_count              9772 non-null int64
vote_average            9772 non-null float64
release_year            9772 non-null int64
budget_adj              9772 non-null float64
revenue_adj             9772 non-null float64
dtypes: float64(4), int64(5), object(6)
memory usage: 1.2+ MB

````

````


df_movies.isnull().sum()


popularity              0
budget                  0
revenue                 0
original_title          0
cast                    0
director                0
runtime                 0
genres                  0
production_companies    0
release_date            0
vote_count              0
vote_average            0
release_year            0
budget_adj              0
revenue_adj             0
dtype: int64


````


## Exploratory Data Analysis
Does bigger Budget has positive relation with Revenue, same thing with Popularity?
It is clear that there is somehow a positive relation betweeh budget and reveune but not as strong as it is been expected
# it is also not relvant to say that the more popular the higher teh revenue should be
# same applies for the average vote, as relatively they do not determine more push towards higher revenue  

````
df_movies.plot(x='popularity', y='revenue', kind='scatter', figsize=(10,6))
df_movies.plot(x='vote_average', y='revenue', kind='scatter', figsize=(10,6))
df_movies.plot(x='budget', y='revenue', kind='scatter', figsize=(10,6))

````




What if we look at top budgeted movies
# would this relationship change

````


top_budget = df_movies.query('budget > budget.mean()')
top_budget.describe()

````

````

top_budget.revenue.plot(kind="box", figsize=(10,6))
plt.title('Revenue distribution across all movies')
plt.xlabel("Mean distribution accross top movies")
plt.ylabel("Total Billion USD ");

````


According to the mean, median and quartel figuer, the revenue level is at a lower distribution point.
This shows that even movies spending money above the average, they generate less revnue
With the exception of few movies (check the chart below
Amongst the top budgetd movies it is obvious that there is a higher relation with regards to revenue generated


##Some of them did exceeded 10X revenue 

````

top_budget.revenue.hist(alpha=0.7, figsize=(10,6), label="revenue")
top_budget.budget.hist(alpha=0.7, figsize=(10,6), label='budget')
plt.legend();

````


Top movies with highest revenue ever generated for their respective years.

````

top_movies=df_movies.groupby('release_year').max()[['original_title', 'revenue']]
top_movies


original_title	revenue
release_year		
1960	Village of the Damned	60000000
1961	West Side Story	215880014
1962	What Ever Happened to Baby Jane?	70000000
1963	X: The Man with the X-Ray Eyes	78898765
1964	Zulu	124900000
1965	What's New Pussycat?	163214286
1966	Who's Afraid of Virginia Woolf?	33736689
1967	You Only Live Twice	205843612
1968	Yours, Mine and Ours	56715371
1969	Women in Love	102308889
1970	Zabriskie Point	136400000
1971	Willy Wonka & the Chocolate Factory	116000000
1972	What's Up, Doc?	245066411
1973	Westworld	441306145
1974	Zardoz	119500000
1975	Trilogy of Terror	470654000
1976	Î¤Î± Ï€Î±Î¹Î´Î¹Î¬ Ï„Î¿Ï… Î´Î¹Î±Î²ÏŒÎ»Î¿Ï…	161000000
1977	Wizards	775398007
1978	Watership Down	300218018
1979	When a Stranger Calls	210300000
1980	Xanadu	538400000
1981	Wolfen	389925971
1982	Zapped!	792910554
1983	Zelig	572700000
1984	Under the Volcano	333000000
1985	Young Sherlock Holmes	381109762
1986	Â¡Three Amigos!	356830601
1987	Withnail & I	320145693
1988	é»‘å¤ªé™½731	354825435
1989	Wild Orchid	474171806
1990	Young Guns II	505000000
1991	Zandalee	520000000
1992	Wuthering Heights	504050219
1993	å¤ªæžå¼ ä¸‰ä¸°	920100000
1994	ã‚´ã‚¸ãƒ©vsã‚¹ãƒšãƒ¼ã‚¹ã‚´ã‚¸ãƒ©	788241776
1995	ç»™çˆ¸çˆ¸çš„ä¿¡	1106279658
1996	Wish Upon a Star	816969268
1997	Wishmaster	1845034188
1998	Zero Effect	553799566
1999	æˆé¾çš„ç‰¹æŠ€	924317558
2000	You Can Count on Me	546388105
2001	Zoolander	976475550
2002	Ã”nibus 174	926287400
2003	Young Adam	1118888979
2004	ã‚¢ãƒƒãƒ—ãƒ«ã‚·ãƒ¼ãƒ‰	919838758
2005	í˜•ì‚¬ Duelist	895921036
2006	ì§íŒ¨	1065659812
2007	Zodiac	961000000
2008	æ±äº¬æ®‹é…·è­¦å¯Ÿ	1001921825
2009	Zombieland	2781505847
2010	í¬í™” ì†ìœ¼ë¡œ	1063171911
2011	è³½å¾·å…‹Â·å·´èŠ (ä¸Š) å¤ªé™½æ——	1327817822
2012	í•˜ìš¸ë§	1519557910
2013	ìºì¹˜ë¯¸	1274219009
2014	ç­‰ä¸€å€‹äººå’–å•¡	955119788
2015	Zipper	2068178225

````

````

top_movies.plot(x='original_title', y="revenue", kind='bar', figsize=(20, 6))
plt.title('Top movies in revenue USD as per their respective years')
plt.ylabel('total revenue - USD Billion');

````


checking which movie is the top reveue ever from all list


````

top_movies.describe()

revenue
count	5.600000e+01
mean	6.515554e+08
std	5.466766e+08
min	3.373669e+07
25%	2.144850e+08
50%	5.125000e+08
75%	9.334955e+08
max	2.781506e+09

````


````

all_time = top_movies['revenue'].max()
all_time
2781505847

````

## Zombieland was all time top revenue movie since 1960 till date

````

top_movies[top_movies['revenue'] == all_time]

original_title	revenue
release_year		
2009	Zombieland	2781505847


````


````

zombieland = df_movies.query('original_title == "Zombieland"').max()

zombieland
popularity                                                         2.0418
budget                                                           23600000
revenue                                                         102391382
original_title                                                 Zombieland
cast                    Jesse Eisenberg|Woody Harrelson|Emma Stone|Abi...
director                                                  Ruben Fleischer
runtime                                                                88
genres                                                      Comedy|Horror
production_companies                                    Columbia Pictures
release_date                                                      10/7/09
vote_count                                                           2165
vote_average                                                          7.1
release_year                                                         2009
budget_adj                                                    2.39871e+07
revenue_adj                                                   1.04071e+08
dtype: object


````


Comparing Zombieland figures (Budget, Revenue, and Profit)

````

z_budget = zombieland.budget
z_revenue = zombieland.revenue
z_profit = zombieland.revenue - zombieland.budget
z_budget, z_revenue, z_profit
(23600000, 102391382, 78791382)

````


Undestand where does profit stands in term ot total budget and revenue

````

plt.subplots(figsize=(15, 6))
plt.bar(["z_revenue","z_budget", "z_profit"], [z_revenue, z_budget, z_profit])

````


````

plt.title("Profit generated compared to the revenue")
plt.xlabel("Item name")
plt.ylabel("Total Billion USD ");

````



Zombieland profit ratio is 4 times it budget

````

profit_ratio = z_revenue/z_budget
profit_ratio
4.338617881355932

````



## TMDB Movies Conclusions
Data Limitation: The avaiable data does not provide us with enogh correleation between top rated movies and teh revenue generated. Thus, concluding that revenue id directly effected by the popularity or the movie, or the ratings is not suffecient and conclusive. it is indeed that positive rating of the movie would help increasing the revenue, but again, sine probably the viewing behavior happens in real time and is effected by digital media and is effected by viewers mode at that time. It is not conculsive to have such a trend that can help forcast movie genres for future years. such a conclusion would require more information about consumer behavior, likability, and ratings combined.

Over All: The postive relation between high rating and poularity is not as strong and positive as we expected. There are movies that generated higher revenue with much lower scores in terms of likability

there is some how a shy relationship between budget spent on movies vs their revenue, surely this is dominated by big players and can be viewed in a clearer way once you focus on production companies spending higher than average industry cost.

top movies: it is clear that after 2008 all top movies listed in their correspondance years, have scored higher by one additional diggit. Thus more investigation should be done on consumer behavior, the influx of OTT, and also the ratio of budgets spend vs. previous years on producing movies.

The top perfomaning movie of all times was Zombieland, although the movie did not over in terms of popularity compared to over all mean.popularity of movies and the movie with max populartity. Hoever in terms of scoring by viewers it only overscorred by 2.2 times over the average.mean eveluation of viewers. While it still registered a 10% lower score compared to highest number.

In trums of profitability: Zombieland had scored 4 times more than its budget and stands as the highest movie genrating revenue since 2009 to 2015.
