# TMBD_Movie_dataset
## Analyise the power of studio production companies

**the genre axis**
> Under the Genre colum they listed all collaborating studies
> So i wanted to split this row to be able to drow conculusion if there is any success or power story behind top_movies per year the studio name.
> And I used this code

````

df_movies['First_Genre'] = df_movies['genres'].str.split('|')
df_movies['First_Genre'] = df_movies['First_Genre'].str[0]

````

> But in this code I am only getting the first Genere stated as First_Genre
> which will give me only the studio name mentioned at the begening. Whish is nt idea for my analysis objective (stated above)

## The question:
**what is the code I can use to split the the studio names under Genere, whithout creating a duplication of the cost and revenue ?**
_especially that the number of studio names under Genre is not consitant and would create another missing values if I opted to create Genre1, Genr2, Genre3, etc ... columns_

If you like to check the full worksheet link pls [Click here](file:///Users/nizarabousaab/Downloads/investigate-a-dataset-template%20(1).html)
