---
layout: page
title: About me
bigimg: /img/bookshelf.jpg
---

I am currently a data scientist working at the Walt Disney Company. My formal education is in Economics and statistics. I am an avid learner and enjoy sharing my knowledge with whoever is willing to listen. I love traveling and exploring places I've never been to.

You can find my official most recent resume [here](https://anthonyjruiz.github.io/files/Anthony J Ruiz - Resume.pdf)

or in R format below

```javascript
library(tidyverse)

aboutMe <- anthonyRuiz %>%
  mutate(
    education =
      case_when(
        year == 2013 & institution == 'University of South Florida' ~ 'B.S. Economics',
        year == 2016 & institution == 'University of South Florida' ~ 'M.S. Economics',
        TRUE ~ NULL),
    experience =
      case_when(
        year > 2018 & employer == 'The Walt Disney Company' ~ 'Data Scientist',
        year < 2018 & employer == 'Electronic Arts' ~ 'Data Analyst',
        TRUE ~ NULL) %>%
    rProgrammingFlag = if_else(!is.na(rskills),1,0)) %>%
    sqlProgrammingFlag = if_else(!is.na(sql),1,0))
      )      
```
