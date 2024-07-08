
##  SQL Exploration & Analysis

####  Top 5 Artists with most streams

```SQL
SELECT artistname,sum(streams) AS total_streams
FROM music.spotifydata
GROUP BY artistname
ORDER BY total_streams DESC
LIMIT 5;
```

#### Average Danceability and Energy by Key - Highest Dancibility to Lowest
```sql
SELECT key, AVG(danceability) AS avg_danceability, AVG(energy) AS avg_energy
FROM music.spotifydata
GROUP BY key
ORDER BY avg_energy DESC;
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

#### Count of charted songs by mode (Major,Minor)

```SQL
SELECT mode, COUNT(mode) AS mode_count
FROM `music.spotifydata`
GROUP BY mode;
```

#### Average BPM and Standard Deviation Rounded to Two Decimals 
```SQL
SELECT 
  ROUND(AVG(BPM),2) AS avg_bpm, 
  ROUND(stddev(bpm),2) AS standard_deviation
FROM `bamboo-life-418613.music.spotifydata`;

```
I realized that I should also have specified the key and mode together to get a more specific analysis of popularity trends.
This was done a bit late in the process. 

```SQL
ALTER TABLE your_dataset.your_table
ADD COLUMN key_mode STRING;


UPDATE `music.spotifydata`
SET key_mode = CONCAT(key, ' ', mode)
WHERE TRUE;
```
