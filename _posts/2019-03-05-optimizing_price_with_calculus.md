I’ve seen a few people do quick examples of price optimization problems,
but one thing I havent seen done is using calculus to obtain the optimal
price for a good. In this extremely quick example im going to
demonstrate how to do this…

Package Load
------------

    library(dplyr)

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    library(ggplot2)
    library(stats) #optimization
    library(broom) #tidy model output
    library(plotly) #make interactive ggplot visuals

    ## 
    ## Attaching package: 'plotly'

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     last_plot

    ## The following object is masked from 'package:stats':
    ## 
    ##     filter

    ## The following object is masked from 'package:graphics':
    ## 
    ##     layout

    getwd()

    ## [1] "/Users/AnthonyRuiz/R Repository/anthonyjruiz.github.io/markdown_files"

Data
----

For this exercise we are using beef sales data that ranges from 1977 -
1999.

    demand.data <- read.csv("https://raw.githubusercontent.com/susanli2016/Machine-Learning-with-Python/master/beef.csv")

    head(demand.data)

    ##   Year Quarter Quantity    Price
    ## 1 1977       1  22.9976 142.1667
    ## 2 1977       2  22.6131 143.9333
    ## 3 1977       3  23.4054 146.5000
    ## 4 1977       4  22.7401 150.8000
    ## 5 1978       1  22.0441 160.0000
    ## 6 1978       2  21.7602 182.5333

Exploration
-----------

Here we are just taking a look at the relationship between price and
demand for beef - we see a pretty clear negative relationship here.

    demand.data %>%
      ggplot(aes(Quantity, Price)) +
        geom_point() +
        geom_smooth(method = "lm")

![](price_optimization_files/figure-markdown_strict/unnamed-chunk-4-1.png)

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

Taking a look at our model output and model diagnostics we get…

How to analyze the model diagnostics will be a post for another day,
let’s just assume this model is amazing.

    summary(demand.model)

    ## 
    ## Call:
    ## lm(formula = Quantity ~ Price, data = demand.data)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -1.05150 -0.44397  0.05104  0.38968  1.34430 
    ## 
    ## Coefficients:
    ##              Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) 30.051486   0.413355   72.70   <2e-16 ***
    ## Price       -0.046511   0.001633  -28.48   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.5734 on 89 degrees of freedom
    ## Multiple R-squared:  0.9011, Adjusted R-squared:    0.9 
    ## F-statistic: 811.2 on 1 and 89 DF,  p-value: < 2.2e-16

    plot(demand.model)

![](price_optimization_files/figure-markdown_strict/unnamed-chunk-6-1.png)![](price_optimization_files/figure-markdown_strict/unnamed-chunk-6-2.png)![](price_optimization_files/figure-markdown_strict/unnamed-chunk-6-3.png)![](price_optimization_files/figure-markdown_strict/unnamed-chunk-6-4.png)

Demand Equation
---------------

Directly from our model output we can find our demand equation… That is.

`Qdemand = 30.05 - 0.04 x Price`

This says, for every $1 increase in price, quantity of beef sales
decreases by .04 (units?) whatever a unit of beef is…

    demand.equation <- function(x){
      (x-80)*(30.05-.0465*x)
    }

Here comes the calculus…
------------------------

Sir Issac Newton would be proud!…

Now how I’ve seen this done by others (and by all means is correct) is
typcially they will generate a vector of prices, say $0 - $1000 and run
that equation at each price and see what the estimated revenue will be…
That definitely works, but there’s a far more efficient and
“sophisticated” way to go about this…

If you remember back to your calculus classes… It’s all about
derivatives. In this instance we can take the first derivative of our
demand function with respect to price, set that equation equal to 0 and
solve for price. Setting the function = 0 and solving will yield what
the revenue maximizing price will be.

Setting it equal to 0 means the slope of the line at the price is flat…
We’ll see an example below.

lucky for us, we dont have to handwrite our math and we have this
optimize function out of the stats package that will do this work for
us…

    profit.max <- stats::optimize(demand.equation, lower = 0, upper = 500, maximum = TRUE)
    print(profit.max$maximum)

    ## [1] 363.1183

Total Revenue Curve
-------------------

In this example we are optimizing price to maximize revenue… We can also
extend this and maximize profit, for that we will just need to include
cost in our equation…

      ggplot(data.frame(price = 0:500), aes(price)) +
        stat_function(fun = demand.equation, geom = 'line') +
        labs(title = "Total Revenue Curve", x = "Price", y = "Total Revenue")

![](price_optimization_files/figure-markdown_strict/unnamed-chunk-9-1.png)
