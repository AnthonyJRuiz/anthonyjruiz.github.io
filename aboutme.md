---
layout: page
title: About me
bigimg: /img/bookshelf.jpg
---

I am currently a data scientist working at the Walt Disney Company. I have both my undergrad and graduate degrees from the University of South Florida in Economics. I am an avid learner and enjoy sharing my knowledge with whoever is willing to listen. I love to travel and explore places I've never been. There are a couple of reasons for the motivation behind creating this page. I wanted somewhere where I could document tips/tricks and other things I've learned throughout my career and be able to refer back to them at any point. If it assists anybody with things they are currently working on, even better. Another reason is I feel by teaching you obtain a deeper level of understanding on the topic and start to think about things a bit differently.

Hope you enjoy...

You can find my official most recent resume [here](https://anthonyjruiz.github.io/files/Anthony J Ruiz - Resume.pdf)

or in dplyr format below :)

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
