---
layout: page
title: Anthony J. Ruiz
subtitle: Email: ruiz.anthonyj@gmail.com | location: Orlando, FL
feature image:  "https://picsum.photos/2560/600?image=957"
---

## Education

**Master of Arts** - Economics &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; August 2014 - May 2016 <br>
University of South Florida - Tampa, FL

**Bachelor of Science** - Business Economics 3.8/4.0 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; August 2014 - May 2016 <br>
University of South Florida - Tampa, FL

## Experience


## Skills

~~~
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
~~~
