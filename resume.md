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
about.me <- anthonyRuiz %>%
  mutate(
    education =
      case_when(
        year == 2013 & institution == 'University of South Florida' ~ 'bsEconomics',
        year == 2016 & institution == 'University of South Florida' ~ 'maEconomics',
        TRUE ~ NULL),
    experience =
      case_when(
        year > 2018 & company == 'The Walt Disney Company' ~ 'dataScientist',
        year == 2018 & company == 'Electronic Arts' ~ 'dataAnalyst',
        year >= 2016 & year < 2018 & company == 'The Walt Disney Company' ~ "revenueManagement"
        TRUE ~ NULL)
        )      
~~~
