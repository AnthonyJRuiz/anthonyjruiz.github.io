---
layout: post
title: Price Optimization using Econometrics and Calculus
tags: [Econometrics, Data Science, Statistics, Memes]
---

I’ve seen a few people do quick examples of price optimization problems,
but one thing I havent seen done is using calculus to obtain the optimal
price for a good. In this extremely quick example im going to
demonstrate how to do this…

Package Load
------------

    library(dplyr)
    library(ggplot2)
    library(stats) #optimization
    library(broom) #tidy model output
    library(plotly) #make interactive ggplot visuals

Data
----

For this exercise we are using beef sales data by quarter that ranges
from 1977 - 1999. We can take a quick look at what that looks like to
get familiar with our dataset.

    head(demand.data)

    ##   Year Quarter Quantity    Price
    ## 1 1977       1  22.9976 142.1667
    ## 2 1977       2  22.6131 143.9333
    ## 3 1977       3  23.4054 146.5000
    ## 4 1977       4  22.7401 150.8000
    ## 5 1978       1  22.0441 160.0000
    ## 6 1978       2  21.7602 182.5333



![](/Users/AnthonyRuiz/R Repository/anthonyjruiz.github.io/img/blog_images/price_optimization_files/table.html)


<p style="text-align:center">
<img src="/img/blog_images/price_optimization_files/table.html" alt="table1"/>
</p>


Exploration
-----------

Here we are just taking a look at the relationship between price and
demand for beef - we see a pretty clear negative relationship here.

    demand.data %>%
      ggplot(aes(Quantity, Price)) +
        geom_point() +
        geom_smooth(method = "lm")



<p style="text-align:center">
<img src="/img/blog_images/price_optimization_files/unnamed-chunk-4-1.png" alt="plot1"/>
</p>


Estimating the demand equation
------------------------------

This is something that’s extremely important to note… We are dealing
with an extremely simplistic dataset. The negative relationship between
price and demand may not always be as clear as we saw in the above plot.
In a real application I can guarantee you it wont be this easy to
uncover the relationship between price and demand. This is why correctly
specifying our model is of EXTREME importantce. The math to determine
the optimal price from our estimated model is super easy compared to
this. There’s going to be many other variables that influence demand
such as seasonality, price of substitute goods etc which impact the
demand for beef. If we control for the correct varaibes we should always
uncover a negative relationship.

    demand.model <- lm(Quantity ~ Price, data = demand.data)

For the sake of brevity, we are going to assume that this model is
simply amazing. We would normally be checking our model diagnostics,
thinking from a theory standpoint if we have the correct variables in
our model violating any of the Gauss-markov assumptions of regression
etc…

How to evaluate the model is any good or not will be a post for another
day.

<div align = "center">
<table style="text-align:center"><caption><strong>Beef Demand Model</strong></caption>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="1" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td>Quantity</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Price</td><td>-0.043<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.003)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">Year</td><td>-0.024</td></tr>
<tr><td style="text-align:left"></td><td>(0.018)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">Constant</td><td>76.283<sup>**</sup></td></tr>
<tr><td style="text-align:left"></td><td>(34.620)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>91</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.903</td></tr>
<tr><td style="text-align:left">Adjusted R<sup>2</sup></td><td>0.901</td></tr>
<tr><td style="text-align:left">Residual Std. Error</td><td>0.571 (df = 88)</td></tr>
<tr><td style="text-align:left">F Statistic</td><td>410.083<sup>***</sup> (df = 2; 88)</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td style="text-align:right"><sup> *</sup>p<0.1; <sup>**</sup>p<0.05; <sup>*** </sup>p<0.01</td></tr>
</table>
</div>


<div align="center">
  <div id="htmlwidget-eb05fc5c52eb1301d9f6" style="width:100%;height:400px;" class="plotly html-widget"></div>
  <script type="application/json" data-for="htmlwidget-eb05fc5c52eb1301d9f6">{"x":{"data":[{"x":[22.9976,22.6131,23.4054,22.7401,22.0441,21.7602,21.6064,21.8814,20.5086,19.0408,19.1457,19.3989,18.947,18.898,19.2127,19.4988,19.3429,18.9816,19.6459,19.2868,18.8193,18.7968,19.9882,19.4242,19.2252,19.3361,20.4952,19.5995,19.3191,19.3744,19.9738,19.7597,19.0586,20.0757,20.8175,19.2488,19.0212,20.1607,20.7108,18.9416,18.2746,18.5266,19.175,17.8807,18.1678,18.4639,18.7622,17.2552,16.9765,17.5192,17.5975,17.2416,16.5516,17.3763,17.4207,16.4315,15.9842,17.0449,17.5591,16.2093,16.3952,16.9538,17.227,15.9093,15.8915,16.2058,17.043,15.9513,16.2494,16.8868,17.3775,16.4723,16.3097,17.0895,17.647,16.3476,17.0783,17.6763,17.1606,16.2764,16.2921,17.2994,17.0513,16.2354,16.6884,17.1985,17.5085,16.6475,16.6785,17.7635,17.6689],"y":[142.1667,143.9333,146.5,150.8,160,182.5333,186.2,186.4333,211.7,231.5,222.7,223.8333,231.1667,227.5,237.5333,238.1667,233.5,230.7,239.0333,235.4333,233.3,242.9667,244.0333,233.1333,233.8667,240.9333,234.3667,227.1667,238.4667,238,232.2,233.2333,234.9,230.4333,222.7333,226.4333,229.2667,222.9,225.6333,229.3,230.6,239.1,242.1667,241.6667,241.7333,250.1,254.5333,255,260.7,266.9667,268.0333,266.9333,272.6333,281.2,280.3667,289.8667,294.2667,295.2,284.6333,279.2,282.2667,286.8333,282.6667,286.6667,292.1333,300.4,292,289.2333,286.6667,286.1667,279.5,279.1667,283.8667,283.1,285.1,285.2667,278.7,277.4,279.8,285.0333,278.8,278.9667,281.0667,279.3,273.4667,278.1,277.3667,279.5333,278,284.7667,289.2333],"text":["Quantity: 22.9976<br />Price: 142.1667","Quantity: 22.6131<br />Price: 143.9333","Quantity: 23.4054<br />Price: 146.5000","Quantity: 22.7401<br />Price: 150.8000","Quantity: 22.0441<br />Price: 160.0000","Quantity: 21.7602<br />Price: 182.5333","Quantity: 21.6064<br />Price: 186.2000","Quantity: 21.8814<br />Price: 186.4333","Quantity: 20.5086<br />Price: 211.7000","Quantity: 19.0408<br />Price: 231.5000","Quantity: 19.1457<br />Price: 222.7000","Quantity: 19.3989<br />Price: 223.8333","Quantity: 18.9470<br />Price: 231.1667","Quantity: 18.8980<br />Price: 227.5000","Quantity: 19.2127<br />Price: 237.5333","Quantity: 19.4988<br />Price: 238.1667","Quantity: 19.3429<br />Price: 233.5000","Quantity: 18.9816<br />Price: 230.7000","Quantity: 19.6459<br />Price: 239.0333","Quantity: 19.2868<br />Price: 235.4333","Quantity: 18.8193<br />Price: 233.3000","Quantity: 18.7968<br />Price: 242.9667","Quantity: 19.9882<br />Price: 244.0333","Quantity: 19.4242<br />Price: 233.1333","Quantity: 19.2252<br />Price: 233.8667","Quantity: 19.3361<br />Price: 240.9333","Quantity: 20.4952<br />Price: 234.3667","Quantity: 19.5995<br />Price: 227.1667","Quantity: 19.3191<br />Price: 238.4667","Quantity: 19.3744<br />Price: 238.0000","Quantity: 19.9738<br />Price: 232.2000","Quantity: 19.7597<br />Price: 233.2333","Quantity: 19.0586<br />Price: 234.9000","Quantity: 20.0757<br />Price: 230.4333","Quantity: 20.8175<br />Price: 222.7333","Quantity: 19.2488<br />Price: 226.4333","Quantity: 19.0212<br />Price: 229.2667","Quantity: 20.1607<br />Price: 222.9000","Quantity: 20.7108<br />Price: 225.6333","Quantity: 18.9416<br />Price: 229.3000","Quantity: 18.2746<br />Price: 230.6000","Quantity: 18.5266<br />Price: 239.1000","Quantity: 19.1750<br />Price: 242.1667","Quantity: 17.8807<br />Price: 241.6667","Quantity: 18.1678<br />Price: 241.7333","Quantity: 18.4639<br />Price: 250.1000","Quantity: 18.7622<br />Price: 254.5333","Quantity: 17.2552<br />Price: 255.0000","Quantity: 16.9765<br />Price: 260.7000","Quantity: 17.5192<br />Price: 266.9667","Quantity: 17.5975<br />Price: 268.0333","Quantity: 17.2416<br />Price: 266.9333","Quantity: 16.5516<br />Price: 272.6333","Quantity: 17.3763<br />Price: 281.2000","Quantity: 17.4207<br />Price: 280.3667","Quantity: 16.4315<br />Price: 289.8667","Quantity: 15.9842<br />Price: 294.2667","Quantity: 17.0449<br />Price: 295.2000","Quantity: 17.5591<br />Price: 284.6333","Quantity: 16.2093<br />Price: 279.2000","Quantity: 16.3952<br />Price: 282.2667","Quantity: 16.9538<br />Price: 286.8333","Quantity: 17.2270<br />Price: 282.6667","Quantity: 15.9093<br />Price: 286.6667","Quantity: 15.8915<br />Price: 292.1333","Quantity: 16.2058<br />Price: 300.4000","Quantity: 17.0430<br />Price: 292.0000","Quantity: 15.9513<br />Price: 289.2333","Quantity: 16.2494<br />Price: 286.6667","Quantity: 16.8868<br />Price: 286.1667","Quantity: 17.3775<br />Price: 279.5000","Quantity: 16.4723<br />Price: 279.1667","Quantity: 16.3097<br />Price: 283.8667","Quantity: 17.0895<br />Price: 283.1000","Quantity: 17.6470<br />Price: 285.1000","Quantity: 16.3476<br />Price: 285.2667","Quantity: 17.0783<br />Price: 278.7000","Quantity: 17.6763<br />Price: 277.4000","Quantity: 17.1606<br />Price: 279.8000","Quantity: 16.2764<br />Price: 285.0333","Quantity: 16.2921<br />Price: 278.8000","Quantity: 17.2994<br />Price: 278.9667","Quantity: 17.0513<br />Price: 281.0667","Quantity: 16.2354<br />Price: 279.3000","Quantity: 16.6884<br />Price: 273.4667","Quantity: 17.1985<br />Price: 278.1000","Quantity: 17.5085<br />Price: 277.3667","Quantity: 16.6475<br />Price: 279.5333","Quantity: 16.6785<br />Price: 278.0000","Quantity: 17.7635<br />Price: 284.7667","Quantity: 17.6689<br />Price: 289.2333"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(0,0,0,1)","opacity":1,"size":5.66929133858268,"symbol":"circle","line":{"width":1.88976377952756,"color":"rgba(0,0,0,1)"}},"hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":23.2896645005136,"r":7.30593607305936,"b":37.244002400057,"l":48.9497716894977},"plot_bgcolor":"rgba(235,235,235,1)","paper_bgcolor":"rgba(255,255,255,1)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[15.515805,23.781095],"tickmode":"array","ticktext":["16","18","20","22"],"tickvals":[16,18,20,22],"categoryorder":"array","categoryarray":["16","18","20","22"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"Quantity","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[134.255035,308.311665],"tickmode":"array","ticktext":["$150","$200","$250","$300"],"tickvals":[150,200,250,300],"categoryorder":"array","categoryarray":["$150","$200","$250","$300"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"Price","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":"rgba(255,255,255,1)","bordercolor":"transparent","borderwidth":1.88976377952756,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"15822cc27508":{"x":{},"y":{},"type":"scatter"}},"cur_data":"15822cc27508","visdat":{"15822cc27508":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
</div>






Demand Equation
---------------

As we see from our model output above, now we have a demand equation.

![](https://latex.codecogs.com/gif.latex?%5Cdpi%7B100%7D%20%5Clarge%20%24%24TR%20%3D%20%5Cbeta_0P%20&plus;%20%5Cbeta_1P%5E2%20%24%24)


`Qdemand = 30.05 - 0.04 x Price`

In english this says, for every $1 increase in price, on average the
quantity of beef sales will decrease by .04 (units?) whatever a unit of
beef is…

Now we are going to save that equation as a function for later use

    demand.equation <- function(x){
      (x-80)*(30.05-.0465*x)
    }

Here comes the calculus…
------------------------

Sir Issac Newton would be proud!…

Now this is where the example is going to diverge from what I’ve seen
others do… And by all means the way others have done it is completely
correct - there’s 1,000 ways to skin a sheep. What I’ve seen typically
done is people will generate a vector of arbitrary prices, say $0 -
$1,000 and evaluate their demand equation at each price and see what the
estimated revenue will be. This definitely works, but in my opinion
there’s a far more efficient and sophisticated way to solve this
problem. After all, I dont want to let all those hours studying math go
to waste.

If you remember back to your calculus classes - one of the main focuses
is derivatives. Now, here’s a real life, useful application of all that
hard work.

In this instance we can take the first derivative of our demand function
with respect to price, set that equation equal to 0 and solve for price.
Setting the function = 0 and solving will yield what the revenue
maximizing price will be.

Setting it equal to 0 means the slope of the line at the price is flat…
We’ll see an example below. Lucky for us, we dont have to handwrite our
math and we have this optimize function out of the stats package that
will do all the heavy lifting for us.

    profit.max <- stats::optimize(demand.equation, lower = 0, upper = 500, maximum = TRUE)
    print(profit.max$maximum)

    ## [1] 363.1183

Using the above function, we see that our revenue maximizing price is
$363.

Total Revenue Curve
-------------------

In this particular example we are optimizing price to maximize revenue.
I understand that’s not always the metric that we are trying to
maximize. We can also extend this and maximize profit, for that we will
just need to include cost in our equation and obtain a profit function
rather than just a total revenue function. (just a bit more algebra)

      ggplot(data.frame(price = 0:500), aes(price)) +
        stat_function(fun = demand.equation, geom = 'line') +
        labs(title = "Total Revenue Curve", x = "Price", y = "Total Revenue")

<p style="text-align:center">
<img src="/img/blog_images/price_optimization_files/unnamed-chunk-9-1.png" alt="plot2"/>
</p>
