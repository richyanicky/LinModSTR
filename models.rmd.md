SNP & STR model analysis
========================================================
---
title: "Landscape PDF from rmarkdown"
author: "Richard Yanicky"
date: "09/06/2019"
output: pdf_document
fontsize: 18
---

==========================================================
The dataset is made under the assumtion all we know is that a SNP & STR are very correlated. I then made up data that I knew would support a robust linear model.

Given different responses (Phenotypes) different models will be used to fit the data. The model structures will generate usefull information. 

Care whould be taken to make sure in complex models that proper assumptions be made.

snp: 0 not present, 1 present

str: 0 not present, 1 present with motif length of 1
                    
2 present with motif length of 2
                    
3 present with motif length of 3
                    
4 present with motif length of 4
                    
pheno: some made up linear response looking data to establish a desired model.

Read data and build models
========================================================


```r
# Pull data from clipbaord or read from drive.
data1 <- read.csv(file="C:\\Users\\default.DESKTOP-KDEUCED\\Documents\\snpstr.csv",header=TRUE)
# build lm model of categorical data
modelFull <- lm(pheno~as.factor(str)+as.factor(snp)+as.factor(str)*as.factor(snp),data=data1)
modelMainEff <- lm(pheno~as.factor(str)+as.factor(snp),data=data1)
```

Model Diagnostics plots
========================================================

![plot of chunk unnamed-chunk-2](strsnpexample.rmd-figure/unnamed-chunk-2-1.png)![plot of chunk unnamed-chunk-2](strsnpexample.rmd-figure/unnamed-chunk-2-2.png)![plot of chunk unnamed-chunk-2](strsnpexample.rmd-figure/unnamed-chunk-2-3.png)![plot of chunk unnamed-chunk-2](strsnpexample.rmd-figure/unnamed-chunk-2-4.png)![plot of chunk unnamed-chunk-2](strsnpexample.rmd-figure/unnamed-chunk-2-5.png)![plot of chunk unnamed-chunk-2](strsnpexample.rmd-figure/unnamed-chunk-2-6.png)![plot of chunk unnamed-chunk-2](strsnpexample.rmd-figure/unnamed-chunk-2-7.png)![plot of chunk unnamed-chunk-2](strsnpexample.rmd-figure/unnamed-chunk-2-8.png)
Model summaries
=========================================================

```r
anova(modelFull)
```

```
Analysis of Variance Table

Response: pheno
                              Df  Sum Sq Mean Sq  F value    Pr(>F)    
as.factor(str)                 4 2296.03  574.01 229.7283 < 2.2e-16 ***
as.factor(snp)                 1   86.19   86.19  34.4929 2.576e-06 ***
as.factor(str):as.factor(snp)  4    2.66    0.67   0.2664    0.8971    
Residuals                     28   69.96    2.50                       
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

```r
anova(modelMainEff)
```

```
Analysis of Variance Table

Response: pheno
               Df  Sum Sq Mean Sq F value    Pr(>F)    
as.factor(str)  4 2296.03  574.01 252.922 < 2.2e-16 ***
as.factor(snp)  1   86.19   86.19  37.975 6.788e-07 ***
Residuals      32   72.62    2.27                      
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

We can drop the Full model and stay with the main effects model
===========================================================

```r
summary(modelMainEff)
```

```

Call:
lm(formula = pheno ~ as.factor(str) + as.factor(snp), data = data1)

Residuals:
     Min       1Q   Median       3Q      Max 
-2.87925 -0.84636  0.04565  0.64286  2.83899 

Coefficients:
                Estimate Std. Error t value Pr(>|t|)    
(Intercept)      10.4659     0.4882  21.437  < 2e-16 ***
as.factor(str)1   2.4133     0.7165   3.368  0.00198 ** 
as.factor(str)2   6.5783     0.7539   8.726 5.70e-10 ***
as.factor(str)3  11.9087     0.8275  14.392 1.58e-15 ***
as.factor(str)4  20.9847     0.7165  29.288  < 2e-16 ***
as.factor(snp)1   3.2818     0.5325   6.162 6.79e-07 ***
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 1.506 on 32 degrees of freedom
Multiple R-squared:  0.9704,	Adjusted R-squared:  0.9658 
F-statistic: 209.9 on 5 and 32 DF,  p-value: < 2.2e-16
```

Looking at the estimates, they can be evaluted as follows. These are the statements I wanted to make there can be more in these results.
=========================================================

Intercept: 10.5 This is the effect of the entire genome , "I need to improve this statement"" . I would like to say: This is the effect of the LD block with both the snp & str at 0?

as.factor(str)1 : 2.4 This is the effect of observation of the str with length 1 with respect to the str not present and keeping all other levels the same.

as.factor(str)2 : 6.5 This is the effect of observation of the str with length 2 with respect to the str not present and keeping all other levels the same.

as.factor(str)3 : 11.9 This is the effect of observation of the str with length 3 with respect to the str not present and keeping all other levels the same.

as.factor(str)4 : 21 This is the effect of observation of the str with length 4 with respect to the str not present and keeping all other levels the same.

as.factor(snp)1 : 3.3 This is the effect of observation of the snp with respect to the snp not present and keeping all other levels the same.


The estimates can be compared directly, for example.

The snp has a effect similar to something between str - 1 and str - 2.

str - 4 has twice the effect of the entire LD segment (i.e. interecpt)
