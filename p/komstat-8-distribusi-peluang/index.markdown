---
title: 'KOMSTAT 8 : Distribusi Peluang'
author: ~
date: '2020-12-24'
slug: komstat-8-distribusi-peluang
categories: [Komstat]
tags: [Komstat]
description : 'Ringkasan materi pertemuan 8 mata kuliah Komputasi Statitik D4 KS - Distribusi Peluang'  
---
# Proses Bernoulli
Suatu eksperimen biasanya dilakukan percobaan secara berulang-ulang. Setiap perulangan tersebut menghasilkan dua kemungkinan hasil yaitu berhasil atau gagal. Proses ini kemudian disebut sebagai proses bernoulli *(Bernoulli Process)*. Setiap percobaannya disebut sebagai percobaan bernoulli *(Bernoulli Trial)*.

Proses Bernoulli harus memiliki sifat-sifat berikut berikut:
1. Percobaan terdiri dari percobaan berulang.
2. Setiap percobaan hasilnya dapat diklasifikasikan sebagai sukses atau gagal.
3. Probabilitas keberhasilan, dilambangkan dengan p dan tetap konstan dari percobaan ke percobaan.
4. Uji coba berulang bersifat independen.

# Distribusi Binomial
Jumlah X keberhasilan dalam n percobaan Bernoulli disebut binomial random variable.
Selanjutnya, distribusi probabilitas random variable diskrit ini disebut distribusi binomial dimana nilainya akan dilambangkan dengan $ b(x; n, p) $ karena bergantung pada jumlah percobaan dan probabilitas keberhasilan pada percobaan tertentu.

## Distribusi binomial dalam R

#### dbinom()
```{}
dbinom(x, size, prob, log = FALSE)
```
Fungsi **dbinom**  mengembalikan nilai fungsi kepadatan peluang (pdf) dari distribusi binomial yang diberikan variabel acak tertentu x, jumlah percobaan (size) dan probabilitas keberhasilan pada setiap percobaan (prob).

**Contoh Soal**

Sasha flips a fair coin 20 times. What is the probability that the coin lands on heads exactly 7 times?

```r
#find the probability of 7 successes during 20 trials where the probability of
#success on each trial is 0.5

dbinom(x=7, size=20, prob=.5)
```

```
## [1] 0.07392883
```

#### pbinom()
```{}
pbinom(q, size, prob, lower.tail = TRUE, log.p = FALSE)
```
Fungsi **pbinom**  mengembalikan nilai fungsi kepadatan kumulatif (cdf) dari distribusi binomial dengan variabel acak tertentu q, jumlah percobaan (size) dan probabilitas keberhasilan pada setiap percobaan (prob).

**Contoh Soal**

Ando flips a fair coin 5 times. What is the probability that the coin lands on heads more than 2 times?


```r
#find the probability of more than 2 successes during 5 trials where the
#probability of success on each trial is 0.5

pbinom(2, size=5, prob=.5, lower.tail=FALSE)
```

```
## [1] 0.5
```

#### qbinom()
```{}
qbinom(p, size, prob, lower.tail = TRUE, log.p = FALSE)
```
Fungsi **qbinom**  mengembalikan nilai fungsi inverse kepadatan kumulatif (cdf) dari distribusi binomial yang diberi variabel acak tertentu p, jumlah percobaan (size) dan probabilitas keberhasilan pada setiap percobaan (prob).

**Contoh Soal**

```r
#find the 10th quantile of a binomial distribution with 10 trials and prob
#of success on each trial = 0.4

qbinom(.10, size=10, prob=.4)
```

```
## [1] 2
```

```r
#find the 40th quantile of a binomial distribution with 30 trials and prob
#of success on each trial = 0.25

qbinom(.40, size=30, prob=.25)
```

```
## [1] 7
```

#### rbinom()
```{}
rbinom(n, size, prob)
```
Fungsi **rbinom**  menghasilkan vektor variabel acak terdistribusi binomial dengan panjang vektor  n, jumlah percobaan (size) dan probabilitas keberhasilan pada setiap percobaan (prob).

**Contoh Soal**

```r
#generate a vector that shows the number of successes of 10 binomial experiments with
#100 trials where the probability of success on each trial is 0.3.

results <- rbinom(10, size=100, prob=.3)
results
```

```
##  [1] 28 24 27 33 29 35 25 33 35 35
```

```r
#find mean number of successes in the 10 experiments (compared to expected
#mean of 30)

mean(results)
```

```
## [1] 30.4
```

```r
#generate a vector that shows the number of successes of 1000 binomial experiments
#with 100 trials where the probability of success on each trial is 0.3.

results <- rbinom(1000, size=100, prob=.3)

#find mean number of successes in the 100 experiments (compared to expected
#mean of 30)

mean(results)
```

```
## [1] 30.087
```

**log.p : logical;** if TRUE, probabilities p are given as log(p). 

**Contoh Soal 1**

Sebuah koin dilempar 10 kali. Berapa peluang bahwa akan muncul tepat angka sebanyak 6 kali?

Jawab : 

```r
size =10; prob = 0.5; x = 6
dbinom(6, 10, 0.5)
```

```
## [1] 0.2050781
```

```r
pbinom(6, 10, 0.5) #kumulatif mulai dari muncul 0 kali hingga 6 kali
```

```
## [1] 0.828125
```









