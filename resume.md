---
layout: page
title: Anthony J. Ruiz
subtitle: Email: ruiz.anthonyj@gmail.com | location: Orlando, FL
---

## Education

**Master of Arts** - Economics &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; August 2014 - May 2016 <br>
University of South Florida - Tampa, FL

**Bachelor of Science** - Business Economics 3.8/4.0 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; August 2014 - May 2016 <br>
University of South Florida - Tampa, FL

## Experience


## Skills

~~~
aboutme <- anthonyRuiz %>%
  mutate(education =
    case_when(
      year == 2013 ~ 'bachelorsDegree',
      year == 2016 ~ 'mastersDegree',
      TRUE ~ NULL)
      )
~~~
