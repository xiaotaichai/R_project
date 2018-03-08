Bootstrapping methods
================
Xiaotai Chai
2/28/2018

1.  A statistics student was interested in prices for used Mustang cars being offered for sale on an internet site. He sampled 25 cars from the website; the data are stored in the data set “MustangPrice” in the Lock5Data package.

<!-- -->

1.  Suppose we are interested in studying the correlation between Price and Miles for the cars. Describe how you might take one bootstrap sample from this data set.

``` r
#install.packages("Lock5Data")
library(Lock5Data)
attach(MustangPrice)

sample_corr <- function(data, i){
 sample = data[i,]
  correlation = cor(sample$Price,sample$Miles)
  return(correlation)
}

(result <- boot(MustangPrice, sample_corr, R=500 ))
```

    ## 
    ## ORDINARY NONPARAMETRIC BOOTSTRAP
    ## 
    ## 
    ## Call:
    ## boot(data = MustangPrice, statistic = sample_corr, R = 500)
    ## 
    ## 
    ## Bootstrap Statistics :
    ##       original      bias    std. error
    ## t1* -0.8246164 -0.01493135  0.05655057

``` r
# I might take one bootstrap sample by taking n amount of sample by replacement.
```

1.  What is the bootstrap estimate of the standard error?

``` r
(std_error <- sd(result$t))
```

    ## [1] 0.05655057

1.  Find a 95% bootstrap confidence interval for the correlation. Interpret the interval in context.

``` r
(CI_corr <- boot.ci(result))
```

    ## Warning in boot.ci(result): bootstrap variances needed for studentized
    ## intervals

    ## Warning in norm.inter(t, adj.alpha): extreme order statistics used as
    ## endpoints

    ## BOOTSTRAP CONFIDENCE INTERVAL CALCULATIONS
    ## Based on 500 bootstrap replicates
    ## 
    ## CALL : 
    ## boot.ci(boot.out = result)
    ## 
    ## Intervals : 
    ## Level      Normal              Basic         
    ## 95%   (-0.9205, -0.6988 )   (-0.9239, -0.7157 )  
    ## 
    ## Level     Percentile            BCa          
    ## 95%   (-0.9335, -0.7254 )   (-0.9088, -0.6713 )  
    ## Calculations and Intervals on Original Scale
    ## Warning : BCa Intervals used Extreme Quantiles
    ## Some BCa intervals may be unstable

``` r
## Interpretation: We are confident that 95% of the correlation would fall between -0.9 and -0.7. There's a negative correlation between price and miles. When miles go up, price goes down.
```

1.  Suppose we would like to create a 95% bootstrap confidence interval for the median price of the mustangs. Simulate 1000 bootstrap samples and graph the bootstrap distribution for the sample median price. If appropriate, construct a 95% bootstrap CI for the median price; if not appropriate, explain why.

``` r
sample_med <- function(data, i){
 sample = data[i,]
  med = median(sample$Price)
  return(med)
}

(result2 <- boot(MustangPrice, sample_med, R=1000 ))
```

    ## 
    ## ORDINARY NONPARAMETRIC BOOTSTRAP
    ## 
    ## 
    ## Call:
    ## boot(data = MustangPrice, statistic = sample_med, R = 1000)
    ## 
    ## 
    ## Bootstrap Statistics :
    ##     original  bias    std. error
    ## t1*     11.9  0.3083    2.452047

``` r
plot(result2)
```

![](Bootstrapping_methods_files/figure-markdown_github/unnamed-chunk-4-1.png)

``` r
(CI_med <- boot.ci(result2))
```

    ## Warning in boot.ci(result2): bootstrap variances needed for studentized
    ## intervals

    ## BOOTSTRAP CONFIDENCE INTERVAL CALCULATIONS
    ## Based on 1000 bootstrap replicates
    ## 
    ## CALL : 
    ## boot.ci(boot.out = result2)
    ## 
    ## Intervals : 
    ## Level      Normal              Basic         
    ## 95%   ( 6.79, 16.40 )   ( 2.80, 14.80 )  
    ## 
    ## Level     Percentile            BCa          
    ## 95%   ( 9.0, 21.0 )   ( 8.2, 16.0 )  
    ## Calculations and Intervals on Original Scale
    ## Some BCa intervals may be unstable

``` r
## It's appropriate.
```
