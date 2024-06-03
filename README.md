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
- During the review of the dataset discussion, I found that user paulabsmanner identified a corrupted streams value for the song "Love Grows (Where My Rosemary Goes)".
  I located and removed this corrupted record using SQL. Acknowledgment and thanks to the Kaggle community for their contributions!

## Key Findings

## Recommendations

## Tables



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
