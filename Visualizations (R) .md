## R StreamTunes Visualizations

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

