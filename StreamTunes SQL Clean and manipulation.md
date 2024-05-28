# StreamTunes SQL Clean and manipulation

- The Google Sheet process and SQL queries outlined below demonstrate the process of cleaning, manipulating, and preparing the Spotify Dataset for further analysis in R. The dataset was thoroughly checked for data quality issues, duplicates, and null values.

- The dataset is sourced from Kaggle and has a comprehensive list of the most famous songs of 2023 as listed on Spotify.
- The data includes The table `spotifydata` in the `music` dataset contains detailed measurements including track name, artist name, number of contributing artists, release date, presence in and rank on Spotify, Apple Music, Deezer, and Shazam 
  playlists and charts, total streams on Spotify, beats per minute (bpm), song key and mode, and various audio features such as danceability, valence, energy, acousticness, instrumentalness, liveness, and speechiness percentages.

- The dataset spans from April 12, 2016, to May 12, 2016, providing a single month of data.

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
## Filling Missing Values
```sql
UPDATE music.musicdata
SET inshazamcharts = COALESCE(inshazamcharts, 0),
    key = COALESCE(key, 'Unknown')
WHERE TRUE;
```



#### Checking the consistency of distinct user IDs across all tables:
 ```sql

``` 
### Result: Counts differed across some tables.

| Dataset           | Distinct IDs |
|-------------------|--------------|
| dailyactivity     | 33           |
| dailycalories     | 33           |
| dailyintensities  | 33           |
| dailysteps        | 33           |
| hourlyintensities | 33           |
| hourlysteps       | 33           |
| sleepday          | 24           |
| weightlog         | 8            |



## Checking for Duplicates

Duplicate check - dailycalories
```sql
SELECT Id, ActivityDay, Calories, Count(*)
FROM bellabeat.dailycalories
GROUP BY Id, ActivityDay, Calories
HAVING Count(*) > 1;
```

Duplicate check - dailyactivity   
```sql SELECT Id, ActivityDate, TotalSteps, Count(*)
FROM bellabeat.dailyactivity
GROUP BY Id, ActivityDate, TotalSteps
HAVING Count(*) > 1;
```

Duplicate check - dailyintensities
```sql
SELECT Id,
      ActivityDay,
      SedentaryMinutes,
      LightlyActiveMinutes,
      FairlyActiveMinutes,
      VeryActiveMinutes,
      SedentaryActiveDistance,
      LightActiveDistance,
      ModeratelyActiveDistance,
      VeryActiveDistance,
      COUNT(*)
FROM `bellabeat.dailyintensities`
GROUP BY Id,
        ActivityDay,
        SedentaryMinutes,
        LightlyActiveMinutes,
        FairlyActiveMinutes,
        VeryActiveMinutes,
        SedentaryActiveDistance,
        LightActiveDistance,
        ModeratelyActiveDistance,
        VeryActiveDistance
HAVING COUNT(*) > 1;
```

Duplicate check - hourlysteps
```sql
SELECT Id, ActivityHour, StepTotal, Count(*)
FROM `bellabeat.hourlysteps`
GROUP BY Id, ActivityHour, StepTotal
HAVING Count(*) > 1;
```
Duplicate check - sleepday
```sql
SELECT Id, 
       SleepDay,
       TotalSleepRecords,
       TotalMinutesAsleep,
       TotalTimeInBed, 
       Count(*)
FROM `bellabeat.sleepday`
GROUP BY Id, 
         SleepDay,
         TotalSleepRecords,
         TotalMinutesAsleep,
         TotalTimeInBed
HAVING Count(*) > 1;
```
*Result: Duplicates were found only in sleepday table.*

| Id         | SleepDay                    | TotalSleepRecords | TotalMinutesAsleep | TotalMinutesInBed | Count of Duplicates |
|------------|-----------------------------|-------------------|--------------------|-------------------|---------------------|
| 4388161847 | 2016-05-05 00:00:00.000000 UTC | 1                 | 471                | 495               | 2                   |
| 4702921684 | 2016-05-07 00:00:00.000000 UTC | 1                 | 520                | 543               | 2                   |
| 8378563200 | 2016-04-25 00:00:00.000000 UTC | 1                 | 388                | 402               | 2                   |


Verifying the duplicates in the sleepday table

```sql
SELECT *
FROM `bellabeat.sleepday`
WHERE (Id = 4388161847 AND SleepDay = "2016-05-05 00:00:00.000000 UTC")
  OR (Id = 4702921684 AND SleepDay = "2016-05-07 00:00:00.000000 UTC")
  OR (Id = 8378563200 AND SleepDay = "2016-04-25 00:00:00.000000 UTC");

```
| Id         | SleepDay                  | TotalSleepRecords | TotalMinutesAsleep | TotalTimeInBed |
|------------|---------------------------|-------------------|--------------------|----------------|
| 4388161847 | 2016-05-05 00:00:00 UTC   | 1                 | 471                | 495            |
| 4388161847 | 2016-05-05 00:00:00 UTC   | 1                 | 471                | 495            |
| 4702921684 | 2016-05-07 00:00:00 UTC   | 1                 | 520                | 543            |
| 4702921684 | 2016-05-07 00:00:00 UTC   | 1                 | 520                | 543            |
| 8378563200 | 2016-04-25 00:00:00 UTC   | 1                 | 388                | 402            |
| 8378563200 | 2016-04-25 00:00:00 UTC   | 1                 | 388                | 402            |

## Removing duplicates from the sleepday table // creating a new clean_sleepday table

```sql
CREATE OR REPLACE TABLE `bellabeat.clean_sleepday` AS
SELECT *
FROM (
  SELECT
    *,
    ROW_NUMBER() OVER (PARTITION BY Id, SleepDay ORDER BY Id) as row_num
  FROM `bellabeat.sleepday`
)
WHERE row_num = 1;
```

## Checking for Null Values

Null value check - weightlog
```sql
SELECT
  COUNT(*) AS total_rows,
  COUNTIF(WeightKg IS NULL) AS null_WeightKg,
  COUNTIF(WeightPounds IS NULL) AS null_WeightPounds,
  COUNTIF(Fat IS NULL) AS null_Fat,
  COUNTIF(BMI IS NULL) AS null_BMI,
  COUNTIF(IsManualReport IS NULL) AS null_report,
  COUNTIF(LogId IS NULL) AS null_logId,
FROM `bamboo-life-418613.bellabeat.weightlog`;
```

Results: Null values were found in the Fat column of the weightlog table.

...
Checked for null values in remaining tables:
dailyactivity, clean_sleepday, dailycalories, dailyintensities, dailysteps, hourlyintensities, hourlysteps

No null values were found in these tables.



# Dropping the weightlog Table

The weightlog table was dropped from the analysis due to the following reasons:

1. Incomplete data: Only 8 unique IDs, which may not provide a comprehensive view of user behavior and patterns.
2. Missing values: High number of null values in the Fat column, suggesting unreliable data capture or reporting.
3. Relevance: Weight data was not considered a major variable for the analysis.


## New Columns

Adding Time of Day Column:  I added a DayPeriod column to the hourlyintensities and hourlysteps tables to categorize data into "Morning" (6 AM to 11:59 AM), "Afternoon" (12 PM to 3:59 PM), and "Night" (4 PM to 5:59 AM):

```sql
SELECT *, 
   CASE 
      WHEN EXTRACT(HOUR FROM ActivityHour) BETWEEN 6 AND 11 THEN 'Morning'
      WHEN EXTRACT(HOUR FROM ActivityHour) BETWEEN 12 AND 15 THEN 'Afternoon'
      ELSE 'Night'
   END AS DayPeriod
FROM bellabeats.hourlyintensities;
```

```sql
SELECT *, 
   CASE 
      WHEN EXTRACT(HOUR FROM ActivityHour) BETWEEN 6 AND 11 THEN 'Morning'
      WHEN EXTRACT(HOUR FROM ActivityHour) BETWEEN 12 AND 15 THEN 'Afternoon'
      ELSE 'Night'
   END AS DayPeriod
FROM bellabeats.hourlysteps;
```
The resulting tables were saved as:
| hourlystepstod | hourlyintensitiestod |
|-------------------|--------------|

### Adding Day of Week Column

Added a DayOfWeek column to the dailysteps, dailyintensities, dailycalories, dailyactivity, and clean_sleepday tables using the FORMAT_DATE() function:
```sql
SELECT *, 
    FORMAT_DATE('%A', ActivityDay) AS DayOfWeek
FROM bellabeat.dailysteps;
sqlCopy codeSELECT 
    *, 
    FORMAT_DATE('%A', ActivityDay) AS DayOfWeek
FROM bellabeat.dailyintensities;
sqlCopy codeSELECT 
    *, 
    FORMAT_DATE('%A', ActivityDay) AS DayOfWeek
FROM bellabeat.dailycalories;
sqlCopy codeSELECT
   *,
   FORMAT_DATE('%A', ActivityDate) AS DayOfWeek
FROM `bellabeat.dailyactivity`;
sqlCopy codeSELECT 
    *, 
    FORMAT_DATE('%A', SleepDay) AS DayOfWeek
FROM bellabeat.clean_sleepday;
```

The resulting tables were saved as:
| dailystepsdow| dailyintensititesdow | dailycaloriesdow | TotalMinutesAsleep |
|------------|---------------------------|-------------------|--------------------|

### Renaming Columns for Consistency

Renamed the ActivityDate column to ActivityDay in the dailyactivitydow table:
```sql
ALTER TABLE bellabeat.dailyactivitydow
RENAME COLUMN ActivityDate TO ActivityDay;
```
Renaming the SleepDay column to ActivityDay in the clean_sleepdaydow table:
```sql
ALTER TABLE bellabeat.clean_sleepdaydow
RENAME COLUMN SleepDay TO ActivityDay;
```

# Data Export

| Original Dataset         | Renamed Dataset           |
|--------------------------|---------------------------|
| clean_sleepdaydow        | sleepdayclean             |
| dailyactivitydow         | dailyactivityclean        |
| dailycaloriesdow         | dailycaloriesclean        |
| dailyintensitiesdow      | dailyintensitiesclean     |
| dailystepsdow            | dailystepsclean           |
| hourlyintensitiestod     | hourlyintensitiesclean    |
| hourlystepstod           | hourlystepsclean          |
