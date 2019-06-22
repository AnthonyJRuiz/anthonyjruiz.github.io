I’ve seen a few people do examples of price optimization problems,
however one thing I havent seen done is using calculus to obtain the
optimal price for the product/service. In this quick example im going to
demonstrate how to do this.

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

    summary(demand.data)

    ##       Year         Quarter         Quantity         Price      
    ##  Min.   :1977   Min.   :1.000   Min.   :15.89   Min.   :142.2  
    ##  1st Qu.:1982   1st Qu.:1.500   1st Qu.:17.04   1st Qu.:231.3  
    ##  Median :1988   Median :2.000   Median :18.17   Median :250.1  
    ##  Mean   :1988   Mean   :2.484   Mean   :18.40   Mean   :250.4  
    ##  3rd Qu.:1994   3rd Qu.:3.000   3rd Qu.:19.36   3rd Qu.:280.7  
    ##  Max.   :1999   Max.   :4.000   Max.   :23.41   Max.   :300.4

Exploration
-----------

Here we are just taking a look at the cross-sectional relationship
between price and demand for beef. We see a pretty clear negative
relationship here between price and demand.

    demand.data %>%
      ggplot(aes(Quantity, Price)) +
        geom_point() +
        geom_smooth(method = "lm")

![](price_optimization_files/figure-markdown_strict/unnamed-chunk-5-1.png)

Looking at the relationship Time series showing aggregated sales and
average price by year

    demand.data %>%
      group_by(Year) %>%
      summarise(totalQuantity = sum(Quantity),
                avgPrice = mean(Price)) %>%
      ggplot() +
        geom_line(aes(Year, totalQuantity)) +
        geom_line(aes(Year, avgPrice), color = "red", linetype = "dashed") +
        labs(title = "Beef Sales by Year", x = "Year", y = "Quantity Sold") +
        scale_y_continuous(sec.axis = dup_axis())

![](price_optimization_files/figure-markdown_strict/unnamed-chunk-6-1.png)

Estimating the demand equation
------------------------------

The model specification, that is, which variables should be
included/excluded in your model is the most **important** part of this
entire process (aside from having reliable data). The optimization on
the back end is relatively easy to do - it’s just a bit of algebra and
calculus. Since we are trying to uncover a casual relationship between
price and demand we must think carefully and critically about the other
variables that influence demand. er variables influence demand and we
must be cognizant of the [Gauss-Markov
assumptions](https://en.wikipedia.org/wiki/Gauss–Markov_theorem) which
ill cover in another post.

A determinant of price elasticity is the availability of substitue
goods. Many would say chicken is a substitue for beef, that is, as the
price of chicken decreases demand for beef will decrease (People are
going to buy more chicken since its cheaper). If we were to leave this
variable out of the model, our demand equation will be biased and any
optimization attempted to be done will be on the WRONG coeffeocoemt.

    demand.model <- lm(Quantity ~ Price, data = demand.data)

For the sake of brevity, we are going to assume that this model is
simply amazing. We would normally be checking our model diagnostics,
thinking from a theoretical standpoint if our model is correctly
specified - validating if our model is violating any of the Gauss-markov
assumptions of regression etc…

How to evaluate the model is any good or not will be a post for another
day.

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

Demand Equation
---------------

Here is a general demand function:
![](https://latex.codecogs.com/gif.latex?%5Clarge%20TR%20%3D%20PQ)

Here is our estimated demand function:

In english this says, for every $1 increase in price, on average the
quantity of beef sales will decrease by .04 (units?) whatever a unit of
beef is…

Now we are going to save that equation as a function for later use…

    demand.equation <- function(x){
      (x-80)*(30.05-.0465*x)
    }

Here comes the calculus…
------------------------

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

### Total Revenue Function

If we were going to understand what our total revenue (TR) of sales was
it would simply be the quanity of products sold (Q) multiplied by the
price of those products (P):

![](https://latex.codecogs.com/gif.latex?%5Clarge%20TR%20%3D%20PQ)

Remember, we just found out what our Q is… We can substitute our demand
equation in our total Revenue function above… we obtain

![](https://latex.codecogs.com/gif.latex?%5Clarge%20TR%20%3D%20P%2830.05%20-%20.0465P%29)


After some alebra we now have our total revenue function with our
estimated demand demand equation for beef.

![](https://latex.codecogs.com/gif.latex?%5Clarge%20TR%20%3D%2030.05%20-%20.0465P%5E2)

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

![](price_optimization_files/figure-markdown_strict/unnamed-chunk-11-1.png)
