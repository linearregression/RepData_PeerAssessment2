---
title: "NOAA Weather Population Health And Economic Damage Analysis"
output: 
  html_document:
    keep_md: true
---
# Title: NOAA Weather Population Health And Economic Damage Analysis from Storm

# Synopsis
An analysis of 
U.S. National Oceanic and Atmospheric Administration's (NOAA) storm database. This database tracks characteristics of major storms and weather events in the United States, including when and where they occur, as well as estimates of any fatalities, injuries, and property damage.
* which types of events (as indicated in the EVTYPE variable) are most harmful with respect to population health?
* which types of events have the greatest economic consequences?

For background of project, please refer to [README.md](https://github.com/linearregression/RepData_PeerAssessment2/blob/master/README.md)

# Data Processing
## Download data and unzip locally

```r
rm(list=ls())
require(utils)
targetfile <- 'StormData.csv'
zipData <- paste(targetfile,'.bz2', sep='')

if(!file.exists(zipData)) {
   print('Downloading StormData.zip ....')   
   download.file(url='https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2FStormData.csv.bz2', destfile=zipData, method='curl')
}
```
## Loading and preprocessing the data
Load data from csv

```r
require(utils)
require(data.table)
stormdata <- stormdata <- read.csv2(file=zipData, header=TRUE, sep=",", stringsAsFactor=FALSE,  strip.white = TRUE, skipNul = TRUE, nrows=100)
```
# Analysis

# Results.


## Publishing Results

```r
title <- "NOAA Weather Population Health And Economic Damage Analysis"
html <- "PA2_template.html"
result <- rpubsUpload(title, html)
```

```
## Error: could not find function "rpubsUpload"
```

```r
if (!is.null(result$continueUrl)) 
    browseURL(result$continueUrl) else stop(result$error)
```

```
## Error: object 'result' not found
```

```r
# update the same document with a new title
updateResult <- rpubsUpload(title, html, result$id)
```

```
## Error: could not find function "rpubsUpload"
```
