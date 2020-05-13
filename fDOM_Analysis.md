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
  head(Pellicer), format = "html",caption = "YSI exo2 Sonde Data",
  booktabs = TRUE
)
```

<table>
<caption>YSI exo2 Sonde Data</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Date..MM.DD.YYYY. </th>
   <th style="text-align:left;"> Time..HH.mm.ss. </th>
   <th style="text-align:left;"> X </th>
   <th style="text-align:right;"> Time..Fract..Sec. </th>
   <th style="text-align:left;"> Site.Name </th>
   <th style="text-align:right;"> Chlorophyll.RFU </th>
   <th style="text-align:right;"> Chlorophyll.ug.L </th>
   <th style="text-align:right;"> Cond.µS.cm </th>
   <th style="text-align:right;"> Depth.m </th>
   <th style="text-align:right;"> fDOM.QSU </th>
   <th style="text-align:right;"> fDOM.RFU </th>
   <th style="text-align:right;"> nLF.Cond.µS.cm </th>
   <th style="text-align:right;"> ODO...sat </th>
   <th style="text-align:right;"> ODO...local </th>
   <th style="text-align:right;"> ODO.mg.L </th>
   <th style="text-align:right;"> Pressure.psi.a </th>
   <th style="text-align:right;"> Sal.psu </th>
   <th style="text-align:right;"> SpCond.µS.cm </th>
   <th style="text-align:right;"> BGA.PE.RFU </th>
   <th style="text-align:right;"> BGA.PE.ug.L </th>
   <th style="text-align:right;"> TDS.mg.L </th>
   <th style="text-align:right;"> Turbidity.FNU </th>
   <th style="text-align:right;"> TSS.mg.L </th>
   <th style="text-align:right;"> Wiper.Position.volt </th>
   <th style="text-align:right;"> pH </th>
   <th style="text-align:right;"> pH.mV </th>
   <th style="text-align:right;"> Temp..C </th>
   <th style="text-align:right;"> Vertical.Position.m </th>
   <th style="text-align:right;"> Battery.V </th>
   <th style="text-align:right;"> Cable.Pwr.V </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 2019-07-02 </td>
   <td style="text-align:left;"> 18:12:18 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:left;"> Pellicer out </td>
   <td style="text-align:right;"> 0.99 </td>
   <td style="text-align:right;"> 3.95 </td>
   <td style="text-align:right;"> 60640.3 </td>
   <td style="text-align:right;"> 10.860 </td>
   <td style="text-align:right;"> 24.92 </td>
   <td style="text-align:right;"> 8.31 </td>
   <td style="text-align:right;"> 51939.2 </td>
   <td style="text-align:right;"> 101.3 </td>
   <td style="text-align:right;"> 105.0 </td>
   <td style="text-align:right;"> 6.06 </td>
   <td style="text-align:right;"> 15.763 </td>
   <td style="text-align:right;"> 34.58 </td>
   <td style="text-align:right;"> 52833.1 </td>
   <td style="text-align:right;"> 4.03 </td>
   <td style="text-align:right;"> 11.28 </td>
   <td style="text-align:right;"> 34342 </td>
   <td style="text-align:right;"> 11.20 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.190 </td>
   <td style="text-align:right;"> 7.9 </td>
   <td style="text-align:right;"> -54.7 </td>
   <td style="text-align:right;"> 32.737 </td>
   <td style="text-align:right;"> 10.859 </td>
   <td style="text-align:right;"> 6.19 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2019-07-02 </td>
   <td style="text-align:left;"> 18:27:18 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:left;"> Pellicer out </td>
   <td style="text-align:right;"> 0.94 </td>
   <td style="text-align:right;"> 3.75 </td>
   <td style="text-align:right;"> 60943.5 </td>
   <td style="text-align:right;"> 10.903 </td>
   <td style="text-align:right;"> 25.13 </td>
   <td style="text-align:right;"> 8.38 </td>
   <td style="text-align:right;"> 52002.9 </td>
   <td style="text-align:right;"> 103.4 </td>
   <td style="text-align:right;"> 107.2 </td>
   <td style="text-align:right;"> 6.17 </td>
   <td style="text-align:right;"> 15.823 </td>
   <td style="text-align:right;"> 34.64 </td>
   <td style="text-align:right;"> 52923.2 </td>
   <td style="text-align:right;"> 4.39 </td>
   <td style="text-align:right;"> 12.29 </td>
   <td style="text-align:right;"> 34400 </td>
   <td style="text-align:right;"> 11.54 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.179 </td>
   <td style="text-align:right;"> 7.9 </td>
   <td style="text-align:right;"> -54.7 </td>
   <td style="text-align:right;"> 32.934 </td>
   <td style="text-align:right;"> 10.901 </td>
   <td style="text-align:right;"> 6.19 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2019-07-02 </td>
   <td style="text-align:left;"> 18:42:18 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:left;"> Pellicer out </td>
   <td style="text-align:right;"> 1.26 </td>
   <td style="text-align:right;"> 5.04 </td>
   <td style="text-align:right;"> 60287.2 </td>
   <td style="text-align:right;"> 10.941 </td>
   <td style="text-align:right;"> 25.41 </td>
   <td style="text-align:right;"> 8.47 </td>
   <td style="text-align:right;"> 51794.1 </td>
   <td style="text-align:right;"> 102.5 </td>
   <td style="text-align:right;"> 106.3 </td>
   <td style="text-align:right;"> 6.16 </td>
   <td style="text-align:right;"> 15.879 </td>
   <td style="text-align:right;"> 34.46 </td>
   <td style="text-align:right;"> 52665.3 </td>
   <td style="text-align:right;"> 4.91 </td>
   <td style="text-align:right;"> 13.75 </td>
   <td style="text-align:right;"> 34232 </td>
   <td style="text-align:right;"> 13.81 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.186 </td>
   <td style="text-align:right;"> 7.9 </td>
   <td style="text-align:right;"> -54.5 </td>
   <td style="text-align:right;"> 32.577 </td>
   <td style="text-align:right;"> 10.941 </td>
   <td style="text-align:right;"> 6.19 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2019-07-02 </td>
   <td style="text-align:left;"> 18:57:18 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:left;"> Pellicer out </td>
   <td style="text-align:right;"> 1.53 </td>
   <td style="text-align:right;"> 6.10 </td>
   <td style="text-align:right;"> 59427.8 </td>
   <td style="text-align:right;"> 10.974 </td>
   <td style="text-align:right;"> 25.73 </td>
   <td style="text-align:right;"> 8.58 </td>
   <td style="text-align:right;"> 51597.4 </td>
   <td style="text-align:right;"> 100.4 </td>
   <td style="text-align:right;"> 104.1 </td>
   <td style="text-align:right;"> 6.09 </td>
   <td style="text-align:right;"> 15.928 </td>
   <td style="text-align:right;"> 34.28 </td>
   <td style="text-align:right;"> 52396.4 </td>
   <td style="text-align:right;"> 5.54 </td>
   <td style="text-align:right;"> 15.50 </td>
   <td style="text-align:right;"> 34058 </td>
   <td style="text-align:right;"> 14.07 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.189 </td>
   <td style="text-align:right;"> 7.9 </td>
   <td style="text-align:right;"> -54.6 </td>
   <td style="text-align:right;"> 32.026 </td>
   <td style="text-align:right;"> 10.973 </td>
   <td style="text-align:right;"> 6.19 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2019-07-02 </td>
   <td style="text-align:left;"> 19:12:18 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:left;"> Pellicer out </td>
   <td style="text-align:right;"> 1.64 </td>
   <td style="text-align:right;"> 6.56 </td>
   <td style="text-align:right;"> 58886.9 </td>
   <td style="text-align:right;"> 11.007 </td>
   <td style="text-align:right;"> 25.42 </td>
   <td style="text-align:right;"> 8.47 </td>
   <td style="text-align:right;"> 51526.9 </td>
   <td style="text-align:right;"> 98.4 </td>
   <td style="text-align:right;"> 102.0 </td>
   <td style="text-align:right;"> 6.00 </td>
   <td style="text-align:right;"> 15.977 </td>
   <td style="text-align:right;"> 34.21 </td>
   <td style="text-align:right;"> 52274.9 </td>
   <td style="text-align:right;"> 6.18 </td>
   <td style="text-align:right;"> 17.32 </td>
   <td style="text-align:right;"> 33979 </td>
   <td style="text-align:right;"> 13.70 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.191 </td>
   <td style="text-align:right;"> 7.9 </td>
   <td style="text-align:right;"> -54.5 </td>
   <td style="text-align:right;"> 31.622 </td>
   <td style="text-align:right;"> 11.008 </td>
   <td style="text-align:right;"> 6.19 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2019-07-02 </td>
   <td style="text-align:left;"> 19:27:18 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:left;"> Pellicer out </td>
   <td style="text-align:right;"> 2.16 </td>
   <td style="text-align:right;"> 8.64 </td>
   <td style="text-align:right;"> 58759.5 </td>
   <td style="text-align:right;"> 11.047 </td>
   <td style="text-align:right;"> 24.95 </td>
   <td style="text-align:right;"> 8.32 </td>
   <td style="text-align:right;"> 51538.8 </td>
   <td style="text-align:right;"> 97.1 </td>
   <td style="text-align:right;"> 100.7 </td>
   <td style="text-align:right;"> 5.94 </td>
   <td style="text-align:right;"> 16.036 </td>
   <td style="text-align:right;"> 34.21 </td>
   <td style="text-align:right;"> 52271.7 </td>
   <td style="text-align:right;"> 5.88 </td>
   <td style="text-align:right;"> 16.45 </td>
   <td style="text-align:right;"> 33977 </td>
   <td style="text-align:right;"> 12.96 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1.179 </td>
   <td style="text-align:right;"> 7.9 </td>
   <td style="text-align:right;"> -54.2 </td>
   <td style="text-align:right;"> 31.498 </td>
   <td style="text-align:right;"> 11.047 </td>
   <td style="text-align:right;"> 6.19 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
</tbody>
</table>

```r
knitr::kable(
  head(Precip), format = "html",caption = "NERR Climatic Data",
  booktabs = TRUE
)
```

<table>
<caption>NERR Climatic Data</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Station_Code </th>
   <th style="text-align:left;"> isSWMP </th>
   <th style="text-align:left;"> DateTimeStamp </th>
   <th style="text-align:right;"> Historical </th>
   <th style="text-align:right;"> ProvisionalPlus </th>
   <th style="text-align:right;"> Frequency </th>
   <th style="text-align:left;"> F_Record </th>
   <th style="text-align:right;"> ATemp </th>
   <th style="text-align:left;"> F_ATemp </th>
   <th style="text-align:right;"> RH </th>
   <th style="text-align:left;"> F_RH </th>
   <th style="text-align:right;"> BP </th>
   <th style="text-align:left;"> F_BP </th>
   <th style="text-align:right;"> WSpd </th>
   <th style="text-align:left;"> F_WSpd </th>
   <th style="text-align:right;"> MaxWSpd </th>
   <th style="text-align:left;"> F_MaxWSpd </th>
   <th style="text-align:left;"> MaxWSpdT </th>
   <th style="text-align:right;"> Wdir </th>
   <th style="text-align:left;"> F_Wdir </th>
   <th style="text-align:right;"> SDWDir </th>
   <th style="text-align:left;"> F_SDWDir </th>
   <th style="text-align:right;"> TotPAR </th>
   <th style="text-align:left;"> F_TotPAR </th>
   <th style="text-align:right;"> TotPrcp </th>
   <th style="text-align:left;"> F_TotPrcp </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> gtmpcmet </td>
   <td style="text-align:left;"> P </td>
   <td style="text-align:left;"> 7/2/2019 0:00 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 25.7 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 1016 </td>
   <td style="text-align:left;"> &lt;1&gt; [SSD] </td>
   <td style="text-align:right;"> 1.5 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 3.0 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:left;"> 23:45 </td>
   <td style="text-align:right;"> 258 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 18 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
  </tr>
  <tr>
   <td style="text-align:left;"> gtmpcmet </td>
   <td style="text-align:left;"> P </td>
   <td style="text-align:left;"> 7/2/2019 0:15 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 25.6 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 1016 </td>
   <td style="text-align:left;"> &lt;1&gt; [SSD] </td>
   <td style="text-align:right;"> 2.0 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 3.6 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:left;"> 0:12 </td>
   <td style="text-align:right;"> 262 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 18 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
  </tr>
  <tr>
   <td style="text-align:left;"> gtmpcmet </td>
   <td style="text-align:left;"> P </td>
   <td style="text-align:left;"> 7/2/2019 0:30 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 25.6 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 1016 </td>
   <td style="text-align:left;"> &lt;1&gt; [SSD] </td>
   <td style="text-align:right;"> 1.8 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 3.2 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:left;"> 0:28 </td>
   <td style="text-align:right;"> 271 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 18 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
  </tr>
  <tr>
   <td style="text-align:left;"> gtmpcmet </td>
   <td style="text-align:left;"> P </td>
   <td style="text-align:left;"> 7/2/2019 0:45 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 25.5 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 1016 </td>
   <td style="text-align:left;"> &lt;1&gt; [SSD] </td>
   <td style="text-align:right;"> 1.8 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 3.7 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:left;"> 0:36 </td>
   <td style="text-align:right;"> 266 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
  </tr>
  <tr>
   <td style="text-align:left;"> gtmpcmet </td>
   <td style="text-align:left;"> P </td>
   <td style="text-align:left;"> 7/2/2019 1:00 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 25.4 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 1016 </td>
   <td style="text-align:left;"> &lt;1&gt; [SSD] </td>
   <td style="text-align:right;"> 1.6 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 2.9 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:left;"> 0:45 </td>
   <td style="text-align:right;"> 270 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
  </tr>
  <tr>
   <td style="text-align:left;"> gtmpcmet </td>
   <td style="text-align:left;"> P </td>
   <td style="text-align:left;"> 7/2/2019 1:15 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 25.3 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 1015 </td>
   <td style="text-align:left;"> &lt;1&gt; [SSD] </td>
   <td style="text-align:right;"> 1.7 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 3.2 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:left;"> 1:00 </td>
   <td style="text-align:right;"> 274 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:left;"> &lt;0&gt; </td>
  </tr>
</tbody>
</table>


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

