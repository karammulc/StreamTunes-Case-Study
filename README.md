## Introduction

## StreamTunes-Case-Study
Background: A major music streaming platform, StreamTunes, aims to enhance its user experience and increase user engagement by leveraging data analysis. The company has acquired a comprehensive dataset containing information about the most famous songs of 2023 on Spotify, including track attributes, popularity, and presence on various music platforms.

### Tools Used
- Google Sheets
- BigQuery
- R Posit Cloud
- Adobe Express
- Tableau



Objectives:
- Identify key factors that contribute to a song's popularity and success across different music platforms.
- Develop strategies to optimize StreamTunes' playlist curation and song recommendations based on user preferences and trends.



## Table of Contents
- [Introduction](#introduction)
- [Documentation](#documentation)
- [Challenges](#challenges)
- [Key Findings](#key-findings)
- [Recommendations](#recommendations)
- [Tables](#tables)
- [Visualizations](#visualizations)

## Documentation
[Cleaning, Manipulation & Visualization Markdown](https://github.com/karammulc/StreamTunes-Case-Study/blob/main/Cleaning%2C%20Manipulation%20%20(Sheets%20%26%20SQL)%20.md)

## Challenges
#### Challenges for this project were relatively limited. 
- Some characters within the dataset were corrupted (� instead of letters); this was resolved with the SUBSTITUTE function within sheets.
- There was slight trouble with keeping the streams variable as INT instead of CHR upon upload to posit cloud; but this was fixed to a num conversion within sheets.
- During a review of the dataset discussion, I found that user [paulabsmanner](https://www.kaggle.com/paulabsmanner) identified a corrupted streams value for the song "Love Grows (Where My Rosemary Goes)".
  I located and removed this corrupted record using SQL.
  ###### Big thanks to the Kaggle community for their contributions!  

Update: After exploring the dashboard, I noticed that there were incorrect 'releasedate' column values for Vance Joy. To try to reduce any other errors within the spreadsheet I filtered to show songs with a 'releasedate' before 2,000 as I can easily flag. 

Immediately, I noticed that songs listed as remastered, though they included their release within the title themselves, had the original versions' release date as their value. Sigue by Ed Sheeran and J Balvin also was incorrect. There a corrupted value that had the artist name in the title; this was resolved.

At first, I tried replacing the data source with a corrected CSV, but unfortunately, Tableau Public's free version does not allow you to replace a data source easily.
Alternatively, I created a calculated field to resolve these errors. 


Incorrect values 

| Track Name | Artist Name | Release Date | Accurate Release Date | Notes |
|------------|-------------|--------------|---------------------|--------|
| Cupid Twin Ver. (FIFTY FIFTY) Spe | sped up 8282 | 1997 | N/A | Corrupted entry - track name and artist name incorrect |
| Riptide | Vance Joy | 1975 | 2013 | |
| Sigue | Ed Sheeran J Balvin | 1996 | 2022 | |
| Smells Like Teen Spirit - Remastered 2021 | Nirvana | 1991 | 2021 | Incorrectly used the original release |
| Something In The Way - Remastered 2021 | Nirvana | 1991 | 2021 | Incorrectly used the original release |
| Master of Puppets (Remastered) | Metallica | 1986 | 2022 | Incorrectly used the original release |



# Dashboard
[StreamTunes Dashboard](https://public.tableau.com/authoring/Test_17204636004920/Dashboard12/StreamTunes%20Dashboard#1)

![Dashboard](https://github.com/karammulc/StreamTunes-Case-Study/blob/main/Images/StreamTunesDashboard.png)

# Key Findings

#### Top Artists by Number of Songs Charted: 
  - Taylor Swift and The Weeknd have the highest amount of songs charted within our dataset. 

#### Distribution of Charted Songs by Release Year: 
  - There is a significant skew of charted songs towards release in recent years, suggesting while some “oldies” gain attention, the majority of content listened to was released after 2010.  

#### Distribution of Songs by Key: 
  - Songs in the key of C# are most common within the dataset followed by G, G# and F.
  
#### Distribution of Streams by Mode: 
  - There is a higher count of charting songs in major key (549 occurrences) than in minor key (403 occurrences). 

#### Average BPM & standard deviation: 
  - The average BPM for charted songs within this dataset is 122.55, a moderate to upbeat range while the standard deviation for BPM is 28.07. This shows a relatively large degree of variation between listening patterns; as to be expected.
  - The BPM distribution shows a concentration around 100-130 BPM. 



# Recommendations

#### Understanding Popularity
- Understanding the popularity of certain keys, tempos and modes can help identify which songs to spotlight. 

#### Promoting New Releases
- By promoting new releases and staying updated with emerging trends, StreamTunes can identify rising stars and guide the process of updating spotlights and playlists within the platform's applications.

#### Leveraging Top Artists
- By leveraging promotions with artists such as Taylor Swift, The Weeknd, and other top artists, StreamTunes can gain the attention of major fan bases. Suggested actions could include listening and release parties, artist relevant merch with the StreamTunes logo, and more.

#### Continued Analysis 
- Further analysis could focus on budget-conscious strategies, balancing marketing expenditures with the expected return on investment based on song popularity and anticipated costs.


## Visualizations

#### Distribution of Streams by Mode
![Distribution of Streams by Mode](https://github.com/karammulc/StreamTunes-Case-Study/blob/main/Images/Distribution%20of%20Streams%20by%20Mode.png)
 - The bars in the histogram show the frequency (count) of songs that fall within specific ranges of stream counts, separated by their mode (Major or Minor).

#### BPM Distribution
![BPM Distribution](https://github.com/karammulc/StreamTunes-Case-Study/blob/main/Images/BPM%20Distribution.png) 

#### Distribution of Songs by Key
![Distribution of Songs by Key](https://github.com/karammulc/StreamTunes-Case-Study/blob/main/Images/Distribtion%20of%20Songs%20by%20Key.png))

#### Distribution of Songs by Release Year
![Distribution of Songs by Release Year](https://github.com/karammulc/StreamTunes-Case-Study/blob/main/Images/DistributionofSongs%20byReleaseYear.png)

#### Top Artists by Number of Songs Charted
![Top Artists by Number of Songs](https://github.com/karammulc/StreamTunes-Case-Study/blob/main/Images/Top%20Artists%20by%20Number%20of%20Songs.png))

## Tables

#### Top 5 Artists with Highest Stream Count

| Artist Name | Stream Count | 
|---------|----------------------|
| The Weeknd   | 14185552870      | 
| Taylor Swift | 14053658300      |
| Ed Sheeran   | 13908947204  | 
| Harry Styles | 11608645649     | 
| Bad Bunny    | 9997799607   | 

#### Average Danceability and Energy by Key

| Key     | Average Danceability | Average Energy           |
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

####  Songs With Highest Stream Count

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



#### Overall Key Count

| key | key_count |
|-----|-----------|
| C#  | 120       |
| G   | 96        |
| G#  | 91        |
| F   | 89        |
| D   | 81        |
| B   | 81        |
| A   | 74        |
| F#  | 73        |
| E   | 62        |
| A#  | 57        |


#### Count of charted songs by mode (major/minor)
| mode  | mode_count |
|-------|------------|
| Minor | 403        |
| Major | 549        |

