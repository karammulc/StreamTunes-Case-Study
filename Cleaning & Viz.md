# StreamTunes Clean and manipulation

- The Google Sheet process and SQL queries outlined below demonstrate the process of cleaning, manipulating, and preparing the Spotify Dataset for further analysis in R. The dataset was thoroughly checked for data quality issues, duplicates, and null values.

- The dataset is sourced from Kaggle and has a comprehensive list of the most famous songs of 2023 as listed on Spotify.
- The data includes The table `spotifydata` in the `music` dataset contains detailed measurements including track name, artist name, number of contributing artists, release date, presence in and rank on Spotify, Apple Music, Deezer, and Shazam 
  playlists and charts, total streams on Spotify, beats per minute (bpm), song key and mode, and various audio features such as danceability, valence, energy, acousticness, instrumentalness, liveness, and speechiness percentages.
- There are 952 total songs within the table.

# Google Sheets Cleaning and Manipulation  

## Table Variables

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
During the review of the dataset discussion, I found that user paulabsmanner identified a corrupted streams value for the song "Love Grows (Where My Rosemary Goes)".
I located and removed this corrupted record.
Acknowledgment and thanks to the Kaggle community for their contributions!

## SQL Cleaning and Manipulation

#### Removing "Love Grows (Where My Rosemary Goes)" record
```sql
DELETE FROM music.spotifydata
WHERE streams = 'BPM110KeyAModeMajorDanceability53Valence75Energy69Acousticness7Instrumentalness0Liveness17Speechiness3';
```
#### Filling Missing Values

#### Counting Nulls in Each Column 

```SQL
SELECT
  SUM(CASE WHEN trackname IS NULL THEN 1 ELSE 0 END) AS trackname_nulls,
  SUM(CASE WHEN artistname IS NULL THEN 1 ELSE 0 END) AS artistname_nulls,
  SUM(CASE WHEN artistcount IS NULL THEN 1 ELSE 0 END) AS artistcount_nulls,
  SUM(CASE WHEN releasedyear IS NULL THEN 1 ELSE 0 END) AS releasedyear_nulls,
  SUM(CASE WHEN releasedmonth IS NULL THEN 1 ELSE 0 END) AS releasedmonth_nulls,
  SUM(CASE WHEN releasedday IS NULL THEN 1 ELSE 0 END) AS releasedday_nulls,
  SUM(CASE WHEN inspotifyplaylists IS NULL THEN 1 ELSE 0 END) AS inspotifyplaylists_nulls,
  SUM(CASE WHEN inspotifycharts IS NULL THEN 1 ELSE 0 END) AS inspotifycharts_nulls,
  SUM(CASE WHEN streams IS NULL THEN 1 ELSE 0 END) AS streams_nulls,
  SUM(CASE WHEN inappleplaylists IS NULL THEN 1 ELSE 0 END) AS inappleplaylists_nulls,
  SUM(CASE WHEN inapplecharts IS NULL THEN 1 ELSE 0 END) AS inapplecharts_nulls,
  SUM(CASE WHEN indeezerplaylists IS NULL THEN 1 ELSE 0 END) AS indeezerplaylists_nulls,
  SUM(CASE WHEN indeezercharts IS NULL THEN 1 ELSE 0 END) AS indeezercharts_nulls,
  SUM(CASE WHEN inshazamcharts IS NULL THEN 1 ELSE 0 END) AS inshazamcharts_nulls,
  SUM(CASE WHEN bpm IS NULL THEN 1 ELSE 0 END) AS bpm_nulls,
  SUM(CASE WHEN key IS NULL THEN 1 ELSE 0 END) AS key_nulls,
  SUM(CASE WHEN mode IS NULL THEN 1 ELSE 0 END) AS mode_nulls,
  SUM(CASE WHEN danceability IS NULL THEN 1 ELSE 0 END) AS danceability_nulls,
  SUM(CASE WHEN valence IS NULL THEN 1 ELSE 0 END) AS valence_nulls,
  SUM(CASE WHEN energy IS NULL THEN 1 ELSE 0 END) AS energy_nulls,
  SUM(CASE WHEN acousticness IS NULL THEN 1 ELSE 0 END) AS acousticness_nulls,
  SUM(CASE WHEN instrumentalness IS NULL THEN 1 ELSE 0 END) AS instrumentalness_nulls,
  SUM(CASE WHEN liveness IS NULL THEN 1 ELSE 0 END) AS liveness_nulls,
  SUM(CASE WHEN speechiness IS NULL THEN 1 ELSE 0 END) AS speechiness_nulls
FROM music.spotifydata;
```
#### Columns with nulls 
| trackname_nulls | inshazamcharts nulls | key nulls  | 
|-----------------|------------------|-------------------------|
| 2               | 50               | 95                      |


#### Updating String Columns with Nulls 
```sql
UPDATE music.spotifydata
SET trackname = 'Unknown'
WHERE trackname IS NULL;

UPDATE music.spotifydata
SET key = 'Unknown'
WHERE key IS NULL;
```
#### I will not be using the inshazamcharts variable for analysis, so I am dropping this column altogether.

```sql
ALTER TABLE music.spotifydata
DROP COLUMN inshazamcharts;
```

##  SQL Analysis

####  Top 5 Artists with most streams
```SQL
SELECT artistname,sum(streams) AS total_streams
FROM music.spotifydata
GROUP BY artistname
ORDER BY total_streams DESC
LIMIT 5;
```
|     | artistname  | total_streams |
|-----|--------------|----------------|
| 1   | The Weeknd  | 14185552870   |
| 2   | Taylor Swift| 14053658300   |
| 3   | Ed Sheeran  | 13908947204   |
| 4   | Harry Styles| 11608645649   |
| 5   | Bad Bunny   | 9997799607    |

#### Average Danceability and Energy by Key
```sql

SELECT key, AVG(danceability) AS avg_danceability, AVG(energy) AS avg_energy
FROM music.spotifydata
GROUP BY key;
```

#### Most Listened to Songs 
```sql
SELECT artistname, trackname, streams
FROM music.spotifydata
ORDER BY streams DESC
LIMIT 10;
```


#### Finding most common Keys in top 100 songs

-  To achieve this, I selected the top 100 songs ordered by streams, grouped by key, and counted the occurrence of each key. Lastly, I ordered the count in descending order.
```SQL
SELECT key, COUNT(*) AS key_count
FROM (
  SELECT key
  FROM music.spotifydata
  ORDER BY streams DESC
) AS subquery
GROUP BY key
ORDER BY key_count DESC
LIMIT 10;
```
| Row | key | key_count |
|-----|-----|-----------|
| 1   | C#  | 120       |
| 2   | G   | 96        |
| 3   | G#  | 91        |
| 4   | F   | 89        |
| 5   | D   | 81        |
| 6   | B   | 81        |
| 7   | A   | 74        |
| 8   | F#  | 73        |
| 9   | E   | 62        |
| 10  | A#  | 57        |

```Count of charted songs by mode (Major,Minor)
SELECT mode, COUNT(mode) AS mode_count
FROM `music.spotifydata`
GROUP BY mode;
```
| Row | mode  | mode_count |
|-----|-------|------------|
| 1   | Minor | 403        |
| 2   | Major | 549        |

## R Vizualizations

#### Loading Libraries
```{r}
library(tidyverse)
library(lubridate)
library(ggplot2)
```

#### Loading Data Set
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

#### Distribution of BPM
```{r}
hist(spotifydata$bpm, main = "Distribution of bpm", xlab = "bpm")
```

#### Distribution of Key
```{r}
ggplot(spotifydata, aes(x = key)) +
  geom_bar(fill = "steelblue") +
  labs(title = "Distribution of Songs by Key",
       x = "Key",
       y = "Count") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

#### Distribution of Streams by Mode
```{r}
ggplot(spotifydata, aes(x = streams, fill = mode)) +
  geom_histogram(alpha = 0.5, position = "identity", bins = 30) +
  scale_fill_manual(values = c("blue", "red")) +
  labs(title = "Distribution of Streams by Mode",
       x = "Streams",
       y = "Frequency",
       fill = "Mode") +
  theme_minimal()
```

#### Distribution of Songs by Release Year
```{r}
ggplot(spotifydata, aes(x = releasedyear)) +
  geom_histogram(binwidth = 10, fill = "steelblue", color = "white") +
  labs(title = "Distribution of Songs by Release Year",
       x = "Release Year",
       y = "Count") +
  theme_minimal()
```
#### Top Ten Artists Variable Assignment
```{r}
top_artists <- spotifydata %>% 
  count(artistname, sort = TRUE ) %>% 
  head(10)
```

#### Top Artists by Number of Songs
```{r}
ggplot(top_artists, aes(x = reorder(artistname, n), y = n)) +
  geom_bar(stat = "identity", fill = "steelblue") +
  coord_flip() +
  labs(title = "Top Artists by Number of Songs",
       x = "Artist",
       y = "Number of Songs") +
  theme_minimal()
```

#### Correlation Matrix
```{r}
correlation_matrix <- cor(spotifydata)
print(correlation_matrix)
```

