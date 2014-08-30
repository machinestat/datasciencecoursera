---
title: "RMarkDownFile"
author: "Shu Guo"
date: "Saturday, August 30, 2014"
output: html_document
---

## Reading local flat files


```r
#if(!file.exists("data")){
#  dir.create("data")
#}
#fileUrl <- "https://data.baltimorecity.gov/api/views/dz54-2aru/rows.csv?accessType=DOWNLOAD"
#download.file(fileUrl, destfile = "data/camera.csv")
#dateDownloaded <- date()
# Get error in reading data
#cameraData <- read.table("data/camera.csv")
# Error in scan(file, what, nmax, sep, dec, quote, skip, nlines, na.strings,  : 
# line 1 did not have 13 elements
# Read data with sep command and header command
cameraData <- read.table("data/camera.csv", sep = ",",
                         header = TRUE)
head(cameraData)
```

```
##                          address direction      street  crossStreet
## 1       S CATON AVE & BENSON AVE       N/B   Caton Ave   Benson Ave
## 2       S CATON AVE & BENSON AVE       S/B   Caton Ave   Benson Ave
## 3 WILKENS AVE & PINE HEIGHTS AVE       E/B Wilkens Ave Pine Heights
## 4        THE ALAMEDA & E 33RD ST       S/B The Alameda      33rd St
## 5        E 33RD ST & THE ALAMEDA       E/B      E 33rd  The Alameda
## 6        ERDMAN AVE & N MACON ST       E/B      Erdman     Macon St
##                 intersection                      Location.1
## 1     Caton Ave & Benson Ave (39.2693779962, -76.6688185297)
## 2     Caton Ave & Benson Ave (39.2693157898, -76.6689698176)
## 3 Wilkens Ave & Pine Heights  (39.2720252302, -76.676960806)
## 4     The Alameda  & 33rd St (39.3285013141, -76.5953545714)
## 5      E 33rd  & The Alameda (39.3283410623, -76.5953594625)
## 6         Erdman  & Macon St (39.3068045671, -76.5593167803)
```

```r
# read.csv sets sep = "," and header = TRUE
```

## Read excel file

```r
# Download the file
#fileUrlexcel <- "https://data.baltimorecity.gov/api/views/dz54-2aru/rows.xlsx?accessType=DOWNLOAD"
#download.file(fileUrlexcel, destfile = "data/camera.xlsx", mode="wb")
#dateDownloadedexcel <- date()
library(xlsx)
```

```
## Loading required package: rJava
## Loading required package: xlsxjars
```

```r
cameraDataExcel <- read.xlsx("data/camera.xlsx", sheetIndex = 1,
                        header = TRUE)
head(cameraDataExcel)
```

```
##                          address direction      street  crossStreet
## 1       S CATON AVE & BENSON AVE       N/B   Caton Ave   Benson Ave
## 2       S CATON AVE & BENSON AVE       S/B   Caton Ave   Benson Ave
## 3 WILKENS AVE & PINE HEIGHTS AVE       E/B Wilkens Ave Pine Heights
## 4        THE ALAMEDA & E 33RD ST       S/B The Alameda      33rd St
## 5        E 33RD ST & THE ALAMEDA       E/B      E 33rd  The Alameda
## 6        ERDMAN AVE & N MACON ST       E/B      Erdman     Macon St
##                 intersection                      Location.1
## 1     Caton Ave & Benson Ave (39.2693779962, -76.6688185297)
## 2     Caton Ave & Benson Ave (39.2693157898, -76.6689698176)
## 3 Wilkens Ave & Pine Heights  (39.2720252302, -76.676960806)
## 4     The Alameda  & 33rd St (39.3285013141, -76.5953545714)
## 5      E 33rd  & The Alameda (39.3283410623, -76.5953594625)
## 6         Erdman  & Macon St (39.3068045671, -76.5593167803)
```

```r
# Reading specific rows and columns on the excel file.
colIndex <- 2:3
rowIndex <- 1:4
cameraDataSubset <- read.xlsx("data/camera.xlsx", sheetIndex = 1,
                    colIndex = colIndex, rowIndex = rowIndex)
cameraDataSubset
```

```
##   direction      street
## 1       N/B   Caton Ave
## 2       S/B   Caton Ave
## 3       E/B Wilkens Ave
```
read.xlsx2 is much faster then read.xlsx but for reading subsets  
of rows may be slightly unstable

The XLConnect package has more options for writing and manipulating
excel files.

## Reading XML 


```r
# Read XML file into R
library(XML)
fileUrlXML <- "http://www.w3schools.com/xml/simple.xml"
doc <- xmlTreeParse(fileUrlXML, useInternal = TRUE)
rootNode <- xmlRoot(doc)
xmlName(rootNode)
```

```
## [1] "breakfast_menu"
```

```r
names(rootNode)
```

```
##   food   food   food   food   food 
## "food" "food" "food" "food" "food"
```

```r
rootNode[[1]]
```

```
## <food>
##   <name>Belgian Waffles</name>
##   <price>$5.95</price>
##   <description>Two of our famous Belgian Waffles with plenty of real maple syrup</description>
##   <calories>650</calories>
## </food>
```

```r
rootNode[[1]][[1]]
```

```
## <name>Belgian Waffles</name>
```

```r
# Programatically extract parts of the file
xmlSApply(rootNode, xmlValue)
```

```
##                                                                                                                     food 
##                               "Belgian Waffles$5.95Two of our famous Belgian Waffles with plenty of real maple syrup650" 
##                                                                                                                     food 
##                    "Strawberry Belgian Waffles$7.95Light Belgian waffles covered with strawberries and whipped cream900" 
##                                                                                                                     food 
## "Berry-Berry Belgian Waffles$8.95Light Belgian waffles covered with an assortment of fresh berries and whipped cream900" 
##                                                                                                                     food 
##                                                "French Toast$4.50Thick slices made from our homemade sourdough bread600" 
##                                                                                                                     food 
##                         "Homestyle Breakfast$6.95Two eggs, bacon or sausage, toast, and our ever-popular hash browns950"
```

```r
# Get the items on the menu and prices.
xpathSApply(rootNode, "//name", xmlValue)
```

```
## [1] "Belgian Waffles"             "Strawberry Belgian Waffles" 
## [3] "Berry-Berry Belgian Waffles" "French Toast"               
## [5] "Homestyle Breakfast"
```

```r
xpathSApply(rootNode, "//price", xmlValue)
```

```
## [1] "$5.95" "$7.95" "$8.95" "$4.50" "$6.95"
```

```r
# Another example
fileUrlxml2 <- "http://espn.go.com/nfl/team/_/name/bal/baltimore-ravens"
doc2 <- htmlTreeParse(fileUrlxml2, useInternal = TRUE)
scores <-xpathSApply(doc2, "//li[@class='score']",xmlValue)
scores
```

```
## [1] "23-3"  "37-30" "23-17" "22-13"
```

```r
teams <- xpathSApply(doc2,"//li[@class='team-name']",xmlValue)
teams
```

```
##  [1] "San Francisco" "Dallas"        "Washington"    "New Orleans"  
##  [5] "Cincinnati"    "Pittsburgh"    "Cleveland"     "Carolina"     
##  [9] "Indianapolis"  "Tampa Bay"     "Atlanta"       "Cincinnati"   
## [13] "Pittsburgh"    "Tennessee"     "New Orleans"   "San Diego"    
## [17] "Miami"         "Jacksonville"  "Houston"       "Cleveland"
```
