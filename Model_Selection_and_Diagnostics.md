Model Selection and Diagnostics
================
Xiaotai Chai
2/28/2018

``` r
library(readxl)
library('dplyr')
library(lmtest)
library(ISLR)
library(MASS)
library(boot)
library(leaps)
```

Model selection, real data
==========================

Suppose a researcher is using the College data set in `ISLR` to predict the number of students enrolled in a college.

1.  The researcher is interested in comparing the model with all predictors to a smaller model predicting enrollment using Private, Accept, Top10perc, F.Undergrad, and Room.Board. Is there statistical evidence to justify keeping the full model? Justify your answer.

``` r
data('College')

mod_full <- lm(Enroll ~ Private + Accept + Top10perc + F.Undergrad + Room.Board, data = College)
full_summ <- summary(mod_full)

allpossreg <- regsubsets(Enroll ~ Private + Accept + Top10perc + F.Undergrad + Room.Board, nbest = 6, data = College)

aprout <- summary(allpossreg)

with(aprout, round(cbind(which, rsq, adjr2, cp, bic), 4))  
```

    ##   (Intercept) PrivateYes Accept Top10perc F.Undergrad Room.Board    rsq
    ## 1           1          0      0         0           1          0 0.9305
    ## 1           1          0      1         0           0          0 0.8311
    ## 1           1          1      0         0           0          0 0.3225
    ## 1           1          0      0         1           0          0 0.0329
    ## 1           1          0      0         0           0          1 0.0016
    ## 2           1          0      1         0           1          0 0.9503
    ## 2           1          0      0         1           1          0 0.9326
    ## 2           1          1      0         0           1          0 0.9316
    ## 2           1          0      0         0           1          1 0.9312
    ## 2           1          1      1         0           0          0 0.8545
    ## 2           1          0      1         0           0          1 0.8464
    ## 3           1          0      1         1           1          0 0.9510
    ## 3           1          0      1         0           1          1 0.9507
    ## 3           1          1      1         0           1          0 0.9504
    ## 3           1          1      0         1           1          0 0.9330
    ## 3           1          0      0         1           1          1 0.9327
    ## 3           1          1      0         0           1          1 0.9318
    ## 4           1          0      1         1           1          1 0.9519
    ## 4           1          1      1         1           1          0 0.9510
    ## 4           1          1      1         0           1          1 0.9510
    ## 4           1          1      0         1           1          1 0.9330
    ## 4           1          1      1         1           0          1 0.8639
    ## 5           1          1      1         1           1          1 0.9520
    ##    adjr2         cp        bic
    ## 1 0.9304   342.6149 -2058.8357
    ## 1 0.8309  1939.6410 -1368.4580
    ## 1 0.3216 10106.5721  -289.2328
    ## 1 0.0316 14758.0531   -12.6562
    ## 1 0.0003 15259.8727    12.0522
    ## 2 0.9502    26.5849 -2312.9203
    ## 2 0.9324   311.4321 -2075.6420
    ## 2 0.9314   327.2886 -2064.3423
    ## 2 0.9310   333.5196 -2059.9465
    ## 2 0.8541  1565.5310 -1477.7740
    ## 2 0.8460  1696.2709 -1435.4700
    ## 3 0.9508    18.1101 -2316.5369
    ## 3 0.9505    23.0604 -2311.6655
    ## 3 0.9502    27.0259 -2307.7851
    ## 3 0.9327   307.4872 -2073.2657
    ## 3 0.9324   311.9909 -2070.0218
    ## 3 0.9316   325.7013 -2060.2289
    ## 4 0.9516     5.7992 -2324.1386
    ## 4 0.9507    20.0357 -2309.9549
    ## 4 0.9507    20.1730 -2309.8194
    ## 4 0.9326   309.2261 -2066.7988
    ## 4 0.8632  1419.2267 -1516.1260
    ## 5 0.9517     6.0000 -2319.2942

**AWS: The adjusted R^2 for the full model is largest and BIC statistic for the full model is one of the smallest, so we should keep the full model.**

1.  Perform model selection using the method of your choice. State the equation for the model you choose, and briefly explain your criteria in choosing that model.

``` r
stepAIC(mod_full, direction = "backward")
```

    ## Start:  AIC=8272.27
    ## Enroll ~ Private + Accept + Top10perc + F.Undergrad + Room.Board
    ## 
    ##               Df Sum of Sq      RSS    AIC
    ## - Private      1     75062 32241086 8272.1
    ## <none>                     32166024 8272.3
    ## - Room.Board   1    669009 32835033 8286.3
    ## - Top10perc    1    674736 32840760 8286.4
    ## - Accept       1  12733995 44900020 8529.4
    ## - F.Undergrad  1  59043083 91209107 9080.1
    ## 
    ## Step:  AIC=8272.08
    ## Enroll ~ Accept + Top10perc + F.Undergrad + Room.Board
    ## 
    ##               Df Sum of Sq       RSS    AIC
    ## <none>                      32241086 8272.1
    ## - Room.Board   1    597050  32838136 8284.3
    ## - Top10perc    1    803575  33044661 8289.2
    ## - Accept       1  12857720  45098806 8530.9
    ## - F.Undergrad  1  68707882 100948968 9156.9

    ## 
    ## Call:
    ## lm(formula = Enroll ~ Accept + Top10perc + F.Undergrad + Room.Board, 
    ##     data = College)
    ## 
    ## Coefficients:
    ## (Intercept)       Accept    Top10perc  F.Undergrad   Room.Board  
    ##   127.66708      0.11388      1.99818      0.13301     -0.02864

The model is *E**n**r**o**l**l* = 127.66708 + 0.11388 \* *A**c**c**e**p**t* + 1.99818 \* *T**o**p*10*p**e**r**c* + 0.13301 \* *F*.*U**n**d**e**r**g**r**a**d* − 0.02864 \* *R**o**o**m*.*B**o**a**r**d*. **Criteria: Consider the predictor with the lowest AIC - If AIC is lower when the predictor has been removed than when it is in model, remove it; otherwise, keep it.**

Model diagnostics, simulated data
=================================

Given the simulated data in the attached file...

1.  Examine the data for evidence of multicollinearity, nonlinearity in the predictors, influential outliers, heteroskedacity, and nonnormal errors. If you find evidence of a problem, adjust your model accordingly. If you find influential outliers, remove them from the data set (we will pretend that any outliers are actually typos). Briefly summarize your results (1-2 sentences) and give the coefficients for your final fitted model.

Read in data
------------

``` r
mydata <- read_excel("myData.xlsx")
```

multicollinearity and nonlinearity in the predictors
----------------------------------------------------

``` r
mydataF <- mydata
names(mydataF) <- c("y", "x1", "x2", "x3")
pairs(mydataF)
```

![](Model_Selection_and_Diagnostics_files/figure-markdown_github/unnamed-chunk-5-1.png)

``` r
cor(mydataF)
```

    ##             y          x1         x2        x3
    ## y   1.0000000 -0.77656174 0.49841324 0.4795614
    ## x1 -0.7765617  1.00000000 0.03000128 0.0302473
    ## x2  0.4984132  0.03000128 1.00000000 0.9703544
    ## x3  0.4795614  0.03024730 0.97035435 1.0000000

``` r
# Check multicollinearity between x1 and x2
mulcol12 <- lm(x1 ~ x2, data=mydata)
summary(mulcol12)
```

    ## 
    ## Call:
    ## lm(formula = x1 ~ x2, data = mydata)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -10.2549  -4.1713  -0.2698   4.2248   9.0776 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) 10.54261    0.68186  15.462   <2e-16 ***
    ## x2           0.01155    0.02720   0.424    0.672    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 5.143 on 200 degrees of freedom
    ## Multiple R-squared:  0.0009001,  Adjusted R-squared:  -0.004095 
    ## F-statistic: 0.1802 on 1 and 200 DF,  p-value: 0.6717

``` r
# Check multicollinearity between x1 and x3
mulcol13 <- lm(x1 ~ x3, data=mydata)
summary(mulcol13)
```

    ## 
    ## Call:
    ## lm(formula = x1 ~ x3, data = mydata)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -10.228  -4.110  -0.317   4.221   9.093 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) 10.54953    0.66425  15.882   <2e-16 ***
    ## x3           0.01237    0.02892   0.428    0.669    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 5.143 on 200 degrees of freedom
    ## Multiple R-squared:  0.0009149,  Adjusted R-squared:  -0.004081 
    ## F-statistic: 0.1831 on 1 and 200 DF,  p-value: 0.6691

``` r
# Check multicollinearity between x2 and x3
mulcol23 <- lm(x2 ~ x3, data=mydata)
summary(mulcol23)
```

    ## 
    ## Call:
    ## lm(formula = x2 ~ x3, data = mydata)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -9.0209 -2.3504 -0.1521  1.9902  7.9805 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  1.37467    0.41733   3.294  0.00117 ** 
    ## x3           1.03150    0.01817  56.780  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 3.231 on 200 degrees of freedom
    ## Multiple R-squared:  0.9416, Adjusted R-squared:  0.9413 
    ## F-statistic:  3224 on 1 and 200 DF,  p-value: < 2.2e-16

**There's multicollinearity between x2 and x3 since the p value for the coefficient of x3 is strongly significant. There are nonlinearity between x1 and x2, x1 and x3. So I'll leave out x3 as a predictor.**

influential outliers
--------------------

``` r
fit <- lm(y ~ x1 +x2, data = mydata)

## Find Y outliers
diag_df <- data.frame(obs_id = 1:nrow(mydata), 
resid = round(resid(fit), 4), 
hatvalues = round(hatvalues(fit), 4), 
delstudent = round(rstudent(fit), 4))

n <- nrow(mydata)
alpha <- 0.1
p <- length(coef(fit))

(t.crit <- qt(1 - alpha/(2 * n), n - p - 1))
```

    ## [1] 3.542062

``` r
df <- mutate(diag_df, outlier = abs(delstudent) >= t.crit)
filter(df, outlier == TRUE)
```

    ##   obs_id     resid hatvalues delstudent outlier
    ## 1     47 -421.8619    0.0923   -15.3158    TRUE
    ## 2    186 -218.7865    0.0174    -5.5525    TRUE

``` r
## Find influential cases using Cook's Distance
infl_df <- data.frame(dffits = round(dffits(fit), 4), 
                      cooksd = round(cooks.distance(fit), 4), 
                      dfbeta0 = round(dfbetas(fit)[, 1], 4), 
                      dfbeta1 = round(dfbetas(fit)[, 2], 4), 
                      dfbeta2 = round(dfbetas(fit)[, 3], 4))

plot(fit, which = 4:5)
```

![](Model_Selection_and_Diagnostics_files/figure-markdown_github/unnamed-chunk-6-1.png)![](Model_Selection_and_Diagnostics_files/figure-markdown_github/unnamed-chunk-6-2.png) **The 47th and 186th observations are the Y outliers, but only 47th observation is outside the Cook's distance dash line, which means it is the influential case for the model. So I'll remove the 47th from the data set.**

``` r
mydatanew <- mydata[-47,]
fit2 <- lm(y ~ x1 +x2, data = mydatanew)
```

heteroskedacity
---------------

``` r
plot(fit2, which = 3)
```

![](Model_Selection_and_Diagnostics_files/figure-markdown_github/unnamed-chunk-8-1.png)

``` r
## Broeusch Pagan Test to check heteroskedacity
bptest(y ~ x1+x2, data=mydatanew)
```

    ## 
    ##  studentized Breusch-Pagan test
    ## 
    ## data:  y ~ x1 + x2
    ## BP = 3.5174, df = 2, p-value = 0.1723

**In the Scale-Location plot, the residuals looks like a random cloud of points for all fitted values along the x-axis and there are no patterns or shapes. So there's no heteroskedacity Also, the test has a p-value greater than a significance level of 0.05, therefore we can accept the null hypothesis that the variance of the residuals is constant and infer that heteroskedacity is not present. **

nonnormal errors
----------------

``` r
plot(fit2, which = 2)
```

![](Model_Selection_and_Diagnostics_files/figure-markdown_github/unnamed-chunk-9-1.png)

``` r
## Shapiro-Wilk normality test to test normality
shapiro.test(residuals(fit2))
```

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  residuals(fit2)
    ## W = 0.83044, p-value = 4.846e-14

**From the Shapiro-Wilk test, the p-value is less than 0.05, so we reject the null hypothesis that the samples come from normal distribution. Also from the QQ plot, the data is a little bit light tailed. Since the nonnormality is small and a small departure from normality may not break the estimates, we don't need to adjust the model here.**

Final fitted model and summary
------------------------------

``` r
summary(fit2)
```

    ## 
    ## Call:
    ## lm(formula = y ~ x1 + x2, data = mydatanew)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -207.241  -15.849    8.319   21.091   35.873 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) 430.3924     5.6807   75.76   <2e-16 ***
    ## x1          -19.4540     0.4018  -48.41   <2e-16 ***
    ## x2            5.3552     0.1587   33.73   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 28.91 on 198 degrees of freedom
    ## Multiple R-squared:  0.943,  Adjusted R-squared:  0.9424 
    ## F-statistic:  1638 on 2 and 198 DF,  p-value: < 2.2e-16

**fit2 is out final fitted model:** *y* = 430.3924 − 19.454 \* *x*1 + 5.3552 \* *x*2 **Summary: there's a multicollinearity between x2 and x3, so I drop x3 from the model. I deleted one influential outlier. Heteroskedacity is not present. There's slight nonnormality and adjustment for it is not necessary here.**

1.  Use your final model to predict $\\hat{Y}$ for the given x-values below. Save your predictions in a vector called `yhat`. (**Note:** if your values for `yhat` are within a certain threshhold of the true expected values of Y, you will receive extra credit. Good luck!)

``` r
x.test.mat <- matrix(c(
8.88,    7.56,  3.21,       
5.20,    8.50,  6.37,       
1.13,    8.62,  11.86,      
1.57,    22.66, 18.34,  
5.75,    3.65,  5.34,   
5.19,    35.76, 33.35,  
6.85,    25.71, 19.22,      
18.27, 43.60,   42.89,      
18.83, 39.41,   37.85,      
10.35, 31.76,   27.12   ), byrow = TRUE, ncol = 3, nrow = 10)
colnames(x.test.mat) <- c("x1", "x2", "x3")
```

``` r
x_test <- data.frame(x.test.mat[,1:2])
(yhat <- predict(fit2, x_test))
```

    ##        1        2        3        4        5        6        7        8 
    ## 298.1266 374.7512 454.5716 521.1994 338.0786 520.9297 434.8158 308.4565 
    ##        9       10 
    ## 275.1238 399.1261
