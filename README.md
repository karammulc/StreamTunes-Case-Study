## Introduction

# StreamTunes-Case-Study
Background: A major music streaming platform, StreamTunes, aims to enhance its user experience and increase user engagement by leveraging data-driven insights. The company has acquired a comprehensive dataset containing information about the most famous songs of 2023 on Spotify,
including track attributes, popularity, and presence on various music platforms.

#### Tools Used: Google Sheets, BigQuery, R Posit Cloud

Objectives:
- Identify key factors that contribute to a song's popularity and success across different music platforms.
- Develop strategies to optimize StreamTunes' playlist curation and song recommendations based on user preferences and trends.
- Explore opportunities for cross-platform partnerships and promotions to expand StreamTunes' user base and market share.

## Table of Contents
- [Introduction](#introduction)
- [Documentation](#documentation)
- [Challenges](#challenges)
- [Key Findings](#key-findings)
- [Recommendations](#recommendations)
- [Tables](#tables)
- [Visualizations](#visualizations)

## Documentation
[Cleaning, Manipulation & Visualization Markdown](https://github.com/karammulc/StreamTunes-Case-Study/blob/main/Cleaning%20%26%20Viz.md)

## Challenges
#### Challenges for this project were relatively limited. 
- Some characters within the dataset were corrupted (� instead of letters); this was resolved with the SUBSTITUTE function within sheets.
- There was slight trouble with keeping the streams variable as INT instead of CHR upon upload to posit cloud; but this was fixed to a num conversion within sheets.
- During a review of the dataset discussion, I found that user [paulabsmanner](https://www.kaggle.com/paulabsmanner) identified a corrupted streams value for the song "Love Grows (Where My Rosemary Goes)".
  I located and removed this corrupted record using SQL.
  ###### Big thanks to the Kaggle community for their contributions!  

## Key Findings

## Recommendations


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

#### Most Common Key in Top 100 songs
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


## Visualizations

## Distribution of Streams by Mode
![Distribution of Streams by Mode](https://github.com/karammulc/StreamTunes-Case-Study/blob/main/Images/Distribution%20of%20Streams%20by%20Mode.png)

## BPM Distribution
![BPM Distribution](https://github.com/karammulc/StreamTunes-Case-Study/blob/main/Images/BPM%20Distribution.png) 

## Distribution of Songs by Key
![Distribution of Songs by Key](https://github.com/karammulc/StreamTunes-Case-Study/blob/main/Images/Distribution%20of%20Songs%20by%20Key.png)

## Distribution of Songs by Release Year
![Distribution of Songs by Release Year](https://github.com/karammulc/StreamTunes-Case-Study/blob/main/Images/Distribution%20of%20Songs%20by%20Release%20Year.png))

## Top Artists by Number of Songs
![Top Artists by Number of Songs](https://github.com/karammulc/StreamTunes-Case-Study/blob/main/Images/Top%20Artists%20by%20Number%20of%20Songs.png)

