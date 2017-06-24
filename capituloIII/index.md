---
title       : Manipulación de datos 
subtitle    : Formato y limpieza de datos 
author      : Kevin Pérez C, Profesor Auxiliar
job         : Departamento de Matemáticas y Estadística - Universidad de Córdoba
logo        : unicordoba3.png
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
widescreen  : true
smaller     : true
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax, bootstrap, quiz, shiny, interactive]            
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Definición de datos


<q>Los datos son valores de `variables` cualitativas o cuantitativas, pertenecientes a un `conjunto de objetos`.</q> [http://en.wikipedia.org/wiki/Data](http://en.wikipedia.org/wiki/Data)


> - **_Conjunto de objetos_**: En ocasiones se le llama población; es un conjunto de objetos o personas con características similares. 
> - **_Variables_**: Una medida o característica de un objeto.

---

## Definición de datos


<q>Los datos son valores de variables `cualitativas` o `cuantitativas`, pertenecientes a un conjunto de objetos.</q> [http://en.wikipedia.org/wiki/Data](http://en.wikipedia.org/wiki/Data)


> - **_Cualitativas_**: País de origen, sexo, tratamiento
> - **_Cuantitavas_**: Estatura, peso, presión sanguínea.

---

## Como deberían lucir los datos 

<br> 

<img class=center src= assets/img/img1.png height=450/>

---

## Como realmente lucen 

<br> 

<img class=center src= assets/img/img2.png height=450/>

---

## De donde vienen los datos  

<br> 

<img class=center src= assets/img/img3.png height=450/>

---




## Los datos fuente 

- El extraño archivo binario que su máquina imprime
- El archivo Excel no formateado con 10 hojas de trabajo que le envió su compañero de empresa 
- Los complicados datos JSON que obtiene al conectarse a la API de Twitter
- Los números ingresados manualmente que se recolectan mirando a través de un microscopio

---

## Los datos ordenados 

- Cada variable que se mide debe estar en una columna
- Cada observación diferente de esa variable debe estar en una fila diferente
- Debe haber una tabla para cada "tipo" de variable
- Si se tienen varias tablas, debe incluir una columna cada tabla que les permita estar vinculadas

---

## Los datos ordenados 
<br> 

<img class=center src= assets/img/img4.png height=450/>

- Cumple con los tres primeros conceptos 

---

## Los datos ordenados 

<br> 

<img class=center src= assets/img/img5.png height=450/>

- ¿Falla en alguno de los conceptos? 

---

## Ancho vs. largo 

<img class=center src= assets/img/img6.png height=450/>

---

## Indexado: Seleccionar - filtrar 


```r
set.seed(13435)
X <- data.frame("var1" = sample(1:5), "var2" = sample(6:10), 
                "var3" = sample(11:15))
X <- X[sample(1:5),]; X$var2[c(1,3)] = NA
X
```

```
##   var1 var2 var3
## 1    2   NA   15
## 4    1   10   11
## 2    3   NA   12
## 3    5    6   14
## 5    4    9   13
```

---

## Indexado: Seleccionar - filtrar 


```r
X[,1]
```

```
## [1] 2 1 3 5 4
```

```r
X[,"var1"]
```

```
## [1] 2 1 3 5 4
```

```r
X[1:2,"var2"]
```

```
## [1] NA 10
```

---

## Indexado: Seleccionar - filtrar 


```r
X[which(X$var2 > 8),]
```

```
##   var1 var2 var3
## 4    1   10   11
## 5    4    9   13
```

---


## Indexado: Seleccionar - filtrar 


```r
X[(X$var1 <= 3 & X$var3 > 11),]
```

```
##   var1 var2 var3
## 1    2   NA   15
## 2    3   NA   12
```

```r
X[(X$var1 <= 3 | X$var3 > 15),]
```

```
##   var1 var2 var3
## 1    2   NA   15
## 4    1   10   11
## 2    3   NA   12
```

---

## Adicionando filas y columnas


```r
X$var4 <- rnorm(5)
X
```

```
##   var1 var2 var3       var4
## 1    2   NA   15  0.1875960
## 4    1   10   11  1.7869764
## 2    3   NA   12  0.4966936
## 3    5    6   14  0.0631830
## 5    4    9   13 -0.5361329
```

---

## Adicionando filas y columnas


```r
Y <- cbind(X,rnorm(5))
Y
```

```
##   var1 var2 var3       var4    rnorm(5)
## 1    2   NA   15  0.1875960  0.62578490
## 4    1   10   11  1.7869764 -2.45083750
## 2    3   NA   12  0.4966936  0.08909424
## 3    5    6   14  0.0631830  0.47838570
## 5    4    9   13 -0.5361329  1.00053336
```

---

## `Sort` (Orden simple)


```r
sort(X$var1)
```

```
## [1] 1 2 3 4 5
```

```r
sort(X$var1,decreasing = TRUE)
```

```
## [1] 5 4 3 2 1
```

```r
sort(X$var2,na.last = TRUE)
```

```
## [1]  6  9 10 NA NA
```

---

## `Order` (Orden compuesto)


```r
X[order(X$var1),]
```

```
##   var1 var2 var3       var4
## 4    1   10   11  1.7869764
## 1    2   NA   15  0.1875960
## 2    3   NA   12  0.4966936
## 5    4    9   13 -0.5361329
## 3    5    6   14  0.0631830
```

---

## `Order` (Orden compuesto)


```r
X[order(X$var1,X$var3),]
```

```
##   var1 var2 var3       var4
## 4    1   10   11  1.7869764
## 1    2   NA   15  0.1875960
## 2    3   NA   12  0.4966936
## 5    4    9   13 -0.5361329
## 3    5    6   14  0.0631830
```

---

## `mutate` (Secuencias)

Algunas veces en el análisis de datos se hace necesario la creación de variables


```r
s1 <- seq(1,10, by = 2) ; s1
```

```
## [1] 1 3 5 7 9
```

```r
s2 <- seq(1,10,length = 3); s2
```

```
## [1]  1.0  5.5 10.0
```

```r
x <- c(1,3,8,25,100); seq(along = x)
```

```
## [1] 1 2 3 4 5
```

---
 
## `mutate` (Subconjuntos de variables)


```r
restData <- read.csv("./data/restaurants.csv")
restData$nearMe = restData$neighborhood %in% c("Roland Park", "Homeland")
table(restData$nearMe)
```

```
## 
## FALSE  TRUE 
##  1314    13
```

---

## `mutate` (Variables binarias)


```r
restData$zipWrong = ifelse(restData$zipCode < 0, TRUE, FALSE)
table(restData$zipWrong,restData$zipCode < 0)
```

```
##        
##         FALSE TRUE
##   FALSE  1326    0
##   TRUE      0    1
```

---

## `mutate` (Variables categóricas)


```r
restData$zipGroups = cut(restData$zipCode,
                         breaks = quantile(restData$zipCode))
table(restData$zipGroups)
```

```
## 
## (-2.123e+04,2.12e+04]  (2.12e+04,2.122e+04] (2.122e+04,2.123e+04] 
##                   337                   375                   282 
## (2.123e+04,2.129e+04] 
##                   332
```

```r
#table(restData$zipGroups,restData$zipCode)
```

---

## `mutate` (Cortes `cutting`)


```r
suppressMessages(suppressWarnings(library(Hmisc)))
restData$zipGroups = cut2(restData$zipCode, g = 4)
table(restData$zipGroups)
```

```
## 
## [-21226,21205) [ 21205,21220) [ 21220,21227) [ 21227,21287] 
##            338            375            300            314
```

---

## `mutate` (Creando factores)


```r
restData$zcf <- factor(restData$zipCode)
restData$zcf[1:10]
```

```
##  [1] 21206 21231 21224 21211 21223 21218 21205 21211 21205 21231
## 32 Levels: -21226 21201 21202 21205 21206 21207 21208 21209 ... 21287
```

```r
class(restData$zcf)
```

```
## [1] "factor"
```

---

## `mutate` (Niveles del factor)


```r
yesno <- sample(c("yes","no"), size = 10, replace = TRUE)
yesnofac = factor(yesno, levels = c("yes", "no"))
relevel(yesnofac, ref = "no")
```

```
##  [1] no  yes no  yes yes yes yes yes yes yes
## Levels: no yes
```

```r
as.numeric(yesnofac)
```

```
##  [1] 2 1 2 1 1 1 1 1 1 1
```

---

## `mutate` (Transformaciones comunes)

* `abs(x)` valor absoluto
* `sqrt(x)` raíz cuadrada
* `ceiling(x)` ceiling(3.475) es 4
* `floor(x)` floor(3.475) es 3
* `round(x,digits=n)` round(3.475,digits=2) es 3.48
* `signif(x,digits=n)` signif(3.475,digits=2) es 3.5
* `cos(x), sin(x)` etc.
* `log(x)` logaritmo natural
* `log2(x)`, `log10(x)` otros logaritmos 
* `exp(x)` exponencial de x

---

## Formato - (Ancho: `wide`)

En general un conjunto de datos estructurado en forma tabular viene con el formato `wide`.


```r
suppressMessages(suppressWarnings(library(reshape2)))
head(mtcars)
```

```
##                    mpg cyl disp  hp drat    wt  qsec vs am gear carb
## Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
## Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
## Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
## Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
## Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1
```

---

## Formato - (Largo: `long`)

La función `melt` de la librería `reshape` es de mucha utilidad a la hora de especificar el formato `long`


```r
mtcars$carname <- rownames(mtcars)
carMelt <- melt(mtcars, id = c("carname","gear","cyl"),
                measure.vars = c("mpg","hp"))
head(carMelt)
```

```
##             carname gear cyl variable value
## 1         Mazda RX4    4   6      mpg  21.0
## 2     Mazda RX4 Wag    4   6      mpg  21.0
## 3        Datsun 710    4   4      mpg  22.8
## 4    Hornet 4 Drive    3   6      mpg  21.4
## 5 Hornet Sportabout    3   8      mpg  18.7
## 6           Valiant    3   6      mpg  18.1
```

---

## Formato - (Largo: `long`)

La función `melt` de la librería `reshape` es de mucha utilidad a la hora de especificar el formato `long`


```r
mtcars$carname <- rownames(mtcars)
carMelt <- melt(mtcars, id = c("carname","gear","cyl"),
                measure.vars = c("mpg","hp"))
tail(carMelt)
```

```
##           carname gear cyl variable value
## 59  Porsche 914-2    5   4       hp    91
## 60   Lotus Europa    5   4       hp   113
## 61 Ford Pantera L    5   8       hp   264
## 62   Ferrari Dino    5   6       hp   175
## 63  Maserati Bora    5   8       hp   335
## 64     Volvo 142E    4   4       hp   109
```

---

## Formato - (Modear: `cast`)


```r
cylData <- dcast(carMelt, cyl ~ variable, mean)
cylData
```

```
##   cyl      mpg        hp
## 1   4 26.66364  82.63636
## 2   6 19.74286 122.28571
## 3   8 15.10000 209.21429
```

---

## `Summarized` (Resumir)

Obtener una vista previa de los datos 


```r
head(restData, n = 3)
```

```
##    name zipCode neighborhood councilDistrict policeDistrict
## 1   410   21206    Frankford               2   NORTHEASTERN
## 2  1919   21231  Fells Point               1   SOUTHEASTERN
## 3 SAUTE   21224       Canton               1   SOUTHEASTERN
##                          Location.1 nearMe zipWrong      zipGroups   zcf
## 1 4509 BELAIR ROAD\nBaltimore, MD\n  FALSE    FALSE [ 21205,21220) 21206
## 2    1919 FLEET ST\nBaltimore, MD\n  FALSE    FALSE [ 21227,21287] 21231
## 3   2844 HUDSON ST\nBaltimore, MD\n  FALSE    FALSE [ 21220,21227) 21224
```

---

## `Summarized` (Resumir)

Obtener una vista previa de los datos 


```r
tail(restData, n = 3)
```

```
##                  name zipCode  neighborhood councilDistrict policeDistrict
## 1325 ZINK'S CAF\u0090   21213 Belair-Edison              13   NORTHEASTERN
## 1326     ZISSIMOS BAR   21211       Hampden               7       NORTHERN
## 1327           ZORBAS   21224     Greektown               2   SOUTHEASTERN
##                              Location.1 nearMe zipWrong      zipGroups
## 1325 3300 LAWNVIEW AVE\nBaltimore, MD\n  FALSE    FALSE [ 21205,21220)
## 1326      1023 36TH ST\nBaltimore, MD\n  FALSE    FALSE [ 21205,21220)
## 1327  4710 EASTERN Ave\nBaltimore, MD\n  FALSE    FALSE [ 21220,21227)
##        zcf
## 1325 21213
## 1326 21211
## 1327 21224
```

---

## `Summarized` (Resumir)


```r
summary(restData)
```

```
##                            name         zipCode             neighborhood
##  MCDONALD'S                  :   8   Min.   :-21226   Downtown    :128  
##  POPEYES FAMOUS FRIED CHICKEN:   7   1st Qu.: 21202   Fells Point : 91  
##  SUBWAY                      :   6   Median : 21218   Inner Harbor: 89  
##  KENTUCKY FRIED CHICKEN      :   5   Mean   : 21185   Canton      : 81  
##  BURGER KING                 :   4   3rd Qu.: 21226   Federal Hill: 42  
##  DUNKIN DONUTS               :   4   Max.   : 21287   Mount Vernon: 33  
##  (Other)                     :1293                    (Other)     :863  
##  councilDistrict       policeDistrict
##  Min.   : 1.000   SOUTHEASTERN:385   
##  1st Qu.: 2.000   CENTRAL     :288   
##  Median : 9.000   SOUTHERN    :213   
##  Mean   : 7.191   NORTHERN    :157   
##  3rd Qu.:11.000   NORTHEASTERN: 72   
##  Max.   :14.000   EASTERN     : 67   
##                   (Other)     :145   
##                           Location.1       nearMe         zipWrong      
##  1101 RUSSELL ST\nBaltimore, MD\n:   9   Mode :logical   Mode :logical  
##  201 PRATT ST\nBaltimore, MD\n   :   8   FALSE:1314      FALSE:1326     
##  2400 BOSTON ST\nBaltimore, MD\n :   8   TRUE :13        TRUE :1        
##  300 LIGHT ST\nBaltimore, MD\n   :   5                                  
##  300 CHARLES ST\nBaltimore, MD\n :   4                                  
##  301 LIGHT ST\nBaltimore, MD\n   :   4                                  
##  (Other)                         :1289                                  
##           zipGroups        zcf     
##  [-21226,21205):338   21202  :201  
##  [ 21205,21220):375   21224  :199  
##  [ 21220,21227):300   21230  :156  
##  [ 21227,21287]:314   21201  :136  
##                       21231  :127  
##                       21218  : 69  
##                       (Other):439
```

---

## `Summarized` (Resumir)


```r
str(restData)
```

```
## 'data.frame':	1327 obs. of  10 variables:
##  $ name           : Factor w/ 1277 levels "1919","19TH HOLE",..: 9 1 990 3 4 2 6 7 8 5 ...
##  $ zipCode        : int  21206 21231 21224 21211 21223 21218 21205 21211 21205 21231 ...
##  $ neighborhood   : Factor w/ 173 levels "Abell","Arlington",..: 53 52 18 66 104 33 98 133 98 157 ...
##  $ councilDistrict: int  2 1 1 14 9 14 13 7 13 1 ...
##  $ policeDistrict : Factor w/ 9 levels "CENTRAL","EASTERN",..: 3 6 6 4 8 3 6 4 6 6 ...
##  $ Location.1     : Factor w/ 1210 levels "1000 ALICEANNA ST\nBaltimore, MD\n",..: 833 324 550 755 484 532 498 525 500 571 ...
##  $ nearMe         : logi  FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ zipWrong       : logi  FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ zipGroups      : Factor w/ 4 levels "[-21226,21205)",..: 2 4 3 2 3 2 2 2 2 4 ...
##  $ zcf            : Factor w/ 32 levels "-21226","21201",..: 5 27 21 10 20 17 4 10 4 27 ...
```

---

## `Summarized` (Resumir)


```r
quantile(restData$councilDistrict,na.rm = TRUE)
```

```
##   0%  25%  50%  75% 100% 
##    1    2    9   11   14
```

```r
quantile(restData$councilDistrict,probs = c(0.5,0.75,0.9))
```

```
## 50% 75% 90% 
##   9  11  12
```

---

## `Summarized` (Resumir)


```r
table(restData$councilDistrict,restData$zipCode)
```

```
##     
##      -21226 21201 21202 21205 21206 21207 21208 21209 21210 21211 21212
##   1       0     0    37     0     0     0     0     0     0     0     0
##   2       0     0     0     3    27     0     0     0     0     0     0
##   3       0     0     0     0     0     0     0     0     0     0     0
##   4       0     0     0     0     0     0     0     0     0     0    27
##   5       0     0     0     0     0     3     0     6     0     0     0
##   6       0     0     0     0     0     0     0     1    19     0     0
##   7       0     0     0     0     0     0     0     1     0    27     0
##   8       0     0     0     0     0     1     0     0     0     0     0
##   9       0     1     0     0     0     0     0     0     0     0     0
##   10      1     0     1     0     0     0     0     0     0     0     0
##   11      0   115   139     0     0     0     1     0     0     0     1
##   12      0    20    24     4     0     0     0     0     0     0     0
##   13      0     0     0    20     3     0     0     0     0     0     0
##   14      0     0     0     0     0     0     0     0     4    14     0
##     
##      21213 21214 21215 21216 21217 21218 21220 21222 21223 21224 21225
##   1      2     0     0     0     0     0     0     7     0   140     1
##   2      0     0     0     0     0     0     0     0     0    54     0
##   3      2    17     0     0     0     3     0     0     0     0     0
##   4      0     0     0     0     0     0     0     0     0     0     0
##   5      0     0    31     0     0     0     0     0     0     0     0
##   6      0     0    15     1     0     0     0     0     0     0     0
##   7      0     0     6     7    15     6     0     0     0     0     0
##   8      0     0     0     0     0     0     0     0     2     0     0
##   9      0     0     0     2     8     0     0     0    53     0     0
##   10     0     0     0     0     0     0     1     0     0     0    18
##   11     0     0     0     0     9     0     0     0     1     0     0
##   12    13     0     0     0     0    26     0     0     0     0     0
##   13    13     0     1     0     0     0     0     0     0     5     0
##   14     1     0     1     0     0    34     0     0     0     0     0
##     
##      21226 21227 21229 21230 21231 21234 21237 21239 21251 21287
##   1      0     0     0     1   124     0     0     0     0     0
##   2      0     0     0     0     0     0     1     0     0     0
##   3      0     1     0     0     0     7     0     0     2     0
##   4      0     0     0     0     0     0     0     3     0     0
##   5      0     0     0     0     0     0     0     0     0     0
##   6      0     0     0     0     0     0     0     0     0     0
##   7      0     0     0     0     0     0     0     0     0     0
##   8      0     2    13     0     0     0     0     0     0     0
##   9      0     0     0    11     0     0     0     0     0     0
##   10    18     0     0   133     0     0     0     0     0     0
##   11     0     0     0    11     0     0     0     0     0     0
##   12     0     0     0     0     2     0     0     0     0     0
##   13     0     1     0     0     1     0     0     0     0     1
##   14     0     0     0     0     0     0     0     0     0     0
```

---

## `Summarized` (Resumir)


```r
sum(is.na(restData$councilDistrict))
```

```
## [1] 0
```

```r
any(is.na(restData$councilDistrict))
```

```
## [1] FALSE
```

```r
all(restData$zipCode > 0)
```

```
## [1] FALSE
```

---

## `Summarized` (Resumir)


```r
colSums(is.na(restData))
```

```
##            name         zipCode    neighborhood councilDistrict 
##               0               0               0               0 
##  policeDistrict      Location.1          nearMe        zipWrong 
##               0               0               0               0 
##       zipGroups             zcf 
##               0               0
```

```r
all(colSums(is.na(restData)) == 0)
```

```
## [1] TRUE
```

---


## `Summarized` (Resumir)


```r
table(restData$zipCode %in% c("21212"))
```

```
## 
## FALSE  TRUE 
##  1299    28
```

```r
table(restData$zipCode %in% c("21212","21213"))
```

```
## 
## FALSE  TRUE 
##  1268    59
```

---

## `Summarized` (Resumir)


```r
data(UCBAdmissions)
DF = as.data.frame(UCBAdmissions)
summary(DF)
```

```
##       Admit       Gender   Dept       Freq      
##  Admitted:12   Male  :12   A:4   Min.   :  8.0  
##  Rejected:12   Female:12   B:4   1st Qu.: 80.0  
##                            C:4   Median :170.0  
##                            D:4   Mean   :188.6  
##                            E:4   3rd Qu.:302.5  
##                            F:4   Max.   :512.0
```

---

## `Summarized` (Resumir)


```r
xt <- xtabs(Freq ~ Gender + Admit,data = DF)
xt
```

```
##         Admit
## Gender   Admitted Rejected
##   Male       1198     1493
##   Female      557     1278
```

---

## `Summarized` (Resumir)


```r
warpbreaks$replicate <- rep(1:9, len = 54)
xt = xtabs(breaks ~.,data=warpbreaks)
ftable(xt)
```

```
##              replicate  1  2  3  4  5  6  7  8  9
## wool tension                                     
## A    L                 26 30 54 25 70 52 51 26 67
##      M                 18 21 29 17 12 18 35 30 36
##      H                 36 21 24 18 10 43 28 15 26
## B    L                 27 14 29 19 29 31 41 20 44
##      M                 42 26 19 16 39 28 21 39 29
##      H                 20 21 24 17 13 15 15 16 28
```

---

## `Summarized` (Resumir)


```r
fakeData = rnorm(1e5)
object.size(fakeData)
```

```
## 800040 bytes
```

```r
print(object.size(fakeData), units = "Mb")
```

```
## 0.8 Mb
```

---

## `merge` (Operaciones sobre tablas)


```r
reviews = read.csv("./data/reviews.csv")
solutions <- read.csv("./data/solutions.csv")
head(reviews, 2)
```

```
##   id solution_id reviewer_id      start       stop time_left accept
## 1  1           3          27 1304095698 1304095758      1754      1
## 2  2           4          22 1304095188 1304095206      2306      1
```

```r
head(solutions, 2)
```

```
##   id problem_id subject_id      start       stop time_left answer
## 1  1        156         29 1304095119 1304095169      2343      B
## 2  2        269         25 1304095119 1304095183      2329      C
```

---

## `merge` (Operaciones sobre tablas)

- Une data frames
- Parámetros Importantes: _x_, _y_, _by_, _by.x_, _by.y_, _all_


```r
names(reviews)
```

```
## [1] "id"          "solution_id" "reviewer_id" "start"       "stop"       
## [6] "time_left"   "accept"
```

```r
names(solutions)
```

```
## [1] "id"         "problem_id" "subject_id" "start"      "stop"      
## [6] "time_left"  "answer"
```

---

## `merge` (Operaciones sobre tablas)


```r
mergedData <- merge(reviews,solutions, by.x = "solution_id",
                    by.y = "id", all = TRUE)
head(mergedData, 3)
```

```
##   solution_id id reviewer_id    start.x     stop.x time_left.x accept
## 1           1  4          26 1304095267 1304095423        2089      1
## 2           2  6          29 1304095471 1304095513        1999      1
## 3           3  1          27 1304095698 1304095758        1754      1
##   problem_id subject_id    start.y     stop.y time_left.y answer
## 1        156         29 1304095119 1304095169        2343      B
## 2        269         25 1304095119 1304095183        2329      C
## 3         34         22 1304095127 1304095146        2366      C
```

---

## `merge` (Operaciones sobre tablas)


```r
intersect(names(solutions),names(reviews))
```

```
## [1] "id"        "start"     "stop"      "time_left"
```

```r
mergedData2 <- merge(reviews,solutions, all = TRUE)
head(mergedData2, 3)
```

```
##   id      start       stop time_left solution_id reviewer_id accept
## 1  1 1304095119 1304095169      2343          NA          NA     NA
## 2  1 1304095698 1304095758      1754           3          27      1
## 3  2 1304095119 1304095183      2329          NA          NA     NA
##   problem_id subject_id answer
## 1        156         29      B
## 2         NA         NA   <NA>
## 3        269         25      C
```

