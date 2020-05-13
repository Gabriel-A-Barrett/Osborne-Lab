---
title: "Analyzing fDOM"
author: "Gabriel Barrett, Nathan Shivers, Jacqueline Stephens"
date: "5/12/2020"
output:
  html_document:
    theme: cosmo
    keep_md: true
---





```r
sonde_unite <- unite(Pellicer,
                     "DateTime", Date..MM.DD.YYYY., Time..HH.mm.ss.)

sonde <- mutate(sonde_unite, 
                TimeStamp = ymd_hms(sonde_unite$DateTime))

Precip1 <- mutate(Precip, 
                  TimeStamp = mdy_hm(Precip$DateTimeStamp))

ult_data <- difference_join(sonde, Precip1, 
                            by = "TimeStamp", mode = "inner", max_dist = 180) %>%
  dplyr::select(5:30,44:57) %>%
  rename(Sonde_Time = TimeStamp.x, Precip_Time = TimeStamp.y)
```


```r
knitr::kable(
  head(ult_data), caption = "",
  booktabs = TRUE
)
```



Table: 

 Chlorophyll.RFU   Chlorophyll.ug.L   Cond.µS.cm   Depth.m   fDOM.QSU   fDOM.RFU   nLF.Cond.µS.cm   ODO...sat   ODO...local   ODO.mg.L   Pressure.psi.a   Sal.psu   SpCond.µS.cm   BGA.PE.RFU   BGA.PE.ug.L   TDS.mg.L   Turbidity.FNU   TSS.mg.L   Wiper.Position.volt    pH   pH.mV   Temp..C   Vertical.Position.m   Battery.V   Cable.Pwr.V  Sonde_Time             WSpd  F_WSpd    MaxWSpd  F_MaxWSpd   MaxWSpdT    Wdir  F_Wdir    SDWDir  F_SDWDir    TotPAR  F_TotPAR    TotPrcp  F_TotPrcp   Precip_Time         
----------------  -----------------  -----------  --------  ---------  ---------  ---------------  ----------  ------------  ---------  ---------------  --------  -------------  -----------  ------------  ---------  --------------  ---------  --------------------  ----  ------  --------  --------------------  ----------  ------------  --------------------  -----  -------  --------  ----------  ---------  -----  -------  -------  ---------  -------  ---------  --------  ----------  --------------------
            0.99               3.95      60640.3    10.860      24.92       8.31          51939.2       101.3         105.0       6.06           15.763     34.58        52833.1         4.03         11.28      34342           11.20          0                 1.190   7.9   -54.7    32.737                10.859        6.19             0  2019-07-02 18:12:18     1.4  <0>           2.3  <0>         18:05        166  <0>           14  <0>           60.5  <0>               0  <0>         2019-07-02 18:15:00 
            0.94               3.75      60943.5    10.903      25.13       8.38          52002.9       103.4         107.2       6.17           15.823     34.64        52923.2         4.39         12.29      34400           11.54          0                 1.179   7.9   -54.7    32.934                10.901        6.19             0  2019-07-02 18:27:18     1.3  <0>           2.3  <0>         18:17        172  <0>           14  <0>           47.7  <0>               0  <0>         2019-07-02 18:30:00 
            1.26               5.04      60287.2    10.941      25.41       8.47          51794.1       102.5         106.3       6.16           15.879     34.46        52665.3         4.91         13.75      34232           13.81          0                 1.186   7.9   -54.5    32.577                10.941        6.19             0  2019-07-02 18:42:18     0.7  <0>           1.3  <0>         18:30        188  <0>           14  <0>           52.2  <0>               0  <0>         2019-07-02 18:45:00 
            1.53               6.10      59427.8    10.974      25.73       8.58          51597.4       100.4         104.1       6.09           15.928     34.28        52396.4         5.54         15.50      34058           14.07          0                 1.189   7.9   -54.6    32.026                10.973        6.19             0  2019-07-02 18:57:18     0.5  <0>           0.8  <0>         18:49        218  <0>           11  <0>           36.7  <0>               0  <0>         2019-07-02 19:00:00 
            1.64               6.56      58886.9    11.007      25.42       8.47          51526.9        98.4         102.0       6.00           15.977     34.21        52274.9         6.18         17.32      33979           13.70          0                 1.191   7.9   -54.5    31.622                11.008        6.19             0  2019-07-02 19:12:18     0.9  <0>           1.8  <0>         19:08        252  <0>           16  <0>           15.5  <0>               0  <0>         2019-07-02 19:15:00 
            2.16               8.64      58759.5    11.047      24.95       8.32          51538.8        97.1         100.7       5.94           16.036     34.21        52271.7         5.88         16.45      33977           12.96          0                 1.179   7.9   -54.2    31.498                11.047        6.19             0  2019-07-02 19:27:18     0.7  <0>           1.3  <0>         19:26        268  <0>           17  <0>            3.9  <0>               0  <0>         2019-07-02 19:30:00 






```r
fDOM_drought <- ggplot(ult_data) + geom_line(aes(x = Sonde_Time, y = fDOM.QSU)) + 
  scale_x_datetime(limits = c(drought_min, drought_max),
                   date_labels = "%b %d",) + 
  theme_classic() +
  ylab("fDOM (QSU)") +
  xlab("")


Precip_drought <- ggplot(ult_data) + geom_line(aes(x= Precip_Time, y= TotPrcp)) + 
  scale_x_datetime(limits = c(drought_min,drought_max),
                   date_labels = "%b %d",) + 
  theme_classic() +
  ylab("Total Precipitation (mm)") +
  xlab("Date")
```


```r
fDOM_rain <- ggplot(ult_data) + geom_line(aes(x = Sonde_Time, y = fDOM.QSU)) + 
  scale_x_datetime(limits = c(rain_min, rain_max),
                   date_labels = "%b %d") + 
  theme_classic() +
  ylab("") +
  xlab("") 


Precip_rain <- ggplot(ult_data) + geom_line(aes(x= Precip_Time, y= TotPrcp)) + 
  scale_x_datetime(limits = c(rain_min,rain_max),
                   date_labels = "%b %d",) + 
  theme_classic() +
  ylab("") +
  xlab("Date")
```


```r
ggarrange(fDOM_drought, fDOM_rain, Precip_drought, Precip_rain, ncol = 2, nrow = 2, align = "v", 
          labels = c("           Drought", "        Precipitation"))
```

![](fDOM_Analysis_files/figure-html/unnamed-chunk-6-1.png)<!-- -->




```r
ult_data_d$size <- ifelse(ult_data_d$fDOM.QSU < mean(ult_data_d$fDOM.QSU), "Small", "Big") 

ult_data_r$size <- ifelse(ult_data_r$fDOM.QSU < mean(ult_data_r$fDOM.QSU), "Small", "Big")

drought <- ggplot(ult_data_d) + 
  aes(x=size)+
  geom_bar() +
  ylab("fDOM Counts")+
  theme_classic()

rain <- ggplot(ult_data_r) + 
  aes(x=size)+
  geom_bar() + 
  ylab("fDOM Counts")+
  theme_classic() 

ggarrange(drought, rain, nrow = 1, ncol = 2, labels = c("   Drought", "     Rain"))
```

![](fDOM_Analysis_files/figure-html/Contingency Table-1.png)<!-- -->


```r
pulse <- ggplot(ult_data) + geom_line(aes(Sonde_Time,fDOM.QSU)) + 
  scale_x_datetime(limits = c(ymd_hm("2019-07-27 00:00"), ymd_hm("2019-07-28 00:00")),
                   date_labels = "%H",
                   breaks = date_breaks("60 min"),
                   expand = c(0, 0)) + 
  theme_classic() +
  ylab("fDOM (QSU)") + 
  xlab("") + 
  annotate("rect", xmin = ymd_hm("2019-07-27 13:00"), xmax = ymd_hm("2019-07-27 14:45"), ymin = 55, ymax = 115, alpha = .2) +
  annotate("text", label = "Pulse of fDOM", x = ymd_hm("2019-07-27 14:00"), y = 130)

storm <- ggplot(ult_data) + geom_line(aes(Precip_Time,TotPrcp)) + 
  scale_x_datetime(limits = c(ymd_hm("2019-07-27 00:00"), ymd_hm("2019-07-28 00:00")),
                   date_labels = "%H",
                   breaks = date_breaks("60 min"),
                   expand = c(0, 0)) + 
  theme_classic() +
  ylab("Precipitation (mm)") + 
  xlab("Hour")
```


```r
ggarrange(pulse, storm, nrow = 2, ncol = 1, align = "v", labels = " July 27th Storm Event")
```

```
## Warning: Removed 2421 rows containing missing values (geom_path).

## Warning: Removed 2421 rows containing missing values (geom_path).
```

![](fDOM_Analysis_files/figure-html/unnamed-chunk-9-1.png)<!-- -->



```r
ggpairs(data = ult_data, columns = c(5,38), title="Precipitation's Predictive Potential for fDOM") +
theme_classic()
```

![](fDOM_Analysis_files/figure-html/unnamed-chunk-10-1.png)<!-- -->


```r
#A tool used when heteroscedasticity variability of a variable is unequal across a range of values 
#via the Thiel-sen test

zen_data <- data.frame("fDOM"=ult_data$fDOM.QSU,"Time"=as.numeric(ult_data$Sonde_Time, units="minutes"))

zen <- zyp.sen(fDOM~Time,zen_data)

ci <- confint.zyp(zen)

print(ci)
```

```
##                   0.025         0.975
## Intercept -1.958484e+04 -1.950374e+04
## Time       1.146715e-05  1.360089e-05
```

