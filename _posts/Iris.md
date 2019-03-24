Unsupervised Learning
================
Anthony Ruiz
June 1, 2018

-   [Background](#background)
-   [Libraries used](#libraries-used)
-   [Data](#data)
-   [Optimal Cluster Size Determination](#optimal-cluster-size-determination)
-   [Modeling](#modeling)

Background
----------

Libraries used
--------------

``` r
library(tidyverse) #manipulate data
library(janitor) #clean data/variable names
library(gridExtra) #arrange multiple plots
library(kableExtra)
```

Data
----

For this exercize we'll be using the iris dataset that ships wit R

``` r
data("iris")
```

In this step we are just cleaning up the variable names with the janitor package. I also create another dataset in which I have removed the species variable. Since this is an unsupervised classification problem im going through the exercize as if we didnt know the species of each flower.

``` r
iris <- clean_names(iris)

training <- iris %>%
     select(sepal_length,sepal_width,petal_length,petal_width)
```

In the visualizations below I see that petal length Vs. petal width does a good job of naturally seperating each flower into groups. I will use these as the variables to cluster on.

``` r
g1 <- ggplot(data = training,aes (sepal_length,sepal_width))+
          geom_point()

g2 <- ggplot(data = training,aes (petal_length,petal_width))+
          geom_point()

g3 <- ggplot(data = training,aes (sepal_length,petal_width))+
          geom_point()

g4 <- ggplot(data = training,aes (petal_length,sepal_width))+
          geom_point()

grid.arrange(g1,g2,g3,g4,nrow = 2)
```

<img src="Iris_files/figure-markdown_github-ascii_identifiers/visualizations-1.png" style="display: block; margin: auto;" />

Optimal Cluster Size Determination
----------------------------------

In this step we are looping through several cluster sizes to see what the optimal amount is. In looking at the scree plot we want to use the clusters size which falls at the "elbow." That is, the marginal WSS error isn't significantly decreasing with the addition of 1 more group. Thus, will use set our k-parameter at 3 for the segmentation. That is, we will have 3 groups.

``` r
kmean_cluster <- data.frame(1)

for(i in 1:15){
     kmean_cluster[i] <-  
          kmeans(training, centers = i, nstart = 20)$tot.withinss
}

kmean_cluster <- kmean_cluster %>%
                 gather(1,"wss")

ggplot(data = kmean_cluster) +
     geom_line(mapping = aes(y = wss, x = 1:15)) +
     labs(y = "Within Sum of Squares", 
          x = "Cluster Size", 
          title = "Scree Plot to Determine Optimal Cluster Size") +
     scale_x_continuous(breaks = c(1,3,5,7,9,11,13,15))
```

<img src="Iris_files/figure-markdown_github-ascii_identifiers/optimal cluster size-1.png" style="display: block; margin: auto;" />

Modeling
--------

Actual classification algorithm

``` r
set.seed(23)
irisclusters <-kmeans(training[,3:4], centers = 3, nstart = 20)
```

### Visualizing the Segments

``` r
prediction <- data.frame(as.factor(irisclusters$cluster))
colnames(prediction) <- "assignedCluster" 

training <- bind_cols(training,prediction)
```

``` r
ggplot(data = training, mapping = aes(petal_length,petal_width, color = assignedCluster))+
    geom_point() +
    labs(title = "Assigned Clusters", y = "petalWidth",x = "petalLength") +
    theme_minimal()
```

<img src="Iris_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-3-1.png" style="display: block; margin: auto;" />

### Confusion Matrix

Now since k-means clustering is an unsupervised classification algorithm in the real world we wouldnt neccessarily know how well it performed. Think of guest segmentation exercizes etc. We would group like guest on similar behaviors but there wouldnt be a right answer per se. However, here we do have the correct species for each flower so I'd like to take a look and see how the algorithm performed.

To do that we can construct a confusion matrix. In lookin at that we can see that all setoas were classified correctly. However our k-means algoritthm incorreccly classified 2 versicolor species and 4 virginica species. We have an overall accuracy of 96%.

``` r
table(training$assignedCluster,iris$species) %>%
  kable("html") %>%
  kable_styling(bootstrap_options = c("striped", "hover"), full_width = F)
```

<table class="table table-striped table-hover" style="width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:right;">
setosa
</th>
<th style="text-align:right;">
versicolor
</th>
<th style="text-align:right;">
virginica
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
50
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
48
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
46
</td>
</tr>
</tbody>
</table>
``` r
original <- data.frame(iris$species) 

training <- training %>%
     bind_cols(original) %>%
     mutate(assignedCluster1 = case_when(
       assignedCluster == 1 ~ "setosa",
       assignedCluster == 2 ~ "versicolor",
       assignedCluster == 3 ~ "virginica")) %>%
     mutate(correctClassification = case_when(
          assignedCluster1 == iris.species ~ "correct",
          assignedCluster1 != iris.species ~ "incorrect"))
```

### Visualization of Accuracy

Here we can visually inspect how the k-means algorithm classified each observation. It looks like it had a tough time classifying fringe observations, for the most part it did a very good job overall.

``` r
ggplot(data = training, mapping = aes(petal_length,petal_width, color = correctClassification))+
  geom_point()+
  labs(title = "Classification Accuracy",
       y = "petalWidth",
       x = "petalLength")
```

<img src="Iris_files/figure-markdown_github-ascii_identifiers/visualization of assigned clusters-1.png" style="display: block; margin: auto;" />
