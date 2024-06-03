# StreamTunes Clean and manipulation

- The Google Sheet process and SQL queries outlined below demonstrate the process of cleaning, manipulating, and preparing the Spotify Dataset for further analysis in R. The dataset was thoroughly checked for data quality issues, duplicates, and null values.

- The dataset is sourced from Kaggle and has a comprehensive list of the most famous songs of 2023 as listed on Spotify.
- The data includes The table `spotifydata` in the `music` dataset contains detailed measurements including track name, artist name, number of contributing artists, release date, presence in and rank on Spotify, Apple Music, Deezer, and Shazam 
  playlists and charts, total streams on Spotify, beats per minute (bpm), song key and mode, and various audio features such as danceability, valence, energy, acousticness, instrumentalness, liveness, and speechiness percentages.
- There are 952 total songs within the table.

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

## SQL Cleaning and Manipulation
#### Removing "Love Grows (Where My Rosemary Goes)" record
```sql
DELETE FROM music.spotifydata
WHERE streams = 'BPM110KeyAModeMajorDanceability53Valence75Energy69Acousticness7Instrumentalness0Liveness17Speechiness3';
```
#### Filling Missing Values
```sql
UPDATE music.musicdata
SET inshazamcharts = COALESCE(inshazamcharts, 0),
    key = COALESCE(key, 'Unknown')
WHERE TRUE;
```
# SQL Analysis

####  Top 5 Artists with most streams
```SQL
SELECT artistname,sum(streams) AS total_streams
FROM music.spotifydata
GROUP BY artistname
ORDER BY total_streams DESC
LIMIT 5;
```

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
LIMIT 100
)
GROUP BY key_count
ORDER BY key_count DESC
LIMIT 10;
```

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

