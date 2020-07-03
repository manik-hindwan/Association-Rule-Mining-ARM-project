Market Basket analysis using ECAT algorithm
================

ECLAT algorithm
---------------

We will be implementing ECLAT algorithm(which uses D.F.S to find the frequent itemsets i.e it is a vertical apriori implementation which is computationally less expensive) using R because of lack of any stable library for doing the same in Python.

Loading the dataset and the packages
------------------------------------

``` r
library(arules)
```

    ## Warning: package 'arules' was built under R version 3.5.3

    ## Loading required package: Matrix

    ## 
    ## Attaching package: 'arules'

    ## The following objects are masked from 'package:base':
    ## 
    ##     abbreviate, write

``` r
data("Groceries")
inspect(head(Groceries))
```

    ##     items                     
    ## [1] {citrus fruit,            
    ##      semi-finished bread,     
    ##      margarine,               
    ##      ready soups}             
    ## [2] {tropical fruit,          
    ##      yogurt,                  
    ##      coffee}                  
    ## [3] {whole milk}              
    ## [4] {pip fruit,               
    ##      yogurt,                  
    ##      cream cheese ,           
    ##      meat spreads}            
    ## [5] {other vegetables,        
    ##      whole milk,              
    ##      condensed milk,          
    ##      long life bakery product}
    ## [6] {whole milk,              
    ##      butter,                  
    ##      yogurt,                  
    ##      rice,                    
    ##      abrasive cleaner}

``` r
size(head(Groceries))
```

    ## [1] 4 3 1 4 4 5

Creating frequent item sets using ECLAT algorithm
-------------------------------------------------

``` r
frequent_items <- eclat(data = Groceries, parameter = list(supp = 0.004, minlen = 2))
```

    ## Eclat
    ## 
    ## parameter specification:
    ##  tidLists support minlen maxlen            target   ext
    ##     FALSE   0.004      2     10 frequent itemsets FALSE
    ## 
    ## algorithmic control:
    ##  sparse sort verbose
    ##       7   -2    TRUE
    ## 
    ## Absolute minimum support count: 39 
    ## 
    ## create itemset ... 
    ## set transactions ...[169 item(s), 9835 transaction(s)] done [0.00s].
    ## sorting and recoding items ... [126 item(s)] done [0.00s].
    ## creating sparse bit matrix ... [126 row(s), 9835 column(s)] done [0.02s].
    ## writing  ... [1272 set(s)] done [0.02s].
    ## Creating S4 object  ... done [0.00s].

``` r
inspect(head(frequent_items))
```

    ##     items                                support     count
    ## [1] {bottled beer,liquor}                0.004677173 46   
    ## [2] {other vegetables,specialty cheese}  0.004270463 42   
    ## [3] {whole milk,rice}                    0.004677173 46   
    ## [4] {spread cheese,rolls/buns}           0.004168785 41   
    ## [5] {other vegetables,canned vegetables} 0.004677173 46   
    ## [6] {whole milk,roll products }          0.004677173 46

Visualizing the frequency of the items using Frequency plot
-----------------------------------------------------------

``` r
itemFrequencyPlot(Groceries, topN = 10, type = "absolute", main = "Item Frequency")
```

![](Market-Basket-Analysis-using-Eclat_files/figure-markdown_github/vis-1.png)

Creating association rules
--------------------------

``` r
assoc_rules <- sort(frequent_items, by="support")
inspect(head(assoc_rules))
```

    ##     items                              support    count
    ## [1] {other vegetables,whole milk}      0.07483477 736  
    ## [2] {whole milk,rolls/buns}            0.05663447 557  
    ## [3] {whole milk,yogurt}                0.05602440 551  
    ## [4] {root vegetables,whole milk}       0.04890696 481  
    ## [5] {root vegetables,other vegetables} 0.04738180 466  
    ## [6] {other vegetables,yogurt}          0.04341637 427
