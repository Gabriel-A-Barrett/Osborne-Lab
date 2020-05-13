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
knitr::kable(
  head(Pellicer[5:10]), format = "html",caption = "YSI exo2 Sonde Data",
  booktabs = TRUE
)
```

<table>
<caption>YSI exo2 Sonde Data</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Site.Name </th>
   <th style="text-align:right;"> Chlorophyll.RFU </th>
   <th style="text-align:right;"> Chlorophyll.ug.L </th>
   <th style="text-align:right;"> Cond.µS.cm </th>
   <th style="text-align:right;"> Depth.m </th>
   <th style="text-align:right;"> fDOM.QSU </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Pellicer out </td>
   <td style="text-align:right;"> 0.99 </td>
   <td style="text-align:right;"> 3.95 </td>
   <td style="text-align:right;"> 60640.3 </td>
   <td style="text-align:right;"> 10.860 </td>
   <td style="text-align:right;"> 24.92 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Pellicer out </td>
   <td style="text-align:right;"> 0.94 </td>
   <td style="text-align:right;"> 3.75 </td>
   <td style="text-align:right;"> 60943.5 </td>
   <td style="text-align:right;"> 10.903 </td>
   <td style="text-align:right;"> 25.13 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Pellicer out </td>
   <td style="text-align:right;"> 1.26 </td>
   <td style="text-align:right;"> 5.04 </td>
   <td style="text-align:right;"> 60287.2 </td>
   <td style="text-align:right;"> 10.941 </td>
   <td style="text-align:right;"> 25.41 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Pellicer out </td>
   <td style="text-align:right;"> 1.53 </td>
   <td style="text-align:right;"> 6.10 </td>
   <td style="text-align:right;"> 59427.8 </td>
   <td style="text-align:right;"> 10.974 </td>
   <td style="text-align:right;"> 25.73 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Pellicer out </td>
   <td style="text-align:right;"> 1.64 </td>
   <td style="text-align:right;"> 6.56 </td>
   <td style="text-align:right;"> 58886.9 </td>
   <td style="text-align:right;"> 11.007 </td>
   <td style="text-align:right;"> 25.42 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Pellicer out </td>
   <td style="text-align:right;"> 2.16 </td>
   <td style="text-align:right;"> 8.64 </td>
   <td style="text-align:right;"> 58759.5 </td>
   <td style="text-align:right;"> 11.047 </td>
   <td style="text-align:right;"> 24.95 </td>
  </tr>
</tbody>
</table>

```r
knitr::kable(
  head(Precip[1:5]), caption = "NERR Climatic Data",
  booktabs = TRUE
)
```



Table: NERR Climatic Data

Station_Code   isSWMP   DateTimeStamp    Historical   ProvisionalPlus
-------------  -------  --------------  -----------  ----------------
gtmpcmet       P        7/2/2019 0:00             0                 1
gtmpcmet       P        7/2/2019 0:15             0                 1
gtmpcmet       P        7/2/2019 0:30             0                 1
gtmpcmet       P        7/2/2019 0:45             0                 1
gtmpcmet       P        7/2/2019 1:00             0                 1
gtmpcmet       P        7/2/2019 1:15             0                 1


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

