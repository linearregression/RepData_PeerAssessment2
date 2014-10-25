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
Because missing data does not equate zero effect, the analysis is thus restricted to most recent collection period - 1996 to present.

# Dollar damage exponents inconsistency
Some property[PPROP] and crop[CROP] damage dollar amounts are recorded in two columns:
 * base dollar amount in respective DMG column
 * unit exponent in DMGEXP column.

Unadjusted dollar amounts, based on the value and suffix. 
Suffix (h,k,m,b) is converted to unadjusted multipler; (+,-,? and empty sring) is converted to multipler of 1. Numeric characters converted to integer multipler.

# New data columns
Injuries and fatalies are grouped as new column population_safety.
Property and crop damages are grouped under economic_damage.


```r
require(stringr) || install.packages('stringr')
```

```
## [1] TRUE
```

```r
propertydamage_exp <- unique(stormdata$PROPDMGEXP)
cropdamage_exp <- unique(stormdata$CROPDMGEXP)


stormdata$YEAR <-format(as.Date(stormdata$BGN_DATE , "%m/%d/%Y"), "%Y", drop0trailing=TRUE)
# only consider fro year 1995 to 2014
stormdata <- stormdata[stormdata$YEAR >= 1995 & stormdata$YEAR <= 2014,] 
# Trim leading and trailing whitespace, tolower case.
stormdata$EVTYPE <- as.vector(tolower(str_trim(stormdata$EVTYPE)))
stormdata$PROPDMGEXP <- as.vector(tolower(str_trim(stormdata$PROPDMGEXP)))
stormdata$CROPDMGEXP <- as.vector(tolower(str_trim(stormdata$CROPDMGEXP)))

# Crete

# Group injuries and fatalies as population_safty


# Group property damage and crop damage as econmoic_damage


# Summarise population_safety by events & rank total  


# Summarise econmoic_damage by events & rank total  
```


The scope of events related harmful to population health are:

# Analysis

# Results.
Tornado and flood causes most economic damage, as well as damage to population health.




## Publishing Results

```r
require(knitr) || install.packages('knitr')
```

```
## [1] TRUE
```

```r
require(markdown) || install.packages('markowns')
```

```
## Loading required package: markdown
```

```
## [1] TRUE
```

```r
title <- "NOAA Weather Population Health And Economic Damage Analysis"
html <- "PA2_template.html"
result <- rpubsUpload(title, html)
if (!is.null(result$continueUrl)) 
    browseURL(result$continueUrl) else stop(result$error)

# update the same document with a new title
updateResult <- rpubsUpload(title, html, result$id)
```

```
## Error in readBin(conn, what = "raw", n = contentLength): invalid 'n' argument
```

