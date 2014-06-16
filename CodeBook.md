 Getting and Cleaning Data Course Project
========================================================
###Modified from http://mathijsdevaan.com/archives/227 (Mathijs de Vaan/md2999@columbia.edu)
# File-Name:       download_and_unzip.R           
# Date:            06_13_2014                                
# Author:          Jeff Roberts
# Email:           uwhawk@uw.edu                                      
# Purpose:         download and unzip files for the web
# Packages Used:   see load libraries and data 
# Machine:         Roberts' Dell laptop
# Github Path:     https://github.com/uwhawk/Getting_and_Cleaning_Data_Course_Project.git

Download and unzip the files from https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 


```r
# load packages
require("R.utils")
```

```
## Loading required package: R.utils
```

```
## Warning: package 'R.utils' was built under R version 3.0.3
```

```
## Loading required package: R.oo
## Loading required package: R.methodsS3
## R.methodsS3 v1.6.1 (2014-01-04) successfully loaded. See ?R.methodsS3 for help.
## R.oo v1.18.0 (2014-02-22) successfully loaded. See ?R.oo for help.
## 
## Attaching package: 'R.oo'
## 
## The following objects are masked from 'package:methods':
## 
##     getClasses, getMethods
## 
## The following objects are masked from 'package:base':
## 
##     attach, detach, gc, load, save
## 
## R.utils v1.32.4 (2014-05-14) successfully loaded. See ?R.utils for help.
## 
## Attaching package: 'R.utils'
## 
## The following object is masked from 'package:utils':
## 
##     timestamp
## 
## The following objects are masked from 'package:base':
## 
##     cat, commandArgs, getOption, inherits, isOpen, parse, warnings
```

```r
# set workdrive
setwd("C:/R/zip")

# open a temporary directory to store data
temp <- tempdir()

# define the url  
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"

file <- basename(url)
  
# download the zip file and store in the "file" object 
download.file(url, file)
```

```
## Error: unsupported URL scheme
```

```r
# unzip the file and store in the temp folder
gunzip(file, exdir = temp)
```

```
## Error: Argument 'filename' and 'destname' are identical:
## getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip
```

```r
(l = list.files(temp))
```

```
## [1] "file6e472fa4a7c"
```

```r
# unlink the temporary directory
unlink(temp)
```

move working files from: C:\R\zip\getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip\UCI HAR Dataset\test & C:\R\zip\getdata###%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip###\UCI HAR Dataset\train from download to working directory: C:\R\zip using Windows Explorer.


```r
setwd("C:/R/zip")
test<- as.matrix(read.table("X_test.txt", header=FALSE))
train<-as.matrix(read.table("X_train.txt", header=FALSE))
###open row labels in dataframe
test.row<- as.matrix(read.table("y_test.txt", header=FALSE))
train.row<- as.matrix(read.table("y_train.txt", header=FALSE))
```

Assign Row Labels

```r
rownames(test) <- test.row[,1]
rownames(train) <- train.row[,1]
```


open features.txt which contains Column Lablels and descriptive names

```r
features<-as.data.frame(read.table("features.txt", header=FALSE))

features$V2<-gsub("mean()", "Mean Value",features$V2,ignore.case = TRUE)
features$V2<-gsub("std()", " Standard Deviation",features$V2,ignore.case = TRUE)
features$V2<-gsub("mad()", " Median absolute deviation",features$V2,ignore.case = TRUE)
features$V2<-gsub("max()", "Largest Value in array", features$V2,ignore.case = TRUE)
features$V2<-gsub("min()", "Smallest Value in array", features$V2,ignore.case = TRUE)
features$V2<-gsub("sma()", "Signal magnitude area", features$V2,ignore.case = TRUE)
features$V2<-gsub("energy()", "Energy measure. Sum of he squares divided by the number of values.", features$V2,ignore.case = TRUE)
features$V2<-gsub("iqr()", "Interquartile range", features$V2,ignore.case = TRUE)
features$V2<-gsub("entropy()", "Signal entropoy", features$V2,ignore.case = TRUE)
features$V2<-gsub("arCoeff()", "Autoregresion coefficients with Burg order equal to 4", features$V2,ignore.case = TRUE)
features$V2<-gsub("correlation()", "correlation coefficient between two signals",features$V2,ignore.case = TRUE)
features$V2<-gsub("maxInds()", "Index of the frequency component with largest magnitue",features$V2,ignore.case = TRUE)
features$V2<-gsub("meanFreq()", "Weighted average of the frequncy components to obtain a mean frequency",features$V2,ignore.case = TRUE)
features$V2<-gsub("skewness()", "Skewness of the frequency domain signal",features$V2,ignore.case = TRUE)
features$V2<-gsub("kurtosis()", "Kurtosis of the frequency domain signal",features$V2,ignore.case = TRUE)
features$V2<-gsub("bandsEnergy()", "Energy of frequency interval within the 64 bins of the FFT of each window",features$V2,ignore.case = TRUE)
features$V2<-gsub("angle()", "Angle between two vectors",features$V2,ignore.case = TRUE)
```

Assign Descriptive Column Labels to Training and Test Databases

```r
colnames(test) <- features[,2]
colnames(train) <- features[,2]
```

Combine test and training sets into a single dataset, add row labels as a column "user" to Subtotal Column Averages on.

```r
single<-as.data.frame(rbind(test, train))
a<-as.data.frame(as.numeric(rownames(single)))
colnames(a)<-"user"
single<-cbind(single,a)
```

```
## Warning: some row.names duplicated:
## 2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,101,102,103,104,105,106,107,108,109,111,112,113,114,115,116,117,118,119,120,121,122,123,124,125,126,127,128,129,130,131,132,133,135,136,137,138,139,140,141,142,143,144,145,146,147,148,149,150,151,152,153,154,155,156,157,158,159,160,161,162,163,164,165,166,167,168,169,170,171,172,173,174,175,176,177,178,179,180,181,182,183,184,185,186,187,188,189,190,191,192,193,194,195,196,197,198,199,200,201,202,203,204,205,206,207,208,209,210,211,212,213,214,215,216,217,218,219,220,221,222,223,224,225,226,227,228,229,230,231,232,233,234,235,236,237,238,239,240,241,242,243,244,245,246,247,248,249,250,251,252,253,254,255,256,257,258,259,260,261,262,263,264,265,266,267,268,269,270,271,272,273,274,275,276,277,278,279,280,281,282,283,284,285,286,287,288,289,290,291,292,293,294,295,296,297,298,299,300,301,302,303,304,305,306,307,308,309,310,311,312,313,314,315,316,317,318,319,320,321,322,323,324,325,326,327,328,329,330,331,332,333,334,335,336,337,338,339,340,341,342,343,344,345,346,347,348,349,350,351,352,353,354,355,356,357,358,359,360,361,362,363,364,365,366,367,368,369,370,371,372,373,374,375,376,377,378,379,380,381,382,383,384,385,386,387,388,389,390,391,392,393,394,395,396,397,398,399,400,401,402,403,404,405,406,407,408,409,410,411,412,413,414,415,416,417,418,419,420,421,422,423,424,425,426,427,428,429,430,431,432,433,434,435,436,437,438,439,440,441,442,443,444,445,446,447,448,449,450,451,452,453,454,455,456,457,458,459,460,461,462,463,464,465,466,467,468,469,470,471,472,473,474,475,476,477,478,479,480,481,482,483,484,485,486,487,488,489,490,491,492,493,494,495,496,497,498,499,500,501,502,503,504,505,506,507,508,509,510,511,512,513,514,515,516,517,518,519,520,521,522,523,524,525,526,527,528,529,530,531,532,533,534,535,536,537,538,539,540,541,542,543,544,545,546,547,548,549,550,551,552,553,554,555,556,557,558,559,560,561,562,563,564,565,566,567,568,569,570,571,572,573,574,575,576,577,578,579,580,581,582,583,584,585,586,587,588,589,590,591,592,593,594,595,596,597,598,599,600,601,602,603,604,605,606,607,608,609,610,611,612,613,614,615,616,617,618,619,620,621,622,623,624,625,626,627,628,629,630,631,632,633,634,635,636,637,638,639,640,641,642,643,644,645,646,647,648,649,650,651,652,653,654,655,656,657,658,659,660,661,662,663,664,665,666,667,668,669,670,671,672,673,674,675,676,677,678,679,680,681,682,683,684,685,686,687,688,689,690,691,692,693,694,695,696,697,698,699,700,701,702,703,704,705,706,707,708,709,710,711,712,713,714,715,716,717,718,719,720,721,722,723,724,725,726,727,728,729,730,731,732,733,734,735,736,737,738,739,740,741,742,743,744,745,746,747,748,749,750,751,752,753,754,755,756,757,758,759,760,761,762,763,764,765,766,767,768,769,770,771,772,773,774,775,776,777,778,779,780,781,782,783,784,785,786,787,788,789,790,791,792,793,794,795,796,797,798,799,800,801,802,803,804,805,806,807,808,809,810,811,812,813,814,815,816,817,818,819,820,821,822,823,824,825,826,827,828,829,830,831,832,833,834,835,836,837,838,839,840,841,842,843,844,845,846,847,848,849,850,851,852,853,854,855,856,857,858,859,860,861,862,863,864,865,866,867,868,869,870,871,872,873,874,875,876,877,878,879,880,881,882,883,884,885,886,887,888,889,890,891,892,893,894,895,896,897,898,899,900,901,902,903,904,905,906,907,908,909,910,911,912,913,914,915,916,917,918,919,920,921,922,923,924,925,926,927,928,929,930,931,932,933,934,935,936,937,938,939,940,941,942,943,944,945,946,947,948,949,950,951,952,953,954,955,956,957,958,959,960,961,962,963,964,965,966,967,968,969,970,971,972,973,974,975,976,977,978,979,980,981,982,983,984,985,986,987,988,989,990,991,992,993,994,995,996,997,998,999,1000,1001,1002,1003,1004,1005,1006,1007,1008,1009,1010,1011,1012,1013,1014,1015,1016,1017,1018,1019,1020,1021,1022,1023,1024,1025,1026,1027,1028,1029,1030,1031,1032,1033,1034,1035,1036,1037,1038,1039,1040,1041,1042,1043,1044,1045,1046,1047,1048,1049,1050,1051,1052,1053,1054,1055,1056,1057,1058,1059,1060,1061,1062,1063,1064,1065,1066,1067,1068,1069,1070,1071,1072,1073,1074,1075,1076,1077,1078,1079,1080,1081,1082,1083,1084,1085,1086,1087,1088,1089,1090,1091,1092,1093,1094,1095,1096,1097,1098,1099,1100,1101,1102,1103,1104,1105,1106,1107,1108,1109,1110,1111,1112,1113,1114,1115,1116,1117,1118,1119,1120,1121,1122,1123,1124,1125,1126,1127,1128,1129,1130,1131,1132,1133,1134,1135,1136,1137,1138,1139,1140,1141,1142,1143,1144,1145,1146,1147,1148,1149,1150,1151,1152,1153,1154,1155,1156,1157,1158,1159,1160,1161,1162,1163,1164,1165,1166,1167,1168,1169,1170,1171,1172,1173,1174,1175,1176,1177,1178,1179,1180,1181,1182,1183,1184,1185,1186,1187,1188,1189,1190,1191,1192,1193,1194,1195,1196,1197,1198,1199,1200,1201,1202,1203,1204,1205,1206,1207,1208,1209,1210,1211,1212,1213,1214,1215,1216,1217,1218,1219,1220,1221,1222,1223,1224,1225,1226,1227,1228,1229,1230,1231,1232,1233,1234,1235,1236,1237,1238,1239,1240,1241,1242,1243,1244,1245,1246,1247,1248,1249,1250,1251,1252,1253,1254,1255,1256,1257,1258,1259,1260,1261,1262,1263,1264,1265,1266,1267,1268,1269,1270,1271,1272,1273,1274,1275,1276,1277,1278,1279,1280,1281,1282,1283,1284,1285,1286,1287,1288,1289,1290,1291,1292,1293,1294,1295,1296,1297,1298,1299,1300,1301,1302,1303,1304,1305,1306,1307,1308,1309,1310,1311,1312,1313,1314,1315,1316,1317,1318,1319,1320,1321,1322,1323,1324,1325,1326,1327,1328,1329,1330,1331,1332,1333,1334,1335,1336,1337,1338,1339,1340,1341,1342,1343,1344,1345,1346,1347,1348,1349,1350,1351,1352,1353,1354,1355,1356,1357,1358,1359,1360,1361,1362,1363,1364,1365,1366,1367,1368,1369,1370,1371,1372,1373,1374,1375,1376,1377,1378,1379,1380,1381,1382,1383,1384,1385,1386,1387,1388,1389,1390,1391,1392,1393,1394,1395,1396,1397,1398,1399,1400,1401,1402,1403,1404,1405,1406,1407,1408,1409,1410,1411,1412,1413,1414,1415,1416,1417,1418,1419,1420,1421,1422,1423,1424,1425,1426,1427,1428,1429,1430,1431,1432,1433,1434,1435,1436,1437,1438,1439,1440,1441,1442,1443,1444,1445,1446,1447,1448,1449,1450,1451,1452,1453,1454,1455,1456,1457,1458,1459,1460,1461,1462,1463,1464,1465,1466,1467,1468,1469,1470,1471,1472,1473,1474,1475,1476,1477,1478,1479,1480,1481,1482,1483,1484,1485,1486,1487,1488,1489,1490,1491,1492,1493,1494,1495,1496,1497,1498,1499,1500,1501,1502,1503,1504,1505,1506,1507,1508,1509,1510,1511,1512,1513,1514,1515,1516,1517,1518,1519,1520,1521,1522,1523,1524,1525,1526,1527,1528,1529,1530,1531,1532,1533,1534,1535,1536,1537,1538,1539,1540,1541,1542,1543,1544,1545,1546,1547,1548,1549,1550,1551,1552,1553,1554,1555,1556,1557,1558,1559,1560,1561,1562,1563,1564,1565,1566,1567,1568,1569,1570,1571,1572,1573,1574,1575,1576,1577,1578,1579,1580,1581,1582,1583,1584,1585,1586,1587,1588,1589,1590,1591,1592,1593,1594,1595,1596,1597,1598,1599,1600,1601,1602,1603,1604,1605,1606,1607,1608,1609,1610,1611,1612,1613,1614,1615,1616,1617,1618,1619,1620,1621,1622,1623,1624,1625,1626,1627,1628,1629,1630,1631,1632,1633,1634,1635,1636,1637,1638,1639,1640,1641,1642,1643,1644,1645,1646,1647,1648,1649,1650,1651,1652,1653,1654,1655,1656,1657,1658,1659,1660,1661,1662,1663,1664,1665,1666,1667,1668,1669,1670,1671,1672,1673,1674,1675,1676,1677,1678,1679,1680,1681,1682,1683,1684,1685,1686,1687,1688,1689,1690,1691,1692,1693,1694,1695,1696,1697,1698,1699,1700,1701,1702,1703,1704,1705,1706,1707,1708,1709,1710,1711,1712,1713,1714,1715,1716,1717,1718,1719,1720,1721,1722,1723,1724,1725,1726,1727,1728,1729,1730,1731,1732,1733,1734,1735,1736,1737,1738,1739,1740,1741,1742,1743,1744,1745,1746,1747,1748,1749,1750,1751,1752,1753,1754,1755,1756,1757,1758,1759,1760,1761,1762,1763,1764,1765,1766,1767,1768,1769,1770,1771,1772,1773,1774,1775,1776,1777,1778,1779,1780,1781,1782,1783,1784,1785,1786,1787,1788,1789,1790,1791,1792,1793,1794,1795,1796,1797,1798,1799,1800,1801,1802,1803,1804,1805,1806,1807,1808,1809,1810,1811,1812,1813,1814,1815,1816,1817,1818,1819,1820,1821,1822,1823,1824,1825,1826,1827,1828,1829,1830,1831,1832,1833,1834,1835,1836,1837,1838,1839,1840,1841,1842,1843,1844,1845,1846,1847,1848,1849,1850,1851,1852,1853,1854,1855,1856,1857,1858
```

Extract mean and standard deviation measurements for Tidy Dataset, compare dimentions of the datasets before and after mean and standard deviation extraction

```r
single.ms<-single[,grep("mean|Mean|standard|Standard|user", colnames(single), value=TRUE)]
dim(single)
```

```
## [1] 10299   562
```

```r
dim(single.ms)
```

```
## [1] 10299    87
```

```r
colnames(single)
```

```
##   [1] "tBodyAcc-Mean Value()-X"                                                                                                                                   
##   [2] "tBodyAcc-Mean Value()-Y"                                                                                                                                   
##   [3] "tBodyAcc-Mean Value()-Z"                                                                                                                                   
##   [4] "tBodyAcc- Standard Deviation()-X"                                                                                                                          
##   [5] "tBodyAcc- Standard Deviation()-Y"                                                                                                                          
##   [6] "tBodyAcc- Standard Deviation()-Z"                                                                                                                          
##   [7] "tBodyAcc- Median absolute deviation()-X"                                                                                                                   
##   [8] "tBodyAcc- Median absolute deviation()-Y"                                                                                                                   
##   [9] "tBodyAcc- Median absolute deviation()-Z"                                                                                                                   
##  [10] "tBodyAcc-Largest Value in array()-X"                                                                                                                       
##  [11] "tBodyAcc-Largest Value in array()-Y"                                                                                                                       
##  [12] "tBodyAcc-Largest Value in array()-Z"                                                                                                                       
##  [13] "tBodyAcc-Signal magnitude areallest Value in array()-X"                                                                                                    
##  [14] "tBodyAcc-Signal magnitude areallest Value in array()-Y"                                                                                                    
##  [15] "tBodyAcc-Signal magnitude areallest Value in array()-Z"                                                                                                    
##  [16] "tBodyAcc-Signal magnitude area()"                                                                                                                          
##  [17] "tBodyAcc-Energy measure. Sum of he squares divided by the number of values.()-X"                                                                           
##  [18] "tBodyAcc-Energy measure. Sum of he squares divided by the number of values.()-Y"                                                                           
##  [19] "tBodyAcc-Energy measure. Sum of he squares divided by the number of values.()-Z"                                                                           
##  [20] "tBodyAcc-Interquartile range()-X"                                                                                                                          
##  [21] "tBodyAcc-Interquartile range()-Y"                                                                                                                          
##  [22] "tBodyAcc-Interquartile range()-Z"                                                                                                                          
##  [23] "tBodyAcc-Signal entropoy()-X"                                                                                                                              
##  [24] "tBodyAcc-Signal entropoy()-Y"                                                                                                                              
##  [25] "tBodyAcc-Signal entropoy()-Z"                                                                                                                              
##  [26] "tBodyAcc-Autoregresion coefficients with Burg order equal to 4()-X,1"                                                                                      
##  [27] "tBodyAcc-Autoregresion coefficients with Burg order equal to 4()-X,2"                                                                                      
##  [28] "tBodyAcc-Autoregresion coefficients with Burg order equal to 4()-X,3"                                                                                      
##  [29] "tBodyAcc-Autoregresion coefficients with Burg order equal to 4()-X,4"                                                                                      
##  [30] "tBodyAcc-Autoregresion coefficients with Burg order equal to 4()-Y,1"                                                                                      
##  [31] "tBodyAcc-Autoregresion coefficients with Burg order equal to 4()-Y,2"                                                                                      
##  [32] "tBodyAcc-Autoregresion coefficients with Burg order equal to 4()-Y,3"                                                                                      
##  [33] "tBodyAcc-Autoregresion coefficients with Burg order equal to 4()-Y,4"                                                                                      
##  [34] "tBodyAcc-Autoregresion coefficients with Burg order equal to 4()-Z,1"                                                                                      
##  [35] "tBodyAcc-Autoregresion coefficients with Burg order equal to 4()-Z,2"                                                                                      
##  [36] "tBodyAcc-Autoregresion coefficients with Burg order equal to 4()-Z,3"                                                                                      
##  [37] "tBodyAcc-Autoregresion coefficients with Burg order equal to 4()-Z,4"                                                                                      
##  [38] "tBodyAcc-correlation coefficient between two signals()-X,Y"                                                                                                
##  [39] "tBodyAcc-correlation coefficient between two signals()-X,Z"                                                                                                
##  [40] "tBodyAcc-correlation coefficient between two signals()-Y,Z"                                                                                                
##  [41] "tGravityAcc-Mean Value()-X"                                                                                                                                
##  [42] "tGravityAcc-Mean Value()-Y"                                                                                                                                
##  [43] "tGravityAcc-Mean Value()-Z"                                                                                                                                
##  [44] "tGravityAcc- Standard Deviation()-X"                                                                                                                       
##  [45] "tGravityAcc- Standard Deviation()-Y"                                                                                                                       
##  [46] "tGravityAcc- Standard Deviation()-Z"                                                                                                                       
##  [47] "tGravityAcc- Median absolute deviation()-X"                                                                                                                
##  [48] "tGravityAcc- Median absolute deviation()-Y"                                                                                                                
##  [49] "tGravityAcc- Median absolute deviation()-Z"                                                                                                                
##  [50] "tGravityAcc-Largest Value in array()-X"                                                                                                                    
##  [51] "tGravityAcc-Largest Value in array()-Y"                                                                                                                    
##  [52] "tGravityAcc-Largest Value in array()-Z"                                                                                                                    
##  [53] "tGravityAcc-Signal magnitude areallest Value in array()-X"                                                                                                 
##  [54] "tGravityAcc-Signal magnitude areallest Value in array()-Y"                                                                                                 
##  [55] "tGravityAcc-Signal magnitude areallest Value in array()-Z"                                                                                                 
##  [56] "tGravityAcc-Signal magnitude area()"                                                                                                                       
##  [57] "tGravityAcc-Energy measure. Sum of he squares divided by the number of values.()-X"                                                                        
##  [58] "tGravityAcc-Energy measure. Sum of he squares divided by the number of values.()-Y"                                                                        
##  [59] "tGravityAcc-Energy measure. Sum of he squares divided by the number of values.()-Z"                                                                        
##  [60] "tGravityAcc-Interquartile range()-X"                                                                                                                       
##  [61] "tGravityAcc-Interquartile range()-Y"                                                                                                                       
##  [62] "tGravityAcc-Interquartile range()-Z"                                                                                                                       
##  [63] "tGravityAcc-Signal entropoy()-X"                                                                                                                           
##  [64] "tGravityAcc-Signal entropoy()-Y"                                                                                                                           
##  [65] "tGravityAcc-Signal entropoy()-Z"                                                                                                                           
##  [66] "tGravityAcc-Autoregresion coefficients with Burg order equal to 4()-X,1"                                                                                   
##  [67] "tGravityAcc-Autoregresion coefficients with Burg order equal to 4()-X,2"                                                                                   
##  [68] "tGravityAcc-Autoregresion coefficients with Burg order equal to 4()-X,3"                                                                                   
##  [69] "tGravityAcc-Autoregresion coefficients with Burg order equal to 4()-X,4"                                                                                   
##  [70] "tGravityAcc-Autoregresion coefficients with Burg order equal to 4()-Y,1"                                                                                   
##  [71] "tGravityAcc-Autoregresion coefficients with Burg order equal to 4()-Y,2"                                                                                   
##  [72] "tGravityAcc-Autoregresion coefficients with Burg order equal to 4()-Y,3"                                                                                   
##  [73] "tGravityAcc-Autoregresion coefficients with Burg order equal to 4()-Y,4"                                                                                   
##  [74] "tGravityAcc-Autoregresion coefficients with Burg order equal to 4()-Z,1"                                                                                   
##  [75] "tGravityAcc-Autoregresion coefficients with Burg order equal to 4()-Z,2"                                                                                   
##  [76] "tGravityAcc-Autoregresion coefficients with Burg order equal to 4()-Z,3"                                                                                   
##  [77] "tGravityAcc-Autoregresion coefficients with Burg order equal to 4()-Z,4"                                                                                   
##  [78] "tGravityAcc-correlation coefficient between two signals()-X,Y"                                                                                             
##  [79] "tGravityAcc-correlation coefficient between two signals()-X,Z"                                                                                             
##  [80] "tGravityAcc-correlation coefficient between two signals()-Y,Z"                                                                                             
##  [81] "tBodyAccJerk-Mean Value()-X"                                                                                                                               
##  [82] "tBodyAccJerk-Mean Value()-Y"                                                                                                                               
##  [83] "tBodyAccJerk-Mean Value()-Z"                                                                                                                               
##  [84] "tBodyAccJerk- Standard Deviation()-X"                                                                                                                      
##  [85] "tBodyAccJerk- Standard Deviation()-Y"                                                                                                                      
##  [86] "tBodyAccJerk- Standard Deviation()-Z"                                                                                                                      
##  [87] "tBodyAccJerk- Median absolute deviation()-X"                                                                                                               
##  [88] "tBodyAccJerk- Median absolute deviation()-Y"                                                                                                               
##  [89] "tBodyAccJerk- Median absolute deviation()-Z"                                                                                                               
##  [90] "tBodyAccJerk-Largest Value in array()-X"                                                                                                                   
##  [91] "tBodyAccJerk-Largest Value in array()-Y"                                                                                                                   
##  [92] "tBodyAccJerk-Largest Value in array()-Z"                                                                                                                   
##  [93] "tBodyAccJerk-Signal magnitude areallest Value in array()-X"                                                                                                
##  [94] "tBodyAccJerk-Signal magnitude areallest Value in array()-Y"                                                                                                
##  [95] "tBodyAccJerk-Signal magnitude areallest Value in array()-Z"                                                                                                
##  [96] "tBodyAccJerk-Signal magnitude area()"                                                                                                                      
##  [97] "tBodyAccJerk-Energy measure. Sum of he squares divided by the number of values.()-X"                                                                       
##  [98] "tBodyAccJerk-Energy measure. Sum of he squares divided by the number of values.()-Y"                                                                       
##  [99] "tBodyAccJerk-Energy measure. Sum of he squares divided by the number of values.()-Z"                                                                       
## [100] "tBodyAccJerk-Interquartile range()-X"                                                                                                                      
## [101] "tBodyAccJerk-Interquartile range()-Y"                                                                                                                      
## [102] "tBodyAccJerk-Interquartile range()-Z"                                                                                                                      
## [103] "tBodyAccJerk-Signal entropoy()-X"                                                                                                                          
## [104] "tBodyAccJerk-Signal entropoy()-Y"                                                                                                                          
## [105] "tBodyAccJerk-Signal entropoy()-Z"                                                                                                                          
## [106] "tBodyAccJerk-Autoregresion coefficients with Burg order equal to 4()-X,1"                                                                                  
## [107] "tBodyAccJerk-Autoregresion coefficients with Burg order equal to 4()-X,2"                                                                                  
## [108] "tBodyAccJerk-Autoregresion coefficients with Burg order equal to 4()-X,3"                                                                                  
## [109] "tBodyAccJerk-Autoregresion coefficients with Burg order equal to 4()-X,4"                                                                                  
## [110] "tBodyAccJerk-Autoregresion coefficients with Burg order equal to 4()-Y,1"                                                                                  
## [111] "tBodyAccJerk-Autoregresion coefficients with Burg order equal to 4()-Y,2"                                                                                  
## [112] "tBodyAccJerk-Autoregresion coefficients with Burg order equal to 4()-Y,3"                                                                                  
## [113] "tBodyAccJerk-Autoregresion coefficients with Burg order equal to 4()-Y,4"                                                                                  
## [114] "tBodyAccJerk-Autoregresion coefficients with Burg order equal to 4()-Z,1"                                                                                  
## [115] "tBodyAccJerk-Autoregresion coefficients with Burg order equal to 4()-Z,2"                                                                                  
## [116] "tBodyAccJerk-Autoregresion coefficients with Burg order equal to 4()-Z,3"                                                                                  
## [117] "tBodyAccJerk-Autoregresion coefficients with Burg order equal to 4()-Z,4"                                                                                  
## [118] "tBodyAccJerk-correlation coefficient between two signals()-X,Y"                                                                                            
## [119] "tBodyAccJerk-correlation coefficient between two signals()-X,Z"                                                                                            
## [120] "tBodyAccJerk-correlation coefficient between two signals()-Y,Z"                                                                                            
## [121] "tBodyGyro-Mean Value()-X"                                                                                                                                  
## [122] "tBodyGyro-Mean Value()-Y"                                                                                                                                  
## [123] "tBodyGyro-Mean Value()-Z"                                                                                                                                  
## [124] "tBodyGyro- Standard Deviation()-X"                                                                                                                         
## [125] "tBodyGyro- Standard Deviation()-Y"                                                                                                                         
## [126] "tBodyGyro- Standard Deviation()-Z"                                                                                                                         
## [127] "tBodyGyro- Median absolute deviation()-X"                                                                                                                  
## [128] "tBodyGyro- Median absolute deviation()-Y"                                                                                                                  
## [129] "tBodyGyro- Median absolute deviation()-Z"                                                                                                                  
## [130] "tBodyGyro-Largest Value in array()-X"                                                                                                                      
## [131] "tBodyGyro-Largest Value in array()-Y"                                                                                                                      
## [132] "tBodyGyro-Largest Value in array()-Z"                                                                                                                      
## [133] "tBodyGyro-Signal magnitude areallest Value in array()-X"                                                                                                   
## [134] "tBodyGyro-Signal magnitude areallest Value in array()-Y"                                                                                                   
## [135] "tBodyGyro-Signal magnitude areallest Value in array()-Z"                                                                                                   
## [136] "tBodyGyro-Signal magnitude area()"                                                                                                                         
## [137] "tBodyGyro-Energy measure. Sum of he squares divided by the number of values.()-X"                                                                          
## [138] "tBodyGyro-Energy measure. Sum of he squares divided by the number of values.()-Y"                                                                          
## [139] "tBodyGyro-Energy measure. Sum of he squares divided by the number of values.()-Z"                                                                          
## [140] "tBodyGyro-Interquartile range()-X"                                                                                                                         
## [141] "tBodyGyro-Interquartile range()-Y"                                                                                                                         
## [142] "tBodyGyro-Interquartile range()-Z"                                                                                                                         
## [143] "tBodyGyro-Signal entropoy()-X"                                                                                                                             
## [144] "tBodyGyro-Signal entropoy()-Y"                                                                                                                             
## [145] "tBodyGyro-Signal entropoy()-Z"                                                                                                                             
## [146] "tBodyGyro-Autoregresion coefficients with Burg order equal to 4()-X,1"                                                                                     
## [147] "tBodyGyro-Autoregresion coefficients with Burg order equal to 4()-X,2"                                                                                     
## [148] "tBodyGyro-Autoregresion coefficients with Burg order equal to 4()-X,3"                                                                                     
## [149] "tBodyGyro-Autoregresion coefficients with Burg order equal to 4()-X,4"                                                                                     
## [150] "tBodyGyro-Autoregresion coefficients with Burg order equal to 4()-Y,1"                                                                                     
## [151] "tBodyGyro-Autoregresion coefficients with Burg order equal to 4()-Y,2"                                                                                     
## [152] "tBodyGyro-Autoregresion coefficients with Burg order equal to 4()-Y,3"                                                                                     
## [153] "tBodyGyro-Autoregresion coefficients with Burg order equal to 4()-Y,4"                                                                                     
## [154] "tBodyGyro-Autoregresion coefficients with Burg order equal to 4()-Z,1"                                                                                     
## [155] "tBodyGyro-Autoregresion coefficients with Burg order equal to 4()-Z,2"                                                                                     
## [156] "tBodyGyro-Autoregresion coefficients with Burg order equal to 4()-Z,3"                                                                                     
## [157] "tBodyGyro-Autoregresion coefficients with Burg order equal to 4()-Z,4"                                                                                     
## [158] "tBodyGyro-correlation coefficient between two signals()-X,Y"                                                                                               
## [159] "tBodyGyro-correlation coefficient between two signals()-X,Z"                                                                                               
## [160] "tBodyGyro-correlation coefficient between two signals()-Y,Z"                                                                                               
## [161] "tBodyGyroJerk-Mean Value()-X"                                                                                                                              
## [162] "tBodyGyroJerk-Mean Value()-Y"                                                                                                                              
## [163] "tBodyGyroJerk-Mean Value()-Z"                                                                                                                              
## [164] "tBodyGyroJerk- Standard Deviation()-X"                                                                                                                     
## [165] "tBodyGyroJerk- Standard Deviation()-Y"                                                                                                                     
## [166] "tBodyGyroJerk- Standard Deviation()-Z"                                                                                                                     
## [167] "tBodyGyroJerk- Median absolute deviation()-X"                                                                                                              
## [168] "tBodyGyroJerk- Median absolute deviation()-Y"                                                                                                              
## [169] "tBodyGyroJerk- Median absolute deviation()-Z"                                                                                                              
## [170] "tBodyGyroJerk-Largest Value in array()-X"                                                                                                                  
## [171] "tBodyGyroJerk-Largest Value in array()-Y"                                                                                                                  
## [172] "tBodyGyroJerk-Largest Value in array()-Z"                                                                                                                  
## [173] "tBodyGyroJerk-Signal magnitude areallest Value in array()-X"                                                                                               
## [174] "tBodyGyroJerk-Signal magnitude areallest Value in array()-Y"                                                                                               
## [175] "tBodyGyroJerk-Signal magnitude areallest Value in array()-Z"                                                                                               
## [176] "tBodyGyroJerk-Signal magnitude area()"                                                                                                                     
## [177] "tBodyGyroJerk-Energy measure. Sum of he squares divided by the number of values.()-X"                                                                      
## [178] "tBodyGyroJerk-Energy measure. Sum of he squares divided by the number of values.()-Y"                                                                      
## [179] "tBodyGyroJerk-Energy measure. Sum of he squares divided by the number of values.()-Z"                                                                      
## [180] "tBodyGyroJerk-Interquartile range()-X"                                                                                                                     
## [181] "tBodyGyroJerk-Interquartile range()-Y"                                                                                                                     
## [182] "tBodyGyroJerk-Interquartile range()-Z"                                                                                                                     
## [183] "tBodyGyroJerk-Signal entropoy()-X"                                                                                                                         
## [184] "tBodyGyroJerk-Signal entropoy()-Y"                                                                                                                         
## [185] "tBodyGyroJerk-Signal entropoy()-Z"                                                                                                                         
## [186] "tBodyGyroJerk-Autoregresion coefficients with Burg order equal to 4()-X,1"                                                                                 
## [187] "tBodyGyroJerk-Autoregresion coefficients with Burg order equal to 4()-X,2"                                                                                 
## [188] "tBodyGyroJerk-Autoregresion coefficients with Burg order equal to 4()-X,3"                                                                                 
## [189] "tBodyGyroJerk-Autoregresion coefficients with Burg order equal to 4()-X,4"                                                                                 
## [190] "tBodyGyroJerk-Autoregresion coefficients with Burg order equal to 4()-Y,1"                                                                                 
## [191] "tBodyGyroJerk-Autoregresion coefficients with Burg order equal to 4()-Y,2"                                                                                 
## [192] "tBodyGyroJerk-Autoregresion coefficients with Burg order equal to 4()-Y,3"                                                                                 
## [193] "tBodyGyroJerk-Autoregresion coefficients with Burg order equal to 4()-Y,4"                                                                                 
## [194] "tBodyGyroJerk-Autoregresion coefficients with Burg order equal to 4()-Z,1"                                                                                 
## [195] "tBodyGyroJerk-Autoregresion coefficients with Burg order equal to 4()-Z,2"                                                                                 
## [196] "tBodyGyroJerk-Autoregresion coefficients with Burg order equal to 4()-Z,3"                                                                                 
## [197] "tBodyGyroJerk-Autoregresion coefficients with Burg order equal to 4()-Z,4"                                                                                 
## [198] "tBodyGyroJerk-correlation coefficient between two signals()-X,Y"                                                                                           
## [199] "tBodyGyroJerk-correlation coefficient between two signals()-X,Z"                                                                                           
## [200] "tBodyGyroJerk-correlation coefficient between two signals()-Y,Z"                                                                                           
## [201] "tBodyAccMag-Mean Value()"                                                                                                                                  
## [202] "tBodyAccMag- Standard Deviation()"                                                                                                                         
## [203] "tBodyAccMag- Median absolute deviation()"                                                                                                                  
## [204] "tBodyAccMag-Largest Value in array()"                                                                                                                      
## [205] "tBodyAccMag-Signal magnitude areallest Value in array()"                                                                                                   
## [206] "tBodyAccMag-Signal magnitude area()"                                                                                                                       
## [207] "tBodyAccMag-Energy measure. Sum of he squares divided by the number of values.()"                                                                          
## [208] "tBodyAccMag-Interquartile range()"                                                                                                                         
## [209] "tBodyAccMag-Signal entropoy()"                                                                                                                             
## [210] "tBodyAccMag-Autoregresion coefficients with Burg order equal to 4()1"                                                                                      
## [211] "tBodyAccMag-Autoregresion coefficients with Burg order equal to 4()2"                                                                                      
## [212] "tBodyAccMag-Autoregresion coefficients with Burg order equal to 4()3"                                                                                      
## [213] "tBodyAccMag-Autoregresion coefficients with Burg order equal to 4()4"                                                                                      
## [214] "tGravityAccMag-Mean Value()"                                                                                                                               
## [215] "tGravityAccMag- Standard Deviation()"                                                                                                                      
## [216] "tGravityAccMag- Median absolute deviation()"                                                                                                               
## [217] "tGravityAccMag-Largest Value in array()"                                                                                                                   
## [218] "tGravityAccMag-Signal magnitude areallest Value in array()"                                                                                                
## [219] "tGravityAccMag-Signal magnitude area()"                                                                                                                    
## [220] "tGravityAccMag-Energy measure. Sum of he squares divided by the number of values.()"                                                                       
## [221] "tGravityAccMag-Interquartile range()"                                                                                                                      
## [222] "tGravityAccMag-Signal entropoy()"                                                                                                                          
## [223] "tGravityAccMag-Autoregresion coefficients with Burg order equal to 4()1"                                                                                   
## [224] "tGravityAccMag-Autoregresion coefficients with Burg order equal to 4()2"                                                                                   
## [225] "tGravityAccMag-Autoregresion coefficients with Burg order equal to 4()3"                                                                                   
## [226] "tGravityAccMag-Autoregresion coefficients with Burg order equal to 4()4"                                                                                   
## [227] "tBodyAccJerkMag-Mean Value()"                                                                                                                              
## [228] "tBodyAccJerkMag- Standard Deviation()"                                                                                                                     
## [229] "tBodyAccJerkMag- Median absolute deviation()"                                                                                                              
## [230] "tBodyAccJerkMag-Largest Value in array()"                                                                                                                  
## [231] "tBodyAccJerkMag-Signal magnitude areallest Value in array()"                                                                                               
## [232] "tBodyAccJerkMag-Signal magnitude area()"                                                                                                                   
## [233] "tBodyAccJerkMag-Energy measure. Sum of he squares divided by the number of values.()"                                                                      
## [234] "tBodyAccJerkMag-Interquartile range()"                                                                                                                     
## [235] "tBodyAccJerkMag-Signal entropoy()"                                                                                                                         
## [236] "tBodyAccJerkMag-Autoregresion coefficients with Burg order equal to 4()1"                                                                                  
## [237] "tBodyAccJerkMag-Autoregresion coefficients with Burg order equal to 4()2"                                                                                  
## [238] "tBodyAccJerkMag-Autoregresion coefficients with Burg order equal to 4()3"                                                                                  
## [239] "tBodyAccJerkMag-Autoregresion coefficients with Burg order equal to 4()4"                                                                                  
## [240] "tBodyGyroMag-Mean Value()"                                                                                                                                 
## [241] "tBodyGyroMag- Standard Deviation()"                                                                                                                        
## [242] "tBodyGyroMag- Median absolute deviation()"                                                                                                                 
## [243] "tBodyGyroMag-Largest Value in array()"                                                                                                                     
## [244] "tBodyGyroMag-Signal magnitude areallest Value in array()"                                                                                                  
## [245] "tBodyGyroMag-Signal magnitude area()"                                                                                                                      
## [246] "tBodyGyroMag-Energy measure. Sum of he squares divided by the number of values.()"                                                                         
## [247] "tBodyGyroMag-Interquartile range()"                                                                                                                        
## [248] "tBodyGyroMag-Signal entropoy()"                                                                                                                            
## [249] "tBodyGyroMag-Autoregresion coefficients with Burg order equal to 4()1"                                                                                     
## [250] "tBodyGyroMag-Autoregresion coefficients with Burg order equal to 4()2"                                                                                     
## [251] "tBodyGyroMag-Autoregresion coefficients with Burg order equal to 4()3"                                                                                     
## [252] "tBodyGyroMag-Autoregresion coefficients with Burg order equal to 4()4"                                                                                     
## [253] "tBodyGyroJerkMag-Mean Value()"                                                                                                                             
## [254] "tBodyGyroJerkMag- Standard Deviation()"                                                                                                                    
## [255] "tBodyGyroJerkMag- Median absolute deviation()"                                                                                                             
## [256] "tBodyGyroJerkMag-Largest Value in array()"                                                                                                                 
## [257] "tBodyGyroJerkMag-Signal magnitude areallest Value in array()"                                                                                              
## [258] "tBodyGyroJerkMag-Signal magnitude area()"                                                                                                                  
## [259] "tBodyGyroJerkMag-Energy measure. Sum of he squares divided by the number of values.()"                                                                     
## [260] "tBodyGyroJerkMag-Interquartile range()"                                                                                                                    
## [261] "tBodyGyroJerkMag-Signal entropoy()"                                                                                                                        
## [262] "tBodyGyroJerkMag-Autoregresion coefficients with Burg order equal to 4()1"                                                                                 
## [263] "tBodyGyroJerkMag-Autoregresion coefficients with Burg order equal to 4()2"                                                                                 
## [264] "tBodyGyroJerkMag-Autoregresion coefficients with Burg order equal to 4()3"                                                                                 
## [265] "tBodyGyroJerkMag-Autoregresion coefficients with Burg order equal to 4()4"                                                                                 
## [266] "fBodyAcc-Mean Value()-X"                                                                                                                                   
## [267] "fBodyAcc-Mean Value()-Y"                                                                                                                                   
## [268] "fBodyAcc-Mean Value()-Z"                                                                                                                                   
## [269] "fBodyAcc- Standard Deviation()-X"                                                                                                                          
## [270] "fBodyAcc- Standard Deviation()-Y"                                                                                                                          
## [271] "fBodyAcc- Standard Deviation()-Z"                                                                                                                          
## [272] "fBodyAcc- Median absolute deviation()-X"                                                                                                                   
## [273] "fBodyAcc- Median absolute deviation()-Y"                                                                                                                   
## [274] "fBodyAcc- Median absolute deviation()-Z"                                                                                                                   
## [275] "fBodyAcc-Largest Value in array()-X"                                                                                                                       
## [276] "fBodyAcc-Largest Value in array()-Y"                                                                                                                       
## [277] "fBodyAcc-Largest Value in array()-Z"                                                                                                                       
## [278] "fBodyAcc-Signal magnitude areallest Value in array()-X"                                                                                                    
## [279] "fBodyAcc-Signal magnitude areallest Value in array()-Y"                                                                                                    
## [280] "fBodyAcc-Signal magnitude areallest Value in array()-Z"                                                                                                    
## [281] "fBodyAcc-Signal magnitude area()"                                                                                                                          
## [282] "fBodyAcc-Energy measure. Sum of he squares divided by the number of values.()-X"                                                                           
## [283] "fBodyAcc-Energy measure. Sum of he squares divided by the number of values.()-Y"                                                                           
## [284] "fBodyAcc-Energy measure. Sum of he squares divided by the number of values.()-Z"                                                                           
## [285] "fBodyAcc-Interquartile range()-X"                                                                                                                          
## [286] "fBodyAcc-Interquartile range()-Y"                                                                                                                          
## [287] "fBodyAcc-Interquartile range()-Z"                                                                                                                          
## [288] "fBodyAcc-Signal entropoy()-X"                                                                                                                              
## [289] "fBodyAcc-Signal entropoy()-Y"                                                                                                                              
## [290] "fBodyAcc-Signal entropoy()-Z"                                                                                                                              
## [291] "fBodyAcc-Largest Value in arrayInds-X"                                                                                                                     
## [292] "fBodyAcc-Largest Value in arrayInds-Y"                                                                                                                     
## [293] "fBodyAcc-Largest Value in arrayInds-Z"                                                                                                                     
## [294] "fBodyAcc-Mean ValueFreq()-X"                                                                                                                               
## [295] "fBodyAcc-Mean ValueFreq()-Y"                                                                                                                               
## [296] "fBodyAcc-Mean ValueFreq()-Z"                                                                                                                               
## [297] "fBodyAcc-Skewness of the frequency domain signal()-X"                                                                                                      
## [298] "fBodyAcc-Kurtosis of the frequency domain signal()-X"                                                                                                      
## [299] "fBodyAcc-Skewness of the frequency domain signal()-Y"                                                                                                      
## [300] "fBodyAcc-Kurtosis of the frequency domain signal()-Y"                                                                                                      
## [301] "fBodyAcc-Skewness of the frequency domain signal()-Z"                                                                                                      
## [302] "fBodyAcc-Kurtosis of the frequency domain signal()-Z"                                                                                                      
## [303] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,8"      
## [304] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-9,16"     
## [305] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-17,24"    
## [306] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-25,32"    
## [307] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-33,40"    
## [308] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-41,48"    
## [309] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-49,56"    
## [310] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-57,64"    
## [311] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,16"     
## [312] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-17,32"    
## [313] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-33,48"    
## [314] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-49,64"    
## [315] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,24"     
## [316] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-25,48"    
## [317] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,8"      
## [318] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-9,16"     
## [319] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-17,24"    
## [320] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-25,32"    
## [321] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-33,40"    
## [322] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-41,48"    
## [323] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-49,56"    
## [324] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-57,64"    
## [325] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,16"     
## [326] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-17,32"    
## [327] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-33,48"    
## [328] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-49,64"    
## [329] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,24"     
## [330] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-25,48"    
## [331] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,8"      
## [332] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-9,16"     
## [333] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-17,24"    
## [334] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-25,32"    
## [335] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-33,40"    
## [336] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-41,48"    
## [337] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-49,56"    
## [338] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-57,64"    
## [339] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,16"     
## [340] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-17,32"    
## [341] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-33,48"    
## [342] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-49,64"    
## [343] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,24"     
## [344] "fBodyAcc-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-25,48"    
## [345] "fBodyAccJerk-Mean Value()-X"                                                                                                                               
## [346] "fBodyAccJerk-Mean Value()-Y"                                                                                                                               
## [347] "fBodyAccJerk-Mean Value()-Z"                                                                                                                               
## [348] "fBodyAccJerk- Standard Deviation()-X"                                                                                                                      
## [349] "fBodyAccJerk- Standard Deviation()-Y"                                                                                                                      
## [350] "fBodyAccJerk- Standard Deviation()-Z"                                                                                                                      
## [351] "fBodyAccJerk- Median absolute deviation()-X"                                                                                                               
## [352] "fBodyAccJerk- Median absolute deviation()-Y"                                                                                                               
## [353] "fBodyAccJerk- Median absolute deviation()-Z"                                                                                                               
## [354] "fBodyAccJerk-Largest Value in array()-X"                                                                                                                   
## [355] "fBodyAccJerk-Largest Value in array()-Y"                                                                                                                   
## [356] "fBodyAccJerk-Largest Value in array()-Z"                                                                                                                   
## [357] "fBodyAccJerk-Signal magnitude areallest Value in array()-X"                                                                                                
## [358] "fBodyAccJerk-Signal magnitude areallest Value in array()-Y"                                                                                                
## [359] "fBodyAccJerk-Signal magnitude areallest Value in array()-Z"                                                                                                
## [360] "fBodyAccJerk-Signal magnitude area()"                                                                                                                      
## [361] "fBodyAccJerk-Energy measure. Sum of he squares divided by the number of values.()-X"                                                                       
## [362] "fBodyAccJerk-Energy measure. Sum of he squares divided by the number of values.()-Y"                                                                       
## [363] "fBodyAccJerk-Energy measure. Sum of he squares divided by the number of values.()-Z"                                                                       
## [364] "fBodyAccJerk-Interquartile range()-X"                                                                                                                      
## [365] "fBodyAccJerk-Interquartile range()-Y"                                                                                                                      
## [366] "fBodyAccJerk-Interquartile range()-Z"                                                                                                                      
## [367] "fBodyAccJerk-Signal entropoy()-X"                                                                                                                          
## [368] "fBodyAccJerk-Signal entropoy()-Y"                                                                                                                          
## [369] "fBodyAccJerk-Signal entropoy()-Z"                                                                                                                          
## [370] "fBodyAccJerk-Largest Value in arrayInds-X"                                                                                                                 
## [371] "fBodyAccJerk-Largest Value in arrayInds-Y"                                                                                                                 
## [372] "fBodyAccJerk-Largest Value in arrayInds-Z"                                                                                                                 
## [373] "fBodyAccJerk-Mean ValueFreq()-X"                                                                                                                           
## [374] "fBodyAccJerk-Mean ValueFreq()-Y"                                                                                                                           
## [375] "fBodyAccJerk-Mean ValueFreq()-Z"                                                                                                                           
## [376] "fBodyAccJerk-Skewness of the frequency domain signal()-X"                                                                                                  
## [377] "fBodyAccJerk-Kurtosis of the frequency domain signal()-X"                                                                                                  
## [378] "fBodyAccJerk-Skewness of the frequency domain signal()-Y"                                                                                                  
## [379] "fBodyAccJerk-Kurtosis of the frequency domain signal()-Y"                                                                                                  
## [380] "fBodyAccJerk-Skewness of the frequency domain signal()-Z"                                                                                                  
## [381] "fBodyAccJerk-Kurtosis of the frequency domain signal()-Z"                                                                                                  
## [382] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,8"  
## [383] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-9,16" 
## [384] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-17,24"
## [385] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-25,32"
## [386] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-33,40"
## [387] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-41,48"
## [388] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-49,56"
## [389] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-57,64"
## [390] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,16" 
## [391] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-17,32"
## [392] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-33,48"
## [393] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-49,64"
## [394] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,24" 
## [395] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-25,48"
## [396] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,8"  
## [397] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-9,16" 
## [398] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-17,24"
## [399] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-25,32"
## [400] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-33,40"
## [401] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-41,48"
## [402] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-49,56"
## [403] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-57,64"
## [404] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,16" 
## [405] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-17,32"
## [406] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-33,48"
## [407] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-49,64"
## [408] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,24" 
## [409] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-25,48"
## [410] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,8"  
## [411] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-9,16" 
## [412] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-17,24"
## [413] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-25,32"
## [414] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-33,40"
## [415] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-41,48"
## [416] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-49,56"
## [417] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-57,64"
## [418] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,16" 
## [419] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-17,32"
## [420] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-33,48"
## [421] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-49,64"
## [422] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,24" 
## [423] "fBodyAccJerk-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-25,48"
## [424] "fBodyGyro-Mean Value()-X"                                                                                                                                  
## [425] "fBodyGyro-Mean Value()-Y"                                                                                                                                  
## [426] "fBodyGyro-Mean Value()-Z"                                                                                                                                  
## [427] "fBodyGyro- Standard Deviation()-X"                                                                                                                         
## [428] "fBodyGyro- Standard Deviation()-Y"                                                                                                                         
## [429] "fBodyGyro- Standard Deviation()-Z"                                                                                                                         
## [430] "fBodyGyro- Median absolute deviation()-X"                                                                                                                  
## [431] "fBodyGyro- Median absolute deviation()-Y"                                                                                                                  
## [432] "fBodyGyro- Median absolute deviation()-Z"                                                                                                                  
## [433] "fBodyGyro-Largest Value in array()-X"                                                                                                                      
## [434] "fBodyGyro-Largest Value in array()-Y"                                                                                                                      
## [435] "fBodyGyro-Largest Value in array()-Z"                                                                                                                      
## [436] "fBodyGyro-Signal magnitude areallest Value in array()-X"                                                                                                   
## [437] "fBodyGyro-Signal magnitude areallest Value in array()-Y"                                                                                                   
## [438] "fBodyGyro-Signal magnitude areallest Value in array()-Z"                                                                                                   
## [439] "fBodyGyro-Signal magnitude area()"                                                                                                                         
## [440] "fBodyGyro-Energy measure. Sum of he squares divided by the number of values.()-X"                                                                          
## [441] "fBodyGyro-Energy measure. Sum of he squares divided by the number of values.()-Y"                                                                          
## [442] "fBodyGyro-Energy measure. Sum of he squares divided by the number of values.()-Z"                                                                          
## [443] "fBodyGyro-Interquartile range()-X"                                                                                                                         
## [444] "fBodyGyro-Interquartile range()-Y"                                                                                                                         
## [445] "fBodyGyro-Interquartile range()-Z"                                                                                                                         
## [446] "fBodyGyro-Signal entropoy()-X"                                                                                                                             
## [447] "fBodyGyro-Signal entropoy()-Y"                                                                                                                             
## [448] "fBodyGyro-Signal entropoy()-Z"                                                                                                                             
## [449] "fBodyGyro-Largest Value in arrayInds-X"                                                                                                                    
## [450] "fBodyGyro-Largest Value in arrayInds-Y"                                                                                                                    
## [451] "fBodyGyro-Largest Value in arrayInds-Z"                                                                                                                    
## [452] "fBodyGyro-Mean ValueFreq()-X"                                                                                                                              
## [453] "fBodyGyro-Mean ValueFreq()-Y"                                                                                                                              
## [454] "fBodyGyro-Mean ValueFreq()-Z"                                                                                                                              
## [455] "fBodyGyro-Skewness of the frequency domain signal()-X"                                                                                                     
## [456] "fBodyGyro-Kurtosis of the frequency domain signal()-X"                                                                                                     
## [457] "fBodyGyro-Skewness of the frequency domain signal()-Y"                                                                                                     
## [458] "fBodyGyro-Kurtosis of the frequency domain signal()-Y"                                                                                                     
## [459] "fBodyGyro-Skewness of the frequency domain signal()-Z"                                                                                                     
## [460] "fBodyGyro-Kurtosis of the frequency domain signal()-Z"                                                                                                     
## [461] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,8"     
## [462] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-9,16"    
## [463] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-17,24"   
## [464] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-25,32"   
## [465] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-33,40"   
## [466] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-41,48"   
## [467] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-49,56"   
## [468] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-57,64"   
## [469] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,16"    
## [470] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-17,32"   
## [471] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-33,48"   
## [472] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-49,64"   
## [473] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,24"    
## [474] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-25,48"   
## [475] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,8"     
## [476] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-9,16"    
## [477] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-17,24"   
## [478] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-25,32"   
## [479] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-33,40"   
## [480] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-41,48"   
## [481] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-49,56"   
## [482] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-57,64"   
## [483] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,16"    
## [484] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-17,32"   
## [485] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-33,48"   
## [486] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-49,64"   
## [487] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,24"    
## [488] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-25,48"   
## [489] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,8"     
## [490] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-9,16"    
## [491] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-17,24"   
## [492] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-25,32"   
## [493] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-33,40"   
## [494] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-41,48"   
## [495] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-49,56"   
## [496] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-57,64"   
## [497] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,16"    
## [498] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-17,32"   
## [499] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-33,48"   
## [500] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-49,64"   
## [501] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-1,24"    
## [502] "fBodyGyro-Energy of frequency interval within the 64 bins of the FFT of each window measure. Sum of he squares divided by the number of values.()-25,48"   
## [503] "fBodyAccMag-Mean Value()"                                                                                                                                  
## [504] "fBodyAccMag- Standard Deviation()"                                                                                                                         
## [505] "fBodyAccMag- Median absolute deviation()"                                                                                                                  
## [506] "fBodyAccMag-Largest Value in array()"                                                                                                                      
## [507] "fBodyAccMag-Signal magnitude areallest Value in array()"                                                                                                   
## [508] "fBodyAccMag-Signal magnitude area()"                                                                                                                       
## [509] "fBodyAccMag-Energy measure. Sum of he squares divided by the number of values.()"                                                                          
## [510] "fBodyAccMag-Interquartile range()"                                                                                                                         
## [511] "fBodyAccMag-Signal entropoy()"                                                                                                                             
## [512] "fBodyAccMag-Largest Value in arrayInds"                                                                                                                    
## [513] "fBodyAccMag-Mean ValueFreq()"                                                                                                                              
## [514] "fBodyAccMag-Skewness of the frequency domain signal()"                                                                                                     
## [515] "fBodyAccMag-Kurtosis of the frequency domain signal()"                                                                                                     
## [516] "fBodyBodyAccJerkMag-Mean Value()"                                                                                                                          
## [517] "fBodyBodyAccJerkMag- Standard Deviation()"                                                                                                                 
## [518] "fBodyBodyAccJerkMag- Median absolute deviation()"                                                                                                          
## [519] "fBodyBodyAccJerkMag-Largest Value in array()"                                                                                                              
## [520] "fBodyBodyAccJerkMag-Signal magnitude areallest Value in array()"                                                                                           
## [521] "fBodyBodyAccJerkMag-Signal magnitude area()"                                                                                                               
## [522] "fBodyBodyAccJerkMag-Energy measure. Sum of he squares divided by the number of values.()"                                                                  
## [523] "fBodyBodyAccJerkMag-Interquartile range()"                                                                                                                 
## [524] "fBodyBodyAccJerkMag-Signal entropoy()"                                                                                                                     
## [525] "fBodyBodyAccJerkMag-Largest Value in arrayInds"                                                                                                            
## [526] "fBodyBodyAccJerkMag-Mean ValueFreq()"                                                                                                                      
## [527] "fBodyBodyAccJerkMag-Skewness of the frequency domain signal()"                                                                                             
## [528] "fBodyBodyAccJerkMag-Kurtosis of the frequency domain signal()"                                                                                             
## [529] "fBodyBodyGyroMag-Mean Value()"                                                                                                                             
## [530] "fBodyBodyGyroMag- Standard Deviation()"                                                                                                                    
## [531] "fBodyBodyGyroMag- Median absolute deviation()"                                                                                                             
## [532] "fBodyBodyGyroMag-Largest Value in array()"                                                                                                                 
## [533] "fBodyBodyGyroMag-Signal magnitude areallest Value in array()"                                                                                              
## [534] "fBodyBodyGyroMag-Signal magnitude area()"                                                                                                                  
## [535] "fBodyBodyGyroMag-Energy measure. Sum of he squares divided by the number of values.()"                                                                     
## [536] "fBodyBodyGyroMag-Interquartile range()"                                                                                                                    
## [537] "fBodyBodyGyroMag-Signal entropoy()"                                                                                                                        
## [538] "fBodyBodyGyroMag-Largest Value in arrayInds"                                                                                                               
## [539] "fBodyBodyGyroMag-Mean ValueFreq()"                                                                                                                         
## [540] "fBodyBodyGyroMag-Skewness of the frequency domain signal()"                                                                                                
## [541] "fBodyBodyGyroMag-Kurtosis of the frequency domain signal()"                                                                                                
## [542] "fBodyBodyGyroJerkMag-Mean Value()"                                                                                                                         
## [543] "fBodyBodyGyroJerkMag- Standard Deviation()"                                                                                                                
## [544] "fBodyBodyGyroJerkMag- Median absolute deviation()"                                                                                                         
## [545] "fBodyBodyGyroJerkMag-Largest Value in array()"                                                                                                             
## [546] "fBodyBodyGyroJerkMag-Signal magnitude areallest Value in array()"                                                                                          
## [547] "fBodyBodyGyroJerkMag-Signal magnitude area()"                                                                                                              
## [548] "fBodyBodyGyroJerkMag-Energy measure. Sum of he squares divided by the number of values.()"                                                                 
## [549] "fBodyBodyGyroJerkMag-Interquartile range()"                                                                                                                
## [550] "fBodyBodyGyroJerkMag-Signal entropoy()"                                                                                                                    
## [551] "fBodyBodyGyroJerkMag-Largest Value in arrayInds"                                                                                                           
## [552] "fBodyBodyGyroJerkMag-Mean ValueFreq()"                                                                                                                     
## [553] "fBodyBodyGyroJerkMag-Skewness of the frequency domain signal()"                                                                                            
## [554] "fBodyBodyGyroJerkMag-Kurtosis of the frequency domain signal()"                                                                                            
## [555] "Angle between two vectors(tBodyAccMean Value,gravity)"                                                                                                     
## [556] "Angle between two vectors(tBodyAccJerkMean Value),gravityMean Value)"                                                                                      
## [557] "Angle between two vectors(tBodyGyroMean Value,gravityMean Value)"                                                                                          
## [558] "Angle between two vectors(tBodyGyroJerkMean Value,gravityMean Value)"                                                                                      
## [559] "Angle between two vectors(X,gravityMean Value)"                                                                                                            
## [560] "Angle between two vectors(Y,gravityMean Value)"                                                                                                            
## [561] "Angle between two vectors(Z,gravityMean Value)"                                                                                                            
## [562] "user"
```

```r
colnames(single.ms)
```

```
##  [1] "tBodyAcc-Mean Value()-X"                                             
##  [2] "tBodyAcc-Mean Value()-Y"                                             
##  [3] "tBodyAcc-Mean Value()-Z"                                             
##  [4] "tBodyAcc- Standard Deviation()-X"                                    
##  [5] "tBodyAcc- Standard Deviation()-Y"                                    
##  [6] "tBodyAcc- Standard Deviation()-Z"                                    
##  [7] "tGravityAcc-Mean Value()-X"                                          
##  [8] "tGravityAcc-Mean Value()-Y"                                          
##  [9] "tGravityAcc-Mean Value()-Z"                                          
## [10] "tGravityAcc- Standard Deviation()-X"                                 
## [11] "tGravityAcc- Standard Deviation()-Y"                                 
## [12] "tGravityAcc- Standard Deviation()-Z"                                 
## [13] "tBodyAccJerk-Mean Value()-X"                                         
## [14] "tBodyAccJerk-Mean Value()-Y"                                         
## [15] "tBodyAccJerk-Mean Value()-Z"                                         
## [16] "tBodyAccJerk- Standard Deviation()-X"                                
## [17] "tBodyAccJerk- Standard Deviation()-Y"                                
## [18] "tBodyAccJerk- Standard Deviation()-Z"                                
## [19] "tBodyGyro-Mean Value()-X"                                            
## [20] "tBodyGyro-Mean Value()-Y"                                            
## [21] "tBodyGyro-Mean Value()-Z"                                            
## [22] "tBodyGyro- Standard Deviation()-X"                                   
## [23] "tBodyGyro- Standard Deviation()-Y"                                   
## [24] "tBodyGyro- Standard Deviation()-Z"                                   
## [25] "tBodyGyroJerk-Mean Value()-X"                                        
## [26] "tBodyGyroJerk-Mean Value()-Y"                                        
## [27] "tBodyGyroJerk-Mean Value()-Z"                                        
## [28] "tBodyGyroJerk- Standard Deviation()-X"                               
## [29] "tBodyGyroJerk- Standard Deviation()-Y"                               
## [30] "tBodyGyroJerk- Standard Deviation()-Z"                               
## [31] "tBodyAccMag-Mean Value()"                                            
## [32] "tBodyAccMag- Standard Deviation()"                                   
## [33] "tGravityAccMag-Mean Value()"                                         
## [34] "tGravityAccMag- Standard Deviation()"                                
## [35] "tBodyAccJerkMag-Mean Value()"                                        
## [36] "tBodyAccJerkMag- Standard Deviation()"                               
## [37] "tBodyGyroMag-Mean Value()"                                           
## [38] "tBodyGyroMag- Standard Deviation()"                                  
## [39] "tBodyGyroJerkMag-Mean Value()"                                       
## [40] "tBodyGyroJerkMag- Standard Deviation()"                              
## [41] "fBodyAcc-Mean Value()-X"                                             
## [42] "fBodyAcc-Mean Value()-Y"                                             
## [43] "fBodyAcc-Mean Value()-Z"                                             
## [44] "fBodyAcc- Standard Deviation()-X"                                    
## [45] "fBodyAcc- Standard Deviation()-Y"                                    
## [46] "fBodyAcc- Standard Deviation()-Z"                                    
## [47] "fBodyAcc-Mean ValueFreq()-X"                                         
## [48] "fBodyAcc-Mean ValueFreq()-Y"                                         
## [49] "fBodyAcc-Mean ValueFreq()-Z"                                         
## [50] "fBodyAccJerk-Mean Value()-X"                                         
## [51] "fBodyAccJerk-Mean Value()-Y"                                         
## [52] "fBodyAccJerk-Mean Value()-Z"                                         
## [53] "fBodyAccJerk- Standard Deviation()-X"                                
## [54] "fBodyAccJerk- Standard Deviation()-Y"                                
## [55] "fBodyAccJerk- Standard Deviation()-Z"                                
## [56] "fBodyAccJerk-Mean ValueFreq()-X"                                     
## [57] "fBodyAccJerk-Mean ValueFreq()-Y"                                     
## [58] "fBodyAccJerk-Mean ValueFreq()-Z"                                     
## [59] "fBodyGyro-Mean Value()-X"                                            
## [60] "fBodyGyro-Mean Value()-Y"                                            
## [61] "fBodyGyro-Mean Value()-Z"                                            
## [62] "fBodyGyro- Standard Deviation()-X"                                   
## [63] "fBodyGyro- Standard Deviation()-Y"                                   
## [64] "fBodyGyro- Standard Deviation()-Z"                                   
## [65] "fBodyGyro-Mean ValueFreq()-X"                                        
## [66] "fBodyGyro-Mean ValueFreq()-Y"                                        
## [67] "fBodyGyro-Mean ValueFreq()-Z"                                        
## [68] "fBodyAccMag-Mean Value()"                                            
## [69] "fBodyAccMag- Standard Deviation()"                                   
## [70] "fBodyAccMag-Mean ValueFreq()"                                        
## [71] "fBodyBodyAccJerkMag-Mean Value()"                                    
## [72] "fBodyBodyAccJerkMag- Standard Deviation()"                           
## [73] "fBodyBodyAccJerkMag-Mean ValueFreq()"                                
## [74] "fBodyBodyGyroMag-Mean Value()"                                       
## [75] "fBodyBodyGyroMag- Standard Deviation()"                              
## [76] "fBodyBodyGyroMag-Mean ValueFreq()"                                   
## [77] "fBodyBodyGyroJerkMag-Mean Value()"                                   
## [78] "fBodyBodyGyroJerkMag- Standard Deviation()"                          
## [79] "fBodyBodyGyroJerkMag-Mean ValueFreq()"                               
## [80] "Angle between two vectors(tBodyAccMean Value,gravity)"               
## [81] "Angle between two vectors(tBodyAccJerkMean Value),gravityMean Value)"
## [82] "Angle between two vectors(tBodyGyroMean Value,gravityMean Value)"    
## [83] "Angle between two vectors(tBodyGyroJerkMean Value,gravityMean Value)"
## [84] "Angle between two vectors(X,gravityMean Value)"                      
## [85] "Angle between two vectors(Y,gravityMean Value)"                      
## [86] "Angle between two vectors(Z,gravityMean Value)"                      
## [87] "user"
```

Create second dataset with average variables (means/sd's) by user/subject

```r
(single.tidy<-aggregate(. ~ user, data = single.ms, mean))
```

```
##   user tBodyAcc-Mean Value()-X tBodyAcc-Mean Value()-Y
## 1    1                  0.2763                -0.01791
## 2    2                  0.2623                -0.02592
## 3    3                  0.2881                -0.01631
## 4    4                  0.2731                -0.01269
## 5    5                  0.2792                -0.01615
## 6    6                  0.2686                -0.01832
##   tBodyAcc-Mean Value()-Z tBodyAcc- Standard Deviation()-X
## 1                 -0.1089                          -0.3146
## 2                 -0.1205                          -0.2380
## 3                 -0.1058                           0.1008
## 4                 -0.1055                          -0.9834
## 5                 -0.1066                          -0.9844
## 6                 -0.1074                          -0.9609
##   tBodyAcc- Standard Deviation()-Y tBodyAcc- Standard Deviation()-Z
## 1                         -0.02358                          -0.2739
## 2                         -0.01603                          -0.1754
## 3                          0.05955                          -0.1908
## 4                         -0.93488                          -0.9390
## 5                         -0.93251                          -0.9399
## 6                         -0.94351                          -0.9481
##   tGravityAcc-Mean Value()-X tGravityAcc-Mean Value()-Y
## 1                     0.9350                    -0.1967
## 2                     0.8750                    -0.2814
## 3                     0.9265                    -0.1685
## 4                     0.8797                     0.1087
## 5                     0.9415                    -0.1842
## 6                    -0.3750                     0.6223
##   tGravityAcc-Mean Value()-Z tGravityAcc- Standard Deviation()-X
## 1                   -0.05383                             -0.9776
## 2                   -0.14080                             -0.9482
## 3                   -0.04797                             -0.9497
## 4                    0.15377                             -0.9797
## 5                   -0.01405                             -0.9880
## 6                    0.55561                             -0.9433
##   tGravityAcc- Standard Deviation()-Y tGravityAcc- Standard Deviation()-Z
## 1                             -0.9669                             -0.9546
## 2                             -0.9255                             -0.9019
## 3                             -0.9343                             -0.9125
## 4                             -0.9577                             -0.9474
## 5                             -0.9694                             -0.9531
## 6                             -0.9632                             -0.9519
##   tBodyAccJerk-Mean Value()-X tBodyAccJerk-Mean Value()-Y
## 1                     0.07672                   0.0115062
## 2                     0.07673                   0.0087589
## 3                     0.08923                   0.0007467
## 4                     0.07588                   0.0050469
## 5                     0.07503                   0.0088053
## 6                     0.08185                   0.0111724
##   tBodyAccJerk-Mean Value()-Z tBodyAccJerk- Standard Deviation()-X
## 1                   -0.002319                             -0.26729
## 2                   -0.006010                             -0.36086
## 3                   -0.008729                             -0.03388
## 4                   -0.002487                             -0.98500
## 5                   -0.004582                             -0.97997
## 6                   -0.004860                             -0.98038
##   tBodyAccJerk- Standard Deviation()-Y
## 1                             -0.10314
## 2                             -0.33923
## 3                             -0.07367
## 4                             -0.97388
## 5                             -0.96434
## 6                             -0.97115
##   tBodyAccJerk- Standard Deviation()-Z tBodyGyro-Mean Value()-X
## 1                              -0.4791                -0.034728
## 2                              -0.6271                 0.006824
## 3                              -0.3887                -0.084035
## 4                              -0.9823                -0.038431
## 5                              -0.9795                -0.026687
## 6                              -0.9795                -0.016725
##   tBodyGyro-Mean Value()-Y tBodyGyro-Mean Value()-Z
## 1                 -0.06942                  0.08636
## 2                 -0.08852                  0.05989
## 3                 -0.05299                  0.09468
## 4                 -0.07212                  0.07778
## 5                 -0.06771                  0.08014
## 6                 -0.09341                  0.12589
##   tBodyGyro- Standard Deviation()-X tBodyGyro- Standard Deviation()-Y
## 1                           -0.4699                           -0.3479
## 2                           -0.4676                           -0.3442
## 3                           -0.3338                           -0.3396
## 4                           -0.9810                           -0.9667
## 5                           -0.9455                           -0.9613
## 6                           -0.9679                           -0.9632
##   tBodyGyro- Standard Deviation()-Z tBodyGyroJerk-Mean Value()-X
## 1                           -0.3384                     -0.09430
## 2                           -0.2371                     -0.11212
## 3                           -0.2728                     -0.07285
## 4                           -0.9580                     -0.09565
## 5                           -0.9571                     -0.09973
## 6                           -0.9635                     -0.10186
##   tBodyGyroJerk-Mean Value()-Y tBodyGyroJerk-Mean Value()-Z
## 1                     -0.04457                     -0.05401
## 2                     -0.03862                     -0.05258
## 3                     -0.05126                     -0.05470
## 4                     -0.04078                     -0.05076
## 5                     -0.04232                     -0.05210
## 6                     -0.03820                     -0.06385
##   tBodyGyroJerk- Standard Deviation()-X
## 1                               -0.3762
## 2                               -0.5531
## 3                               -0.3827
## 4                               -0.9857
## 5                               -0.9670
## 6                               -0.9761
##   tBodyGyroJerk- Standard Deviation()-Y
## 1                               -0.5126
## 2                               -0.6673
## 3                               -0.4659
## 4                               -0.9865
## 5                               -0.9803
## 6                               -0.9805
##   tBodyGyroJerk- Standard Deviation()-Z tBodyAccMag-Mean Value()
## 1                               -0.4474                  -0.1679
## 2                               -0.5610                  -0.1002
## 3                               -0.3265                   0.1012
## 4                               -0.9838                  -0.9546
## 5                               -0.9771                  -0.9542
## 6                               -0.9848                  -0.9411
##   tBodyAccMag- Standard Deviation() tGravityAccMag-Mean Value()
## 1                           -0.3378                     -0.1679
## 2                           -0.2499                     -0.1002
## 3                            0.1165                      0.1012
## 4                           -0.9393                     -0.9546
## 5                           -0.9465                     -0.9542
## 6                           -0.9322                     -0.9411
##   tGravityAccMag- Standard Deviation() tBodyAccJerkMag-Mean Value()
## 1                              -0.3378                      -0.2415
## 2                              -0.2499                      -0.3909
## 3                               0.1165                      -0.1118
## 4                              -0.9393                      -0.9824
## 5                              -0.9465                      -0.9771
## 6                              -0.9322                      -0.9792
##   tBodyAccJerkMag- Standard Deviation() tBodyGyroMag-Mean Value()
## 1                              -0.21456                   -0.2749
## 2                              -0.38540                   -0.1783
## 3                              -0.01122                   -0.1298
## 4                              -0.97891                   -0.9467
## 5                              -0.97145                   -0.9422
## 6                              -0.97424                   -0.9384
##   tBodyGyroMag- Standard Deviation() tBodyGyroJerkMag-Mean Value()
## 1                            -0.3826                       -0.4605
## 2                            -0.3371                       -0.6080
## 3                            -0.2514                       -0.4169
## 4                            -0.9512                       -0.9879
## 5                            -0.9295                       -0.9787
## 6                            -0.9406                       -0.9827
##   tBodyGyroJerkMag- Standard Deviation() fBodyAcc-Mean Value()-X
## 1                                -0.4988                -0.29789
## 2                                -0.6668                -0.29341
## 3                                -0.4409                 0.03526
## 4                                -0.9846                -0.98309
## 5                                -0.9735                -0.98162
## 6                                -0.9768                -0.96681
##   fBodyAcc-Mean Value()-Y fBodyAcc-Mean Value()-Z
## 1                -0.04234                 -0.3418
## 2                -0.13495                 -0.3681
## 3                 0.05668                 -0.2137
## 4                -0.94792                 -0.9570
## 5                -0.94313                 -0.9574
## 6                -0.95268                 -0.9600
##   fBodyAcc- Standard Deviation()-X fBodyAcc- Standard Deviation()-Y
## 1                          -0.3228                        -0.077206
## 2                          -0.2189                        -0.021811
## 3                           0.1219                        -0.008234
## 4                          -0.9837                        -0.932533
## 5                          -0.9859                        -0.931133
## 6                          -0.9590                        -0.942461
##   fBodyAcc- Standard Deviation()-Z fBodyAcc-Mean ValueFreq()-X
## 1                          -0.2961                    -0.28686
## 2                          -0.1466                    -0.43668
## 3                          -0.2459                    -0.40002
## 4                          -0.9343                    -0.04264
## 5                          -0.9354                     0.01560
## 6                          -0.9456                    -0.25938
##   fBodyAcc-Mean ValueFreq()-Y fBodyAcc-Mean ValueFreq()-Z
## 1                   0.0518637                     0.07496
## 2                  -0.1698513                    -0.26520
## 3                   0.0006031                     0.09243
## 4                   0.0653032                     0.08030
## 5                  -0.0332741                     0.05247
## 6                   0.1430456                     0.20319
##   fBodyAccJerk-Mean Value()-X fBodyAccJerk-Mean Value()-Y
## 1                     -0.3111                     -0.1704
## 2                     -0.3899                     -0.3647
## 3                     -0.0723                     -0.1164
## 4                     -0.9852                     -0.9740
## 5                     -0.9800                     -0.9645
## 6                     -0.9802                     -0.9714
##   fBodyAccJerk-Mean Value()-Z fBodyAccJerk- Standard Deviation()-X
## 1                     -0.4510                             -0.28790
## 2                     -0.5917                             -0.38899
## 3                     -0.3332                             -0.08219
## 4                     -0.9796                             -0.98618
## 5                     -0.9762                             -0.98183
## 6                     -0.9766                             -0.98246
##   fBodyAccJerk- Standard Deviation()-Y
## 1                             -0.09087
## 2                             -0.35763
## 3                             -0.09142
## 4                             -0.97575
## 5                             -0.96683
## 6                             -0.97305
##   fBodyAccJerk- Standard Deviation()-Z fBodyAccJerk-Mean ValueFreq()-X
## 1                              -0.5063                         -0.2584
## 2                              -0.6616                         -0.3391
## 3                              -0.4436                         -0.3149
## 4                              -0.9837                          0.1850
## 5                              -0.9815                          0.2029
## 6                              -0.9810                          0.1052
##   fBodyAccJerk-Mean ValueFreq()-Y fBodyAccJerk-Mean ValueFreq()-Z
## 1                       -0.354659                       -0.240686
## 2                       -0.452501                       -0.441163
## 3                       -0.386044                       -0.237403
## 4                       -0.058311                        0.002996
## 5                       -0.131893                        0.006700
## 6                        0.004854                        0.069962
##   fBodyGyro-Mean Value()-X fBodyGyro-Mean Value()-Y
## 1                  -0.3482                  -0.3884
## 2                  -0.3942                  -0.4593
## 3                  -0.2179                  -0.3176
## 4                  -0.9773                  -0.9725
## 5                  -0.9437                  -0.9653
## 6                  -0.9629                  -0.9676
##   fBodyGyro-Mean Value()-Z fBodyGyro- Standard Deviation()-X
## 1                  -0.3104                           -0.5104
## 2                  -0.2969                           -0.4953
## 3                  -0.1656                           -0.3751
## 4                  -0.9610                           -0.9823
## 5                  -0.9584                           -0.9470
## 6                  -0.9642                           -0.9697
##   fBodyGyro- Standard Deviation()-Y fBodyGyro- Standard Deviation()-Z
## 1                           -0.3320                           -0.4106
## 2                           -0.2932                           -0.2920
## 3                           -0.3619                           -0.3804
## 4                           -0.9640                           -0.9610
## 5                           -0.9595                           -0.9607
## 6                           -0.9614                           -0.9667
##   fBodyGyro-Mean ValueFreq()-X fBodyGyro-Mean ValueFreq()-Y
## 1                     -0.06774                     -0.09845
## 2                     -0.21284                     -0.31952
## 3                     -0.17002                     -0.04409
## 4                      0.06259                     -0.21803
## 5                     -0.22749                     -0.21601
## 6                     -0.01746                     -0.13934
##   fBodyGyro-Mean ValueFreq()-Z fBodyAccMag-Mean Value()
## 1                     -0.07218                  -0.2756
## 2                     -0.26034                  -0.2620
## 3                     -0.01879                   0.1428
## 4                     -0.01270                  -0.9524
## 5                     -0.09143                  -0.9559
## 6                      0.11328                  -0.9477
##   fBodyAccMag- Standard Deviation() fBodyAccMag-Mean ValueFreq()
## 1                          -0.48000                      0.18442
## 2                          -0.36175                     -0.05322
## 3                          -0.07543                      0.02504
## 4                          -0.94200                      0.11411
## 5                          -0.94960                      0.04849
## 6                          -0.93492                      0.11623
##   fBodyBodyAccJerkMag-Mean Value()
## 1                        -0.214654
## 2                        -0.353962
## 3                         0.004762
## 4                        -0.978684
## 5                        -0.971090
## 6                        -0.974300
##   fBodyBodyAccJerkMag- Standard Deviation()
## 1                                  -0.22162
## 2                                  -0.43421
## 3                                  -0.04227
## 4                                  -0.97815
## 5                                  -0.97095
## 6                                  -0.97318
##   fBodyBodyAccJerkMag-Mean ValueFreq() fBodyBodyGyroMag-Mean Value()
## 1                              0.07731                       -0.4092
## 2                              0.06287                       -0.4498
## 3                              0.02007                       -0.2895
## 4                              0.28146                       -0.9643
## 5                              0.25127                       -0.9479
## 6                              0.28112                       -0.9549
##   fBodyBodyGyroMag- Standard Deviation() fBodyBodyGyroMag-Mean ValueFreq()
## 1                                -0.4738                           0.16320
## 2                                -0.3814                          -0.16871
## 3                                -0.3612                           0.06717
## 4                                -0.9516                          -0.07644
## 5                                -0.9306                          -0.18371
## 6                                -0.9421                          -0.02937
##   fBodyBodyGyroJerkMag-Mean Value()
## 1                           -0.5155
## 2                           -0.6587
## 3                           -0.4380
## 4                           -0.9853
## 5                           -0.9749
## 6                           -0.9780
##   fBodyBodyGyroJerkMag- Standard Deviation()
## 1                                    -0.5144
## 2                                    -0.7031
## 3                                    -0.4864
## 4                                    -0.9845
## 5                                    -0.9735
## 6                                    -0.9766
##   fBodyBodyGyroJerkMag-Mean ValueFreq()
## 1                               0.13081
## 2                               0.09411
## 3                               0.09576
## 4                               0.17774
## 5                               0.08487
## 6                               0.16573
##   Angle between two vectors(tBodyAccMean Value,gravity)
## 1                                              0.014918
## 2                                              0.035371
## 3                                             -0.039692
## 4                                              0.012034
## 5                                              0.006991
## 6                                              0.010366
##   Angle between two vectors(tBodyAccJerkMean Value),gravityMean Value)
## 1                                                            -0.007011
## 2                                                             0.006652
## 3                                                            -0.018665
## 4                                                             0.002458
## 5                                                             0.010397
## 6                                                             0.016013
##   Angle between two vectors(tBodyGyroMean Value,gravityMean Value)
## 1                                                         0.011332
## 2                                                        -0.129903
## 3                                                         0.203588
## 4                                                         0.013413
## 5                                                         0.004614
## 6                                                         0.022788
##   Angle between two vectors(tBodyGyroJerkMean Value,gravityMean Value)
## 1                                                            -0.019443
## 2                                                             0.036432
## 3                                                            -0.076029
## 4                                                            -0.033260
## 5                                                             0.015957
## 6                                                             0.009191
##   Angle between two vectors(X,gravityMean Value)
## 1                                        -0.7619
## 2                                        -0.6380
## 3                                        -0.7809
## 4                                        -0.7060
## 5                                        -0.7741
## 6                                         0.5203
##   Angle between two vectors(Y,gravityMean Value)
## 1                                        0.21860
## 2                                        0.27864
## 3                                        0.20019
## 4                                        0.00614
## 5                                        0.20982
## 6                                       -0.43594
##   Angle between two vectors(Z,gravityMean Value)
## 1                                        0.05977
## 2                                        0.12279
## 3                                        0.05587
## 4                                       -0.08953
## 5                                        0.03174
## 6                                       -0.42775
```

```r
dim(single.tidy)
```

```
## [1]  6 87
```

write single.tidy to R report table and .csv files

```r
write.table(single.tidy, "tidy_data.dat", col.names = TRUE, row.names=TRUE)
write.csv(single.tidy, "tidy.csv", row.names=FALSE)
```
