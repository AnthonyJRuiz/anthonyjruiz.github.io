---
layout: page
title: About me
subtitle: Why you'd want to go on a date with me
---

I am currently a data scientist currently working at the Walt Disney company. I have both an undergraduate and graduate degrees in economics from the University of South Florida. I love traveling, economics, statistics and memes...

You can find my most recent resume [here](anthonyjruiz.github.io/filesAnthonyRuizResume 20171213)

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
        TRUE ~ NULL)
      )      
```
