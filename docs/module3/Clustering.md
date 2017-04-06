Applied Machine Learning 410
========================================================
css: ../../assets/style/uw.css
author: Justin Donaldson
date: April-06-2017
autosize: true

Clustering
---------------------------------
(AKA: Birds of a feather)



Clustering History
========================================================
Clustering first originated in *anthropology*
![kroeber](img/kroeber.jpg)
- Divide people into culturally similar groups
- "...matrilinear descent and avoidance of
relatives-in-law were or tended to be inherently connected."

***
![quant_expression](img/quant_expression.png)


Clustering Needs
========================================================
![clustering needs](img/cluster_desire.png)
“Data Clustering: 50 Years Beyond K-Means”, A.K. Jain (2008

Overview
========================================================
type : sub-section
- Connectivity Clustering
- Centroid-based Clustering
- Density-based Clustering


Connectivity-based clustering
========================================================
type : sub-section

Hierarchical Clustering
============
- Group together observations based on *proximity* in space.
- Agglomerative clustering (clusters grow from individual observations)

***
<a title="By [[File:Hierarchical_clustering_diagram.png#file|]]: Stathis Sideris on 10/02/2005 derivative work:  Mhbrugman ([[File:Hierarchical_clustering_diagram.png#file|]]) [CC-BY-SA-3.0 (http://creativecommons.org/licenses/by-sa/3.0/) or GFDL (http://www.gnu.org/copyleft/fdl.html)], via Wikimedia Commons" href="https://commons.wikimedia.org/wiki/File%3AHierarchical_clustering_simple_diagram.svg"><img width="256" alt="Hierarchical clustering simple diagram" src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/ad/Hierarchical_clustering_simple_diagram.svg/256px-Hierarchical_clustering_simple_diagram.svg.png"/></a>

Hierarchical Clustering
============
For this analysis, let's look at arrest rates at a state level.

```r
  head(USArrests)
```

```
           Murder Assault UrbanPop Rape
Alabama      13.2     236       58 21.2
Alaska       10.0     263       48 44.5
Arizona       8.1     294       80 31.0
Arkansas      8.8     190       50 19.5
California    9.0     276       91 40.6
Colorado      7.9     204       78 38.7
```

Hierarchical Clustering
============
The arrest information is not expressed in distances, so we need to transform it.

```r
  head(as.matrix(dist(USArrests)))
```

```
            Alabama   Alaska   Arizona  Arkansas California Colorado
Alabama     0.00000 37.17701  63.00833  46.92814   55.52477 41.93256
Alaska     37.17701  0.00000  46.59249  77.19741   45.10222 66.47594
Arizona    63.00833 46.59249   0.00000 108.85192   23.19418 90.35115
Arkansas   46.92814 77.19741 108.85192   0.00000   97.58202 36.73486
California 55.52477 45.10222  23.19418  97.58202    0.00000 73.19713
Colorado   41.93256 66.47594  90.35115  36.73486   73.19713  0.00000
           Connecticut Delaware   Florida  Georgia   Hawaii     Idaho
Alabama      128.20694 16.80625 102.00162 25.84183 191.8031 116.76198
Alaska       159.40656 45.18296  79.97450 57.03026 221.1935 146.48498
Arizona      185.15953 58.61638  41.65453 86.03796 248.2690 176.81767
Arkansas      85.02829 53.01038 148.73574 25.58613 147.7760  70.58704
California   169.27711 49.29148  60.98073 73.99730 231.0711 162.61279
Colorado      98.08119 41.47783 131.40582 25.09303 159.1792  90.88641
           Illinois   Indiana     Iowa    Kansas  Kentucky Louisiana
Alabama    28.45488 123.34521 180.6101 121.51987 127.28417  15.45445
Alaska     42.91165 152.80409 209.9835 151.48020 156.61204  32.34888
Arizona    45.69781 181.89780 239.9915 180.02891 187.69030  48.49464
Arkansas   67.77027  78.47809 134.5949  76.75344  81.09285  61.54551
California 32.71880 166.22996 224.6347 164.51675 173.20791  41.63556
Colorado   47.66907  93.61506 152.0797  92.17972 101.02475  49.97499
              Maine  Maryland Massachusetts Michigan Minnesota Mississippi
Alabama    154.1453  64.99362      91.64851 28.48543  164.6510    27.39014
Alaska     183.8975  44.83949     123.25421 28.85775  194.2536    28.63512
Arizona    214.3274  15.01599     145.87591 39.87242  223.0883    52.70873
Arkansas   107.8507 111.64291      54.18118 71.10028  119.3246    69.68536
California 199.9311  36.34735     129.52471 27.74635  207.2225    55.68357
Colorado   127.9002  97.30041      59.90000 51.45483  134.7645    68.66440
            Missouri   Montana  Nebraska   Nevada New Hampshire New Jersey
Alabama     59.78829 127.39262 134.43697 37.43047      179.7362   83.24302
Alaska      89.30672 156.67358 164.11426 34.88682      209.2544  114.73557
Arizona    116.46738 187.54085 193.42360 44.79743      239.2556  135.85040
Arkansas    24.89438  81.16311  88.97893 74.28869      133.6783   49.84426
California 100.98891 172.99607 178.10081 26.74696      224.0554  119.04117
Colorado    29.17979 100.75167 105.66835 48.83421      151.5892   50.42083
           New Mexico New York North Carolina North Dakota      Ohio
Alabama      51.64349 33.71083      101.96102     192.4161 117.38761
Alaska       33.52193 43.18298       79.37607     221.3786 147.37334
Arizona      13.89604 40.85352       57.61961     252.8082 174.33818
Arkansas     97.93120 73.76212      147.18424     145.8555  74.36975
California   24.49510 26.90093       80.33212     238.2145 157.99851
Colorado     81.73622 52.27810      138.97759     165.7509  85.81754
            Oklahoma    Oregon Pennsylvania Rhode Island South Carolina
Alabama     85.84870  78.38686    131.08509     70.33811       44.18292
Alaska     116.42942 106.93012    161.60090    103.90380       27.55649
Arizona    143.93141 135.67288    188.86622    122.41887       36.89092
Arkansas    43.01267  36.89512     86.99086     42.18531       89.24887
California 128.77935 120.03958    172.99936    107.21311       47.06134
Colorado    57.09974  47.36412    101.03960     43.87949       82.64194
           South Dakota Tennessee    Texas      Utah  Vermont  Virginia
Alabama        151.0891  48.34760 41.56609 118.50270 190.3707  80.29533
Alaska         179.9481  77.88453 72.36221 148.27609 218.2905 110.64669
Arizona        211.7516 108.25812 93.27599 174.25734 251.4893 139.42471
Arkansas       104.4552  12.61428 32.74462  76.43900 143.5286  36.42156
California     197.5244  94.72766 77.38023 157.49263 237.4355 124.82091
Colorado       125.3021  28.00589 14.50103  85.62552 165.0477  53.41685
           Washington West Virginia Wisconsin   Wyoming
Alabama      92.82047      156.7924  183.7757  75.50709
Alaska      122.14700      185.6409  213.5754 106.74010
Arizona     149.29786      218.0061  242.3124 135.38039
Arkansas     51.20478      110.0711  138.3442  30.98726
California  133.10657      204.2537  226.4575 121.72034
Colorado     60.64206      132.3601  154.1152  52.03672
```

Hierarchical Clustering
============
R provides a good hierarchical clustering method in the standard distribution.

```r
  hc <-hclust(dist(USArrests))
  hc
```

```

Call:
hclust(d = dist(USArrests))

Cluster method   : complete 
Distance         : euclidean 
Number of objects: 50 
```

Hierarchical Clustering
============
Plotting it gives us a representation of the clusters in a tree-like form.

```r
plot(hc, hang=-1)
```

<img src="Clustering-figure/unnamed-chunk-4-1.png" title="plot of chunk unnamed-chunk-4" alt="plot of chunk unnamed-chunk-4" width="900px" />

Hierarchical Clustering
============
We can set a split point using "cutree", which will give us the desired number of clusters

```r
clusters = cutree(hc, k=5) # cut into 5 clusters
clusters
```

```
       Alabama         Alaska        Arizona       Arkansas     California 
             1              1              1              2              1 
      Colorado    Connecticut       Delaware        Florida        Georgia 
             2              3              1              4              2 
        Hawaii          Idaho       Illinois        Indiana           Iowa 
             5              3              1              3              5 
        Kansas       Kentucky      Louisiana          Maine       Maryland 
             3              3              1              5              1 
 Massachusetts       Michigan      Minnesota    Mississippi       Missouri 
             2              1              5              1              2 
       Montana       Nebraska         Nevada  New Hampshire     New Jersey 
             3              3              1              5              2 
    New Mexico       New York North Carolina   North Dakota           Ohio 
             1              1              4              5              3 
      Oklahoma         Oregon   Pennsylvania   Rhode Island South Carolina 
             2              2              3              2              1 
  South Dakota      Tennessee          Texas           Utah        Vermont 
             5              2              2              3              5 
      Virginia     Washington  West Virginia      Wisconsin        Wyoming 
             2              2              5              5              2 
```

Hierarchical Clustering
============
We can overlay the cluster boundaries on the original plot.

```r
plot(hc)
rect.hclust(hc, k=5, border="purple")
```

<img src="Clustering-figure/unnamed-chunk-6-1.png" title="plot of chunk unnamed-chunk-6" alt="plot of chunk unnamed-chunk-6" width="900px" />

Hierarchical Clustering
===========

```r
USArrests[clusters==1,]
```

```
               Murder Assault UrbanPop Rape
Alabama          13.2     236       58 21.2
Alaska           10.0     263       48 44.5
Arizona           8.1     294       80 31.0
California        9.0     276       91 40.6
Delaware          5.9     238       72 15.8
Illinois         10.4     249       83 24.0
Louisiana        15.4     249       66 22.2
Maryland         11.3     300       67 27.8
Michigan         12.1     255       74 35.1
Mississippi      16.1     259       44 17.1
Nevada           12.2     252       81 46.0
New Mexico       11.4     285       70 32.1
New York         11.1     254       86 26.1
South Carolina   14.4     279       48 22.5
```

```r
USArrests[clusters==2,]
```

```
              Murder Assault UrbanPop Rape
Arkansas         8.8     190       50 19.5
Colorado         7.9     204       78 38.7
Georgia         17.4     211       60 25.8
Massachusetts    4.4     149       85 16.3
Missouri         9.0     178       70 28.2
New Jersey       7.4     159       89 18.8
Oklahoma         6.6     151       68 20.0
Oregon           4.9     159       67 29.3
Rhode Island     3.4     174       87  8.3
Tennessee       13.2     188       59 26.9
Texas           12.7     201       80 25.5
Virginia         8.5     156       63 20.7
Washington       4.0     145       73 26.2
Wyoming          6.8     161       60 15.6
```

Hierarchical Clustering
===========
<img src="Clustering-figure/unnamed-chunk-8-1.png" title="plot of chunk unnamed-chunk-8" alt="plot of chunk unnamed-chunk-8" width="900px" />


Hierarchical Clustering
============

```r
head(as.matrix(UScitiesD))
```

```
           Atlanta Chicago Denver Houston LosAngeles Miami NewYork
Atlanta          0     587   1212     701       1936   604     748
Chicago        587       0    920     940       1745  1188     713
Denver        1212     920      0     879        831  1726    1631
Houston        701     940    879       0       1374   968    1420
LosAngeles    1936    1745    831    1374          0  2339    2451
Miami          604    1188   1726     968       2339     0    1092
           SanFrancisco Seattle Washington.DC
Atlanta            2139    2182           543
Chicago            1858    1737           597
Denver              949    1021          1494
Houston            1645    1891          1220
LosAngeles          347     959          2300
Miami              2594    2734           923
```

Hierarchical Clustering
============

```r
hc = hclust(dist(UScitiesD))
plot(hc)
rect.hclust(hc, k=3, border="purple")
```

<img src="Clustering-figure/unnamed-chunk-10-1.png" title="plot of chunk unnamed-chunk-10" alt="plot of chunk unnamed-chunk-10" width="900px" />

Hierarchical Clustering
============
Options - Distance (**good defaults**)
-----
- **Euclidean** $\|a-b \|_2 = \sqrt{\sum_i (a_i-b_i)^2}$
- Squared Euclidean $\|a-b \|_2^2 = \sum_i (a_i-b_i)^2$
- Manhattan $\|a-b \|_1 = \sum_i |a_i-b_i|$
- Maximum Distance $\|a-b \|_\infty = \max_i |a_i-b_i|$
- Mahalanobis Distance : ($S$  is the covariance matrix)* $\sqrt{(a-b)^{\top}S^{-1}(a-b)}$  

Hierarchical Clustering
============
Options - Linkage (**good defaults**)
-----
- single = closest neighbor to any point in cluster gets merged 
- **complete** = closest neighbor to furthest point in cluster gets added 
- UPGMA - Average of distances $d(x,y)$ : ${1 \over {|\mathcal{A}|\cdot|\mathcal{B}|}}\sum_{x \in \mathcal{A}}\sum_{ y \in \mathcal{B}} d(x,y)$
- centroid - distance between average of points in cluster.

Centroid-based clustering
========================================================
type : sub-section


K-Means
============
K-means is the classic centroid-based technique.

```r
  kc <-kmeans(dist(USArrests), 5)
  kc
```

```
K-means clustering with 5 clusters of sizes 12, 20, 2, 5, 11

Cluster means:
    Alabama    Alaska   Arizona  Arkansas California  Colorado Connecticut
1  40.62136  34.03632  34.00118  83.38388   32.30270  67.54820   160.53721
2 149.56564 179.11210 208.56598 104.26803  193.31161 121.13893    35.80897
3 101.98132  79.67528  49.63707 147.95999   70.65643 135.19170   227.90351
4  25.22935  55.64567  78.25796  39.00082   65.87818  24.60089   110.31680
5  73.92292 104.47051 131.03908  34.74916  116.14039  47.15276    57.46329
   Delaware   Florida   Georgia    Hawaii     Idaho  Illinois   Indiana
1  39.40545  70.20848  61.21676 223.36707 151.06893  30.30025 156.41202
2 151.58099 249.50416 125.47198  53.25924  38.20562 164.03901  33.31695
3 100.98494  19.26396 126.55123 291.51441 217.55445  91.38645 223.97154
4  26.23892 118.49243  21.40628 173.15447 101.31917  37.49009 106.11907
5  75.13618 171.89296  50.69707 119.95368  50.34740  86.91311  53.60144
       Iowa    Kansas  Kentucky Louisiana     Maine  Maryland
1 214.17285 154.66364 161.60320  29.64869 188.41483  36.97838
2  38.93004  33.90574  32.87698 162.72774  30.73726 213.55729
3 281.25892 222.09584 228.23207  89.18925 254.79614  41.21221
4 163.94337 104.35003 111.71999  34.25565 138.48700  83.05271
5 110.74996  51.66851  60.54855  86.10328  86.16791 136.43456
  Massachusetts  Michigan Minnesota Mississippi  Missouri   Montana
1     122.30332  25.55886 197.62750    36.59891  91.81696 161.57960
2      67.90653 170.03371  32.97264   173.26891  93.15621  32.12143
3     189.72218  84.69445 265.12939    81.73242 159.47445 228.28786
4      72.79166  41.80944 147.29164    50.55044  42.28261 111.67666
5      25.96958  92.92977  93.91532    99.27154  23.67919  60.15518
   Nebraska    Nevada New Hampshire New Jersey New Mexico  New York
1 167.85801  31.52167     213.42342  112.74986   27.74185  29.06504
2  29.18400 169.51365      38.55116   78.58514  199.10045 169.55461
3 235.11950  90.64507     280.37652  180.18006   55.52174  87.33721
4 117.61055  45.37255     163.21764   63.82298   68.70139  42.83759
5  64.79865  93.05082     110.07973   25.16945  121.86326  92.38786
  North Carolina North Dakota      Ohio  Oklahoma    Oregon Pennsylvania
1       77.04220    226.62563 149.45426 118.93184 110.68960    163.79051
2      250.48544     49.41974  39.94665  65.70698  74.65524     31.96314
3       19.26396    293.00584 217.33438 186.27518 178.41623    231.31087
4      122.37644    176.72414  99.14098  68.79518  60.98459    113.41754
5      174.92480    124.13150  46.66363  20.60562  20.33116     60.36805
  Rhode Island South Carolina South Dakota Tennessee     Texas      Utah
1    100.10533       33.33218    185.54771  82.84434  70.24009 149.80195
2     91.93839      192.81462     31.61779 102.62319 116.80849  42.51872
3    166.04521       61.81649    251.81454 149.31923 137.46298 217.83086
4     53.09638       65.82480    135.86264  35.57343  23.71672  99.70635
5     28.88866      117.50951     84.26120  30.67650  42.11178  47.52109
    Vermont  Virginia Washington West Virginia Wisconsin   Wyoming
1 224.94566 114.08720  124.60591     191.61982 216.81902 110.08598
2  52.13897  70.38132   61.65565      35.30830  41.89676  74.95322
3 290.77766 181.14632  192.52879     257.36255 284.21914 176.43007
4 175.54581  64.18894   74.55291     142.19551 166.46476  60.75605
5 124.01654  19.92724   24.75574      91.20473 112.98346  21.18628

Clustering vector:
       Alabama         Alaska        Arizona       Arkansas     California 
             4              1              1              5              1 
      Colorado    Connecticut       Delaware        Florida        Georgia 
             4              2              4              3              4 
        Hawaii          Idaho       Illinois        Indiana           Iowa 
             2              2              1              2              2 
        Kansas       Kentucky      Louisiana          Maine       Maryland 
             2              2              1              2              1 
 Massachusetts       Michigan      Minnesota    Mississippi       Missouri 
             5              1              2              1              5 
       Montana       Nebraska         Nevada  New Hampshire     New Jersey 
             2              2              1              2              5 
    New Mexico       New York North Carolina   North Dakota           Ohio 
             1              1              3              2              2 
      Oklahoma         Oregon   Pennsylvania   Rhode Island South Carolina 
             5              5              2              5              1 
  South Dakota      Tennessee          Texas           Utah        Vermont 
             2              5              4              2              2 
      Virginia     Washington  West Virginia      Wisconsin        Wyoming 
             5              5              2              2              5 

Within cluster sum of squares by cluster:
[1] 160409.608 644071.327   2336.177  52398.235 107193.337
 (between_SS / total_SS =  90.0 %)

Available components:

[1] "cluster"      "centers"      "totss"        "withinss"    
[5] "tot.withinss" "betweenss"    "size"         "iter"        
[9] "ifault"      
```
K-Means
============
K-means is the classic centroid-based technique.
- Assignment Step : $S_i^{(t)} = \big \{ x_p : \big \| x_p - m^{(t)}_i \big \|^2 \le \big \| x_p - m^{(t)}_j \big \|^2 \ \forall j, 1 \le j \le k \big\}$
  - Assign each data point to cluster whose mean is the smallest within-cluster sum of squares.
- Update Step : $m^{(t+1)}_i = \frac{1}{|S^{(t)}_i|} \sum_{x_j \in S^{(t)}_i} x_j$
  - Calculate new centroids

K-Means
============

```r
  colors =brewer.pal(5, "Spectral")
  arrests$clusters = colors[kc$cluster]
  ggpairs(arrests, mapping=ggplot2::aes(fill=clusters,color=clusters))
```

<img src="Clustering-figure/unnamed-chunk-12-1.png" title="plot of chunk unnamed-chunk-12" alt="plot of chunk unnamed-chunk-12" width="900px" />

K-Means
============
K-means is more sensitive to scaling

```r
  arrests = data.frame(scale(USArrests))
```

<img src="Clustering-figure/unnamed-chunk-14-1.png" title="plot of chunk unnamed-chunk-14" alt="plot of chunk unnamed-chunk-14" width="900px" />

Quick aside on scaling
=====
Field measurements can have different *scales*

```r
dat = cbind(
  x=rnorm(100, mean =  0, sd = 1),
  y=rnorm(100, mean = -4, sd = 2),
  z=rnorm(100, mean =  4, sd = 9)
  )
boxplot(dat)
```

<img src="Clustering-figure/unnamed-chunk-15-1.png" title="plot of chunk unnamed-chunk-15" alt="plot of chunk unnamed-chunk-15" width="900px" />

Quick aside on scaling
=====
In some cases it may be useful to *noramlize* the scales.
Standard normalization:
- mean = 0
- standard deviation = 1

```r
boxplot(scale(dat))
```

<img src="Clustering-figure/unnamed-chunk-16-1.png" title="plot of chunk unnamed-chunk-16" alt="plot of chunk unnamed-chunk-16" width="900px" />

K-means compared to Hierarchical Clustering
======
Hierarchical Clustering
-----
- cluster count can be determined post hoc
- linkages are robust against scaling differences in fields
- good at extracting "strand" clusters
- quadratic time complexity : ~ $O(n^2)$
- repeatable

K-Means
----
- "k" cluster count must be given up front.
- scaling affects centroid calculation, consider normalizing if possible.
- good at extracting "spherical" clusters
- near-linear time complexity : ~ $O(n)$
- requires random initialization, results may not be repeatable in some cases

Distribution-based clustering
========================================================
type : sub-section
Bio break? - 15m

Expectation Maximization
==========

```r
require(EMCluster)
```
- based on probability distributions
- no hard assignment of points to clusters, instead use probabilities
- underlying distributions can be anything, gaussians are common 
- optimize : $Q(\boldsymbol\theta|\boldsymbol\theta^{(t)}) = \operatorname{E}_{\mathbf{Z}|\mathbf{X},\boldsymbol\theta^{(t)}}\left[ \log L (\boldsymbol\theta;\mathbf{X},\mathbf{Z})  \right]$

Expectation Maximization
==========
EM works well when you have a good idea of the underlying distribution of the data.
In this case, normal deviates

```r
means = c(0,20,-15)
x = rnorm(300, means[1], 10)
y = rnorm(100, means[2], 1)
z = rnorm(100, means[3], 1)
dat = data.frame(x=c(x,y,z))
ggplot(data = dat,aes(x=x)) + geom_histogram()
```

<img src="Clustering-figure/unnamed-chunk-18-1.png" title="plot of chunk unnamed-chunk-18" alt="plot of chunk unnamed-chunk-18" width="900px" />

Expectation Maximization
==========
EM fits a GMM to the data

```r
library(EMCluster)
sem = shortemcluster(dat, simple.init(dat, nclass = 3))
em = emcluster(dat, sem, assign.class=T)
colors = brewer.pal(3, "Spectral")[em$class]
dat$class = colors
ggplot(data = dat, aes(x=x, fill=class)) + geom_histogram(position="dodge")
```

<img src="Clustering-figure/unnamed-chunk-19-1.png" title="plot of chunk unnamed-chunk-19" alt="plot of chunk unnamed-chunk-19" width="900px" />

Expectation Maximization
==========
EM models the distribution parameters, and can capture them even if distributions overlap.

```r
em$Mu
```

```
            [,1]
[1,]  -0.9011189
[2,] -15.0891657
[3,]  20.2240093
```

```r
as.matrix(means)
```

```
     [,1]
[1,]    0
[2,]   20
[3,]  -15
```

Expectation Maximization
==========
Considerations
- If distribution characteristics are known, EM works well at recovering from noise and overlap in cluster boundaries.
- Parameterization includes cluster count *and* mixture model specification. 
- Most similar to K-means, except cluster assignment is *soft*, rather than *hard* as in km.

Density-based Clustering
=========
type : sub-section


DBScan
==========

```r
library("dbscan")
```


