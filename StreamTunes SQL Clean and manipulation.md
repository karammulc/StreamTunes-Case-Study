Background: A major music streaming platform, StreamTunes, aims to enhance its user experience and increase user engagement by leveraging data-driven insights. The company has acquired a comprehensive dataset containing information about the most famous songs of 2023 on Spotify,
including track attributes, popularity, and presence on various music platforms.


Objectives:
- Identify key factors that contribute to a song's popularity and success across different music platforms.
- Develop strategies to optimize StreamTunes' playlist curation and song recommendations based on user preferences and trends.
- Explore opportunities for cross-platform partnerships and promotions to expand StreamTunes' user base and market share.


# StreamTunes Clean and manipulation

- The Google Sheet process and SQL queries outlined below demonstrate the process of cleaning, manipulating, and preparing the Spotify Dataset for further analysis in R. The dataset was thoroughly checked for data quality issues, duplicates, and null values.

- The dataset is sourced from Kaggle and has a comprehensive list of the most famous songs of 2023 as listed on Spotify.
- The data includes The table `spotifydata` in the `music` dataset contains detailed measurements including track name, artist name, number of contributing artists, release date, presence in and rank on Spotify, Apple Music, Deezer, and Shazam 
  playlists and charts, total streams on Spotify, beats per minute (bpm), song key and mode, and various audio features such as danceability, valence, energy, acousticness, instrumentalness, liveness, and speechiness percentages.
- There are 952 total songs within the table.



| New Column  | Original Column         |
|-----------------|---------------------|
| trackname       | track_name          |
| artistname      | artist_name         |
| artistcount     | artist_count        |
| releasedyear    | released_year       |
| releasedmonth   | released_month      |
| releasedday     | Released_day        |
| inspotifyplaylists | in_spotify_playlists |
| inspotifycharts | in_spotify_charts   |
| streams         | streams             |
| inappleplaylists | in_apple_playlists  |
| inapplecharts   | in_apple_charts     |
| indeezerplaylists | in_deezer_playlists |
| indeezercharts  | in_deezer_charts    |
| inshazamcharts  | in_shazam_charts    |
| bpm             | bpm                 |
| key             | key                 |
| mode            | mode                |
| danceability    | danceability_%      |
| valence         | valence_%           |
| energy          | energy_%            |

# Google Sheets Cleaning and Manipulation  

### Data Cleaning and Preparation
1. Column Data Type Conversion:

- Converted the columns streams, indeezerplaylists, and inshazamcharts to numeric data types in Google Sheets.

2. Handling Corrupted Characters:

- Discovered corrupted characters (�) in many songtitles and artistname fields.
- Used the SUBSTITUTE function to replace corrupted characters:

=SUBSTITUTE(A2, "�", "")

- Created new columns and pasted the cleaned values from the function results.
- Deleted unnecessary columns that contained corrupted characters or functions.

3. Addressing Null Values:
- Used filters to identify null values in trackname and artistname columns.
- For two records with null trackname, replaced the values with "unknown":
(artistname = YOASOBI
artistname = FUJII KAZE)

#### Other null values will be addressed through SQL cleaning.

4. Numerical/Date Formatting:

- Applied numerical and date formatting to the releasedyear column.

#### Community Contribution 
During the review of the dataset discussion, user paulabsmanner identified a corrupted streams value for the song "Love Grows (Where My Rosemary Goes)".
Located and removed this corrupted record.
Acknowledgment and thanks to the Kaggle community for their contributions!

#SQL Cleaning and Manipulation
## Removing "Love Grows (Where My Rosemary Goes)" record
```sql
DELETE FROM music.spotifydata
WHERE streams = 'BPM110KeyAModeMajorDanceability53Valence75Energy69Acousticness7Instrumentalness0Liveness17Speechiness3';
```
## Filling Missing Values
```sql
UPDATE music.musicdata
SET inshazamcharts = COALESCE(inshazamcharts, 0),
    key = COALESCE(key, 'Unknown')
WHERE TRUE;
```
# SQL Analysis

## Finding Top 5 Artists With Highest Average Total Streams 
```SQL
SELECT s.artistname, AVG(t.total_streams) AS avg_total_streams
FROM music.spotifydata s
JOIN (
  SELECT artistname, SUM(streams) AS total_streams
  FROM music.spotifydata
  GROUP BY artistname
) t ON s.artistname = t.artistname
GROUP BY s.artistname
ORDER BY avg_total_streams DESC
LIMIT 5;`
```


## Average Danceability and Energy by Key
```sql

SELECT key, AVG(danceability) AS avg_danceability, AVG(energy) AS avg_energy
FROM music.spotifydata
GROUP BY key;
```

| Key     | Avgerage Danceability | Average Energy           |
|---------|----------------------|----------------------|
| D       | 67.80246913580244    | 63.530864197530867   |
| Unknown | 64.378947368421066   | 63.684210526315752   |
| B       | 69.49382716049378    | 68.0                 |
| A#      | 68.7894736842105     | 62.78947368421052    |
| A       | 64.108108108108127   | 60.094594594594582   |
| F#      | 68.232876712328775   | 66.726027397260253   |
| F       | 67.101123595505584   | 64.382022471910062   |
| G       | 67.447916666666657   | 63.71875000000005    |
| G#      | 66.373626373626351   | 64.0659340659341     |
| C#      | 68.6416666666667     | 66.550000000000026   |
| E       | 65.032258064516114   | 62.11290322580647    |
| D#      | 64.545454545454561   | 62.8484848484848     |

```sql
SELECT artistname, trackname, streams
FROM music.spotifydata
ORDER BY streams DESC
LIMIT 10;
```


| Artist Name                      | Track Name                                 | Streams    |
|---------------------------------|-------------------------------------------|------------|
| Taylor Swift                    | Anti-Hero                                 | 999748277  |
| Duncan Laurence                 | Arcade                                    | 991336132  |
| Joji                            | Glimpse of Us                             | 988515741  |
| Lana Del Rey                    | Summertime Sadness                        | 983637508  |
| SZA                             | Seek & Destroy                            | 98709329   |
| Lost Frequencies, Calum Scott   | Where Are You Now                         | 972509632  |
| The Walters                     | I Love You So                             | 972164968  |
| Sofia Carson                    | Come Back Home - From "Purple Hearts"     | 97610446   |
| (G)I-DLE                        | Queencard                                 | 96273746   |
| The Weeknd, Future              | Double Fantasy (with Future)              | 96180277   |

## What is the most common key in the top ranking 100 songs?

#### 1. Select the top 100 songs based on a certain criteria (e.g., streams, popularity).
#### 2. Group the top 100 songs by their key.
#### 3. Count the occurrence of each key.
#### 4. Order the results by the count in descending order.
#### 5. Limit the output to the top result.

```SQL
SELECT key, COUNT(*) AS key_count
FROM (
  SELECT key
FROM music.spotifydata
ORDER BY streams DESC
LIMIT 100
)
GROUP BY key_count
ORDER BY key_count DESC
LIMIT 10;
```

| Key                                 | Key Count                             | 
|---------------------------------|-------------------------------------------|
| C#                              | 19                                        | 
| A                               | 11                                        | 
| F#                              | 11                                        | 
| E                               | 9                                         | 
| G                               | 8                                         | 
| F                               | 7                                         |
| G#                              | 7                                         | 
| B                               | 5                                         |
| A                               | 5                                         | 
| Unknown                         | 5                                         | 

#### R Vizualizations

```{r}
library(tidyverse)
library(lubridate)
library(ggplot2)
```

#### Loading data set
```{r}
spotifydata <- read.csv("spotify.csv")
```

```{r}
str(spotifydata)
summary(spotifydata)
```

```{r}
str(spotifydata)
```

#### Distribution of bpm
```{r}
hist(spotifydata$bpm, main = "Distribution of bpm", xlab = "bpm")
```

#### Distribution of Key
```{r}
hist(spotifydata$bpm, main = "Distribution of key", xlab = "key")
```

#### Distribution of Streams by Mode
```{r}
library(ggplot2)

# Create a histogram with layered densities for different 'mode' categories
ggplot(spotifydata, aes(x = streams, fill = mode)) +
  geom_histogram(alpha = 0.5, position = "identity", bins = 30) +
  scale_fill_manual(values = c("blue", "red")) +
  labs(title = "Distribution of Streams by Mode",
       x = "Streams",
       y = "Frequency",
       fill = "Mode") +
  theme_minimal()
```


#Correlation Matrix
```{r}
correlation_matrix <- cor(spotifydata_cleaned_complete)
print(correlation_matrix)
```

