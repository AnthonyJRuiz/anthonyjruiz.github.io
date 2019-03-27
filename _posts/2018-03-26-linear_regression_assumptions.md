---
layout: post
title: Gauss-Markov Assumptions
tags: [Econometrics]
comments: true
# image: /img/da_real_mvp.jpg
---

Linear regression is probably one of the most used statistical techniques in all of econometrics/data science. There are a couple reasons for this; its relatively easy to interpret the model output, its not computationally expensive and...

<p style="text-align:center">
<img src="/img/da_real_mvp.jpg" alt="MVP" />
</p>


However Contrary to what many people think, there are a certain set of assumptions that must be satisfied in order to kind of believe you model. These are called the Gauss-Markov assumptions which is named after mathematicians Carl Friedrich Gauss and Andrey Markov.

  1. Linear in the parameters
  2. Random Sampling
  3. No *Perfect* collinearity
  4. Zero conditional mean - the expected value of u given x = 0
  5. Homoskedasticity



With it being one of the most used techniques, it's shocking to see how many people are misinformed on these assumptions. I wonder how many people are incorrectly using one of the most simplest techniques out there in the wild. I've read things on forums like [reddit](https://reddit.com/r/datascience) like you can only model a linear relationship (referring to the first assumption), how you cant have multicollinearity etc. I just wanted to quickly talk through the assumptions and their meanings...
