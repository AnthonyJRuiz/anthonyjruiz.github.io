---
layout: page
title: Anthony J. Ruiz
subtitle: Email: ruiz.anthonyj@gmail.com | location: Orlando, FL
---

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
