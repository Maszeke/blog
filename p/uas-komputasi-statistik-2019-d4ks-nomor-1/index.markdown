---
title: UAS Komputasi Statistik 2019 D4KS Nomor 1
author: ~
date: '2020-12-29'
slug: uas-komputasi-statistik-2019-d4ks-nomor-1
categories: []
tags: []
---
## Soal
Dari library(nycflights13), gunakan dataset flights, airports, airlines, planes, dan weather. Kemudian lakukan :

1. Buat bar-chart untuk urutan 10 maskapai penerbangan airlines (lengkap dengan nama maskapainya) dengan rata-rata waktu keterlambatan keberangkatan (departure delay) paling lama
2. Uji apakah terdapat beda rata-rata jarak yang ditempuh dari pesawat yang dibuat sebelum tahun 2000 dan setelah (termasuk) tahun 2000
3. Uji apakah terdapat beda rata-rata waktu keterlambatan keberangkatan dari empat musim (winter: oct-jan, spring: feb-may, summer: june-sept). Lakukan uji lanjutan jika diperlukan.
4. Berdasarkan data diatas, maskapai mana yang paling bagus kinerjanya serta berikan alasannya

## Jawab 

#### Nomor 1

**Perintah :** Membuat bar-chart untuk rata-rata waktu keterlambatan keberangkatan (departure delay) paling lama

**Langkah-langkah :**

- Load data

```r
library(nycflights13)
```

```
## Warning: package 'nycflights13' was built under R version 3.6.3
```

```r
library(dplyr)
```

```
## Warning: package 'dplyr' was built under R version 3.6.3
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
head(flights)
```

```
## # A tibble: 6 x 19
##    year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
##   <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
## 1  2013     1     1      517            515         2      830            819
## 2  2013     1     1      533            529         4      850            830
## 3  2013     1     1      542            540         2      923            850
## 4  2013     1     1      544            545        -1     1004           1022
## 5  2013     1     1      554            600        -6      812            837
## 6  2013     1     1      554            558        -4      740            728
## # ... with 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
## #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
## #   hour <dbl>, minute <dbl>, time_hour <dttm>
```
- Summeries data berdasarkan maskapai ```carrier``` dengan rata-rata Keterlambatan keberangktaan ```dep_delay``

```r
data = summarise(group_by(flights, carrier), mean_dep_delay = mean(dep_delay, na.rm = TRUE))
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

```r
data = arrange(data, desc(mean_dep_delay))
```
- Buat barplot dari data tersebut

```r
barplot(data$mean_dep_delay, main="Car Distribution", names.arg=data$carrier)
```

<img src="index_files/figure-html/unnamed-chunk-3-1.png" width="672" />

