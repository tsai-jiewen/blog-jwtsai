---
title: "bibliometrixExtra: R for synonyms"
date: 2021-11-29
draft: false
featured: false
authors:
  - admin
tags:
  - xaringan
  - r
  - zh-tw
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---

[![hackmd-github-sync-badge](https://hackmd.io/rukacjfsSMKLe32BVjUhrA/badge)](https://hackmd.io/rukacjfsSMKLe32BVjUhrA)


:::success
Build a customed package `bibliometrixExtra`  (Extended functions for bibliometrix) to put the functions in, that will make the synonyms replacing processing  easier.
:::

{{< toc >}}


:::spoiler older info

Need to replace synonyms, but the function `termExtraction()` in **R-bibliometrx** can't work smoothly??


## ★ replace synonyms by importing csv file `syn_replace()` 
(copy/paste and run this func.)
the customed `str_replace_multiple` can use multiple patterns to be replaced, but it's not enough. we expect a replacing function can work like in VOSviewr. the `syn_replace` can do it.

==version 3, it can replace the full terms, and a term inculeds '-' or ' '(space).==

### Example:
if we want to replace 'citaion' as 'apple', then
- 'cocitaion' --> 'cocitaion' 
- 'co-citaion' --> 'co-citaion' 
- 'co citation' --> 'co citation'
- 'ciation' --> 'apple'

```r=
## version 3
syn_replace <- function(df, data, tag){
  data[[tag]] <- gsub(
    ';_', replacement = '; ', 
    x= gsub('^_', replacement = ' ', 
        x= gsub('\\s', replacement = '_', 
            x= gsub('-', replacement = '_-_', data[[tag]] 
    ) ) ) ) 
  for (i in 1:nrow(df)){
    data[[tag]] <- gsub(
      x = data[[tag]] , 
      pattern = paste0( '\\<', df[i, 'pattern'], '\\>' ), 
      replacement = df[i, 'replace'])
  }
  data[[tag]] <- gsub('_-_', replacement = '-', x=data[[tag]] )
  data[[tag]] <- gsub('_', replacement = ' ', x=data[[tag]] )
  return( data[[tag]] )
}
```
:::





## ★ Usage

```r=
remotes::install_github('tsai-jiewen/bibliometrixExtra')
library(bibliometrixExtra) # syn_export(), syn_replace()
library(bibliometrix)
library(tidyverse)
```

### 1. Using `syn_export()` to export csv from `bibliometrix`.
(from the customed package `bibliometrixExtra` )

```r=
data(scientometrics, package = "bibliometrixData")

# make a tab-freq table, than export as a csv file
syn_export(
    file = 'test1124.csv',   # export file name
    data = scientometrics,  # the biblio data
    tag = 'ID'               # the field tag  
)
```

the export file looks like that:
![](https://i.imgur.com/UmKGKsQ.png)


:::spoiler older info
- delete col A (number)
- change col B name to 'pattern' (MUST!)
- change col C name to 'replace' (MUST!), than delete all numbers.
:::

- ==fill in the terms you want to replace by in col C (replace), one by one.==
- ==delete the terms no need to change in col B (pattern).==

like this, (for example)
change 'CITATIONS' and 'CITATION ANALYSIS' to 'APPLE'.
![](https://i.imgur.com/XGQ1sWP.png)


### 2. Using `syn_import()` to read the edited csv file into R env.
```r=
DTF <- syn_import(file = 'test1124.csv')
```
check
```
> DTF
            pattern  replace
1         CITATIONS    APPLE
2 CITATION ANALYSIS    APPLE
```

### 3. Using `syn_replace()` to replace the original synonyms terms. 
(from the customed package `bibliometrixExtra` )

take 'ID' for example.
you can change to 'DE', etc, but remember keep the `$ID` and `tag='ID'` as the same.
```r=
scientometrics$ID <- syn_replace(
  df = DTF,               # the edited file you import
  data = scientometrics,  # the original biblio data
  tag = 'ID'              # the field tag  
)
```

:::spoiler older info
check before, there were 8 'CITATIONS', 25 'CITATION ANALYSIS' and 0 'APPLE' in the data frame.
```
> data(scientometrics, package = "bibliometrixData")
> scientometrics$ID %>% 
+   stringr::str_match(pattern ='CITATIONS') %>%
+   table()
.
CITATIONS 
        8 
        
> scientometrics$ID %>% 
+   stringr::str_match(pattern = 'CITATION ANALYSIS') %>%
+   table()
.
CITATION ANALYSIS 
               25 
               
> scientometrics$ID %>% 
+   stringr::str_match(pattern = 'APPLE') %>%
+   table()
< table of extent 0 >
```

check after, there are zero 'CITATIONS' and 'CITATION ANALYSIS', but 33 'APPLE' now! 
It's successful in replacing!
```
> scientometrics$ID %>% 
+   stringr::str_match(pattern = 'CITATIONS') %>%
+   table()
< table of extent 0 >

> scientometrics$ID %>% 
+   stringr::str_match(pattern = 'CITATION ANALYSIS') %>%
+   table()
< table of extent 0 >

> scientometrics$ID %>% 
+   stringr::str_match(pattern = 'APPLE') %>%
+   table()
.
APPLE 
   33 
```
:::



### Remark
the term-replacing takes place at the bibliometrix data.frame, so it can continue to be used in the next analysis.



## thematicEvolution() 
Perform a Thematic Evolution Analysis.

- M: can replace as your own biblio dataset.
- years: can set the timepoints as you want
- See [thematicEvolution: Perform a Thematic Evolution Analysis](https://www.rdocumentation.org/packages/bibliometrix/versions/3.1.4/topics/thematicEvolution)

```r=
nexus <- thematicEvolution(M,field="DE", years=c(2005, 2010, 2015), n=250, minFreq=2)
plotThematicEvolution(nexus$Nodes,nexus$Edges)
```

![](https://i.imgur.com/wT0NdTw.png)


## fieldByYear() 
Field Tag distribution by Year
- See [fieldByYear: Field Tag distribution by Year](https://www.rdocumentation.org/packages/bibliometrix/versions/3.1.4/topics/fieldByYear)

```r=
fieldByYear(scientometrics, field = "ID", timespan = c(2005,2015), 
            min.freq = 5, n.items = 5, graph = TRUE)
```

![](https://i.imgur.com/GhTr1N5.png)




---

:::spoiler older info (functions)
## (older versions) str_replace_multiple
just a liitle bit better than `stringr::str_replace_all`

```r=
str_replace_multiple <- function(pattern_list, data, tag, replacement){
  require(stringr)
  for (i in 1:length(pattern_list)){
    data[[tag]] <- stringr::str_replace_all(
      data[[tag]] , 
      pattern = pattern_list[i], 
      replacement = replacement)
  }
  return(data[[tag]])
}
```
## (older versions) str_replace_df 
```r=
## version 2
str_replace_df <- function(df, data, tag){
  for (i in 1:nrow(df)){
    data[[tag]] <- gsub(
      x = data[[tag]] , 
      pattern = paste0( '\\<', df[i, 'pattern'], '\\>' ), 
      replacement = df[i, 'replace'])
  }
  return(data[[tag]])
}
```



```r=
## version 1
str_replace_df <- function(df, data, tag){
  require(stringr)
  for (i in 1:nrow(df)){
    data[[tag]] <- stringr::str_replace_all(
      data[[tag]] , 
      pattern = df[i, 'pattern'], 
      replacement = df[i, 'replace'])
  }
  return(data[[tag]])
}
```
:::

