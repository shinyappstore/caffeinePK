
# R package: caffsim

> Monte Carlo Simulation of Plasma Caffeine Concentrations by Using Population Pharmacokinetic Model

- This package is used for publication of the paper about pharmacokinetics of plasma caffeine.
- Gitbook <http://asancpt.github.io/CaffeineEdison> is created solely dependent on this R package.
- Reproducible research is expected.



## Installation


```r
install.pacakges("devtools")
devtools::install_github("asancpt/caffsim")

# Simply create single dose dataset
caffsim::Dataset(Weight = 20, Dose = 200, N = 20) 

# Simply create multiple dose dataset
caffsim::DatasetMulti(Weight = 20, Dose = 200, N = 20, Tau = 12) 
```

## Single dose

### Create a PK dataset for caffeine single dose


```r
MyDataset <- caffsim::Dataset(Weight = 20, Dose = 200, N = 20)
knitr::kable(head(MyDataset))
```

      Tmax       Cmax         AUC   Half_life         CL          V          Ka          Ke
----------  ---------  ----------  ----------  ---------  ---------  ----------  ----------
 0.4541990   16.46882   130.65865    5.173522   1.530706   11.42733    9.521546   0.1339513
 0.6745180   10.02211    68.06539    4.212161   2.938351   17.85975    5.317354   0.1645236
 0.5132871   15.47555   113.63005    4.718932   1.760098   11.98526    7.914429   0.1468553
 0.9029560   13.37353    93.67009    4.178842   2.135153   12.87513    3.562795   0.1658354
 0.9850155   12.10619   101.36777    5.071957   1.973014   14.44017    3.399654   0.1366337
 0.1857226   16.82389   163.59497    6.608742   1.222532   11.65858   30.681544   0.1048611

### Create a dataset for concentration-time curve


```r
MyConcTime <- ConcTime(Weight = 20, Dose = 200, N = 20)
knitr::kable(head(MyConcTime))
```



 Subject   Time       Conc
--------  -----  ---------
       1    0.0   0.000000
       1    0.1   1.276234
       1    0.2   2.451772
       1    0.3   3.533573
       1    0.4   4.528126
       1    0.5   5.441481

### Create a concentration-time curve


```r
Plot(MyConcTime)
```

![](Figures/MyPlotMyConcTime-1.png)<!-- -->

### Create plots for publication (according to the amount of caffeine)

- `cowplot` package is required


```r
#install.packages("cowplot") # if you don't have it
library(cowplot)

MyPlotPub <- lapply(
    c(seq(100, 800, by = 100)), 
    function(x) PlotMulti(ConcTime(20, x, 20)) + 
        theme(legend.position="none") + 
        labs(title = paste0("Single Dose ", x, "mg")))

plot(plot_grid(MyPlotPub[[1]], MyPlotPub[[2]],
               MyPlotPub[[3]], MyPlotPub[[4]],
               MyPlotPub[[5]], MyPlotPub[[6]],
               MyPlotPub[[7]], MyPlotPub[[8]],
               labels=LETTERS[1:8], ncol = 2, nrow = 4))
```

![](Figures/MyPlotPub-1.png)<!-- -->

## Multiple dose

### Create a PK dataset for caffeine multiple doses


```r
MyDatasetMulti <- DatasetMulti(Weight = 20, Dose = 200, N = 20, Tau = 12)
knitr::kable(head(MyDatasetMulti))
```

     TmaxS       CmaxS        AUCS         AI       Aavss       Cavss      Cmaxss      Cminss
----------  ----------  ----------  ---------  ----------  ----------  ----------  ----------
 0.9672339   12.619490    83.99915   1.132942    93.14789    6.999929   16.992378    1.993930
 0.4522879   14.299054    98.96153   1.184420   107.31511    8.246794   18.165845    2.828508
 0.0704218   18.424888   102.17318   1.125741    91.05209    8.514431   21.010178    2.346749
 0.3557365   21.127091   229.36221   1.467814   174.54408   19.113518   32.079839   10.224316
 0.9765485    7.375711    49.08763   1.132235    92.94357    4.090636    9.945664    1.161566
 5.3823675   10.540067   214.54069   1.719125   229.00267   17.878391   26.786811   11.205155

### Create a dataset for concentration-time curve


```r
MyConcTimeMulti <- ConcTimeMulti(Weight = 20, Dose = 200, N = 20, Tau = 12, Repeat = 10)
knitr::kable(head(MyConcTimeMulti))
```

       CL          V        Ka          Ke   Subject   Time        Conc    ConcOrig   ConcTemp
---------  ---------  --------  ----------  --------  -----  ----------  ----------  ---------
 1.758682   10.94079   1.16128   0.1607454         1    0.0    0.000000    0.000000          0
 1.758682   10.94079   1.16128   0.1607454         1    0.2    3.726128    3.726128          0
 1.758682   10.94079   1.16128   0.1607454         1    0.4    6.562104    6.562104          0
 1.758682   10.94079   1.16128   0.1607454         1    0.6    8.696147    8.696147          0
 1.758682   10.94079   1.16128   0.1607454         1    0.8   10.277350   10.277350          0
 1.758682   10.94079   1.16128   0.1607454         1    1.0   11.423790   11.423790          0

### Create a concentration-time curve


```r
PlotMulti(MyConcTimeMulti)
```

![](Figures/MyPlotMultiMyConcTimeMulti-1.png)<!-- -->

### Create plots for publication (according to dosing interval)

- `cowplot` package is required


```r
#install.packages("cowplot") # if you don't have it
library(cowplot)

MyPlotMultiPub <- lapply(
    c(seq(4, 32, by = 4)), 
    function(x) PlotMulti(ConcTimeMulti(20, 250, 20, x, 15)) + 
        theme(legend.position="none") + 
        labs(title = paste0("q", x, "hr" )))

plot(plot_grid(MyPlotMultiPub[[1]], MyPlotMultiPub[[2]],
               MyPlotMultiPub[[3]], MyPlotMultiPub[[4]],
               MyPlotMultiPub[[5]], MyPlotMultiPub[[6]],
               MyPlotMultiPub[[7]], MyPlotMultiPub[[8]],
               labels=LETTERS[1:8], ncol = 2, nrow = 4))
```

![](Figures/MyPlotMultiPub-1.png)<!-- -->
