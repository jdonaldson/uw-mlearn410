Applied Machine Learning 410
========================================================
css: ../../assets/style/uw.css
author: Justin Donaldson
date: April - Some time around then I guess
autosize: true

Decision Trees and Random Forests
---------------------------------
Divide and Concur


Setup
=====

```r
install.packages("tree", repos="http://cran.rstudio.com/")
```

```

The downloaded binary packages are in
	/var/folders/zs/wpzz7px56hx33_xp67zr_mhnz7kvzw/T//RtmpdjaOWs/downloaded_packages
```

```r
require(tree)
```
install and require ``tree`` package.

Example
=======

```r
data(cpus, package="MASS")
cpus.ltr <- tree(log10(perf) ~ syct+mmin+mmax+cach+chmin+chmax, cpus)
snip.tree(cpus.ltr,2)
```

```
node), split, n, deviance, yval
      * denotes terminal node

 1) root 209 43.12000 1.753  
   2) cach < 27 143 11.79000 1.525 *
   3) cach > 27 66  7.64300 2.249  
     6) mmax < 28000 41  2.34100 2.062  
      12) cach < 96.5 34  1.59200 2.008  
        24) mmax < 11240 14  0.42460 1.827 *
        25) mmax > 11240 20  0.38340 2.135 *
      13) cach > 96.5 7  0.17170 2.324 *
     7) mmax > 28000 25  1.52300 2.555  
      14) cach < 56 7  0.06929 2.268 *
      15) cach > 56 18  0.65350 2.667 *
```



First Slide
========================================================

For more details on authoring R presentations please visit <https://support.rstudio.com/hc/en-us/articles/200486468>.

- Bullet 1
- Bullet 2
- Bullet 3

Slide With Code
========================================================


```r
summary(cars)
```

```
     speed           dist       
 Min.   : 4.0   Min.   :  2.00  
 1st Qu.:12.0   1st Qu.: 26.00  
 Median :15.0   Median : 36.00  
 Mean   :15.4   Mean   : 42.98  
 3rd Qu.:19.0   3rd Qu.: 56.00  
 Max.   :25.0   Max.   :120.00  
```

Slide With Plot
========================================================

![plot of chunk unnamed-chunk-4](Trees-figure/unnamed-chunk-4-1.png)
