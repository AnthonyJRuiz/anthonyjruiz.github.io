---
layout: post
title: Animate ggplot Visuals with gganimate
tags: [Econometrics, Data Science, Statistics, Memes]
---

Using data from the CDC

Testing Gif
-----------

    health.data <- read.csv("https://data.cdc.gov/api/views/bi63-dtpu/rows.csv?accessType=DOWNLOAD")

    health.data <- health.data %>%
      clean_names("lower_camel") %>%
      rename("detailedCause" = "x113CauseName")

Plotting leading cause of death by year
=======================================

    health.data %>%
      filter(causeName != 'All causes') %>%
      group_by(year, causeName) %>%
      summarize(totalDeaths = sum(deaths)) %>%
        ggplot() +
        geom_bar(aes(causeName, totalDeaths), stat = 'identity') +
        labs(title = "Leading cause of death in: {frame_time}", y = "Total Deaths", x = "Cause of Death") +
        scale_y_continuous(label = comma) +
        coord_flip() +
        transition_time(year)

<p style="text-align:center">
<img src="/img/gif_example.gif" alt="gif"/>
</p>
