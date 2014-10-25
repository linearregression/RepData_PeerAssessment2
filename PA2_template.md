---
title: "Reproducible Research: Peer Assessment 2"
output: 
  html_document:
    keep_md: true
---

# Title 

Analysis of NOAA weather events for population health effects and economic damage

# Synopsis


From  NOAA storm data for years 1950-2011, an emprical comparison is performed toinvestigate which weathe events are most costly in terms of US dollars and havig most damages to population health.

No inflation adjustment in dollars is made. Comparison is later restricted from 1995 to 2014 because of events collections changes, and missing data cannot be treated as zero.

For background of project, please refer to [README.md](https://github.com/linearregression/RepData_PeerAssessment2/blob/master/README.md)

# Data Processing
## Download data from source

```r
rm(list=ls())
targetfile <- 'StormData.csv'
zipData <- paste(targetfile,'.bz2', sep='')

if(!file.exists(zipData)) {
   print('Downloading StormData.zip ....')   
   download.file(url='https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2FStormData.csv.bz2', destfile=zipData, method='curl')
}
```
## Loading and preprocessing the data
For the two questions to research only columns are relevant:
BGN_DATE for time,
FATALITIES, "INJURIES for population health
PROPDMG, PROPDMGEXP, CROPDMG, CROPDMGEXP - for property and crop damages and their corresponding numeric exponents

Load data from csv

```r
require(utils) || install.packages('utils')
```

```
## [1] TRUE
```

```r
require(data.table) || install.packages('data.table')
```

```
## [1] TRUE
```

```r
columns <-c("NULL", "character", rep("NULL", 5), "character", rep("NULL", 14), rep("character", 5),'character', rep("NULL", 9))
stormdata <- read.csv2(file=zipData, header=TRUE, sep=",", stringsAsFactor=FALSE,  strip.white = TRUE, skipNul = TRUE, colClasses= columns)

# Find how many events
events <- unique(stormdata$EVTYPE)
population_health <- c("FATALITIES", "INJURIES")
# Get the Unit exponent for dollar amount damage of property and crops
propertydamage_exp <- unique(stormdata$PROPDMGEXP)
cropdamage_exp <- unique(stormdata$CROPDMGEXP)
```
# Cleanse data
## Event entries inconsistency
Event names are trimmed of leading and trailing whitespaces and normalized o lowercases. Events are not recategorized avoiding changing any original assumptions.

## Events collection inconsistency 
Events categorizations and data collections expands from 1950-2014. For example, only Tornado is collected from 1950 to 1954; but from 1996 onwards 48 event types are recorded.
Since missing data does not equate zero effect, the analysis to compare 1996 to present when collections methods are uniform and data most recent.

# Dollar damage exponents inconsistency
Some property and crop damage dollar amounts are recorded in two columns.
Base dollar amount in respective DMG, unit exponent in DMGEXP.
The total dollar amount need to be noralized from both DM and DMGEXP.
For example for property damanage 100 as 1 in PROPDMG, H or h in PROPDMGEXP. 
All non-numeric characters are normalzed to lower case and are limited to h,k,m,b (hundreds, thousand, million, billion).
Numeric characters are conerted to integer.
All others as 0, to be converted to 10^0 which is 1.







