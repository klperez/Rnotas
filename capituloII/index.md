---
title       : Obtención y Manipulación de datos 
subtitle    : Obtención de datos locales  
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

## Obtención de datos en `R`

La obtención de datos en R, consiste en la tarea de llevar al espacio de trabajo, datos desde cualquier fuente de información. Las fuentes mas comunes son:

- Archivos planos, .txt, .csv, etc.
- Archivos de Excel .xlsx
- Otros software estadístico, SAS, SPSS Modeler, Stata.
- Bases de datos, MySQL, Mongo, SQL Server, Postgre SQL
- Datos de la web y redes sociales 

---

## Leyendo Datos

Existen una serie de funciones principales para la lectura de datos en R.

- `read.table`, `read.csv`, para leer datos tabulares 
- `readLines`, para leer lineas de un archivo de texto
- `source`, para leer en los archivos de código R (`inverse` de `dump`) 
- `dget`, para leer en los archivos de código R (`inverse` de `dput`)
- `load`, para la lectura en espacios de trabajo guardados
- `unserialize`, para la lectura de los objetos individuales de R en forma binaria

---

## Lectura de archivos de datos con `read.table`

La función `read.table` es una de las funciones más utilizadas para la lectura de datos. Cuenta con unos argumentos importantes:

- `file`, el nombre de un archivo, o una conexión
- `header`, lógico indicando si el archivo tiene una línea de cabecera
- `sep`, una cadena que indica cómo se separan las columnas
- `colClasses`, un vector de caracteres que indica la clase de cada columna en el conjunto de datos 
- `nrows`, el número de filas en el conjunto de datos
- `comment.char`, una cadena de caracteres que indica el carácter de comentario
- `skip`, El número de líneas a saltar desde el principio
- `stringsAsFactors`, deben codificarse variables de carácter como factores?

---

## `read.table`

Para los conjuntos de datos pequeños o de tamaño moderado, por lo general puede llamar `read.table` sin especificar cualquier otro argumento

```r
data <- read.table("foo.txt")
```
R automáticamente

- salta las líneas que comienzan con un `#`
- averigua cuántas filas existen (y cuánta memoria tiene que ser asignada)- averigua qué tipo de variable es la ubicada en cada columna de la tabla

Decirle directamente a R todas estas cosas hace a  R funcionar más rápido y más eficientemente.
- `read.csv` es idéntica a read.table excepto que el separador predeterminado es una coma.

---

## `read.csv`


```r
# Lista los archivos en tu directorio
dir()
```

```
## [1] "assets"       "clean.xlsx"   "data"         "index.html"  
## [5] "index.md"     "index.Rmd"    "libraries"    "renamed.xlsx"
```

```r
# Importa swimming_pools.csv: pools
pools <- read.csv("data/swimming_pools.csv")
```

---

## `read.csv`


```r
# Lista los archivos en tu directorio
dir()
```

```
## [1] "assets"       "clean.xlsx"   "data"         "index.html"  
## [5] "index.md"     "index.Rmd"    "libraries"    "renamed.xlsx"
```

```r
# Imprime la estructura de pools 
str(pools)
```

```
## 'data.frame':	20 obs. of  4 variables:
##  $ Name     : Factor w/ 20 levels "Acacia Ridge Leisure Centre",..: 1 2 3 4 5 6 19 7 8 9 ...
##  $ Address  : Factor w/ 20 levels "100 Edmonstone Street, South Brisbane",..: 4 20 18 10 8 11 5 15 12 17 ...
##  $ Latitude : num  -27.6 -27.6 -27.6 -27.5 -27.4 ...
##  $ Longitude: num  153 153 153 153 153 ...
```

---

## `read.table`


```r
# Importa el archivo hotdogs.txt 
hotdogs <- read.table("data/hotdogs.txt",sep = "\t", 
                      col.names = c("type", "calories","sodium"))

# Llama a head() en hotdogs.txt 
head(hotdogs)
```

```
##   type calories sodium
## 1 Beef      186    495
## 2 Beef      181    477
## 3 Beef      176    425
## 4 Beef      149    322
## 5 Beef      184    482
## 6 Beef      190    587
```

---

## La lectura en grandes conjuntos de datos con read.table

Con conjuntos de datos mucho más grandes, hacer las siguientes cosas le hará la vida más fácil y evitará que R se 'ahogue'.

- Lea la página de ayuda para read.table, que contiene muchos consejos
- Hacer un cálculo aproximado de la memoria necesaria para almacenar su conjunto de datos. Si el conjunto de datos es mayor que la cantidad de RAM en su equipo, es probable que usted deba parar aquí.
- Establecer `comment.char =" "` si no hay líneas comentadas en el archivo.

---

## Lectura en grandes conjuntos de datos con read.table

- Utilice el argumento `colClasses`. Especificar esta opción en lugar de utilizar el valor por defecto puede hacer que 'read.table' corra mucho más rápido, a menudo el doble de rápido. Para utilizar esta opción, usted tiene que saber la clase de cada columna en el marco de datos. Si todas las columnas son "numérico", por ejemplo, a continuación, puedes configurar `colClasses =" numérico "`. Una manera rápida de averiguar las clases de cada columna es el siguiente:

```r
initial <- read.table("datatable.txt", nrows = 100)
classes <- sapply(initial, class)
tabAll <- read.table("datatable.txt",
                     colClasses = classes)
```

- Declarar `nrows`. Esto no hace que R funcione más rápido pero ayuda con el uso de memoria. Una sobreestimación suave está bien. Puede utilizar la herramienta `wc` de Unix para calcular el número de líneas en un archivo.

---

## Conocer tu sistema

En general, cuando se utiliza R con grandes conjuntos de datos, es útil saber algunas cosas acerca de su sistema.

- ¿Cuánta memoria está disponible?
- ¿Qué otras aplicaciones están en uso?
- ¿Existen otros usuarios registrados en el mismo sistema?
- qué sistema operativo?
- Es el sistema operativo de 32 o 64 bits?

---

## Cálculo de requisitos de memoria

Suponga que tiene un data frame con 1.500.000 filas y  120 columnas, los cuales son datos numéricos. Usualmente, que cantidad de memoria se requiere para almacenar este data frame?


1,500,000 × 120 × 8 bytes/numeric 

  = 1440000000 bytes
  
  = 1440000000 / $2^{20}$ bytes/MB  
  
  = 1,373.29 MB
  
  = 1.34 GB                     

---

## Importando datos desde `Excel`

- Como sabemos `Excel` es una de las herramientas de análisis de datos más comunes del mercado 

- Muchos paquetes en `R` interactuan con datos de `Excel`

- `readxl` - _Hadley Wickhan_
- `XLConnect` - _Mirai Solutions GmbH_
- `gdata` - _Gregory R. Warnes, et All_

---

## Importando datos desde `Excel`
Antes de importar el documento, es importante encontrar que hojas se encuentran disponibles.


```r
# Carga el paquete 
suppressMessages(suppressWarnings(library(readxl)))
# Encuentra los nombre de las hojas 
sheets <- excel_sheets("data/latitude.xlsx")
# Imprime las hojas
sheets
```

```
## [1] "1700" "1900"
```

```r
class(sheets)
```

```
## [1] "character"
```

---

## Importando datos desde `Excel`
Ahora que ya sabe los nombres es posible empezar a importar los datos de Excel


```r
latitude_1 <- read_excel("data/latitude.xlsx", sheet = 1)

latitude_2 <- read_excel("data/latitude.xlsx", sheet = "1900")

lat_list <- list(latitude_1, latitude_2)

str(lat_list)
```

```
## List of 2
##  $ :Classes 'tbl_df', 'tbl' and 'data.frame':	246 obs. of  2 variables:
##   ..$ country      : chr [1:246] "Afghanistan" "Akrotiri and Dhekelia" "Albania" "Algeria" ...
##   ..$ latitude_1700: num [1:246] 34.6 34.6 41.3 36.7 -14.3 ...
##  $ :Classes 'tbl_df', 'tbl' and 'data.frame':	246 obs. of  2 variables:
##   ..$ country      : chr [1:246] "Afghanistan" "Akrotiri and Dhekelia" "Albania" "Algeria" ...
##   ..$ latitude_1900: num [1:246] 34.6 34.6 41.3 36.7 -14.3 ...
```

---

## Importando datos desde `Excel`
Una forma mas sencilla de hacerlo es utilizando funciones vectorizadas en el proceso 


```r
lat_list <- lapply(excel_sheets("data/latitude.xlsx"), read_excel,
                  path = "latitude.xlsx")
```

```
## Error in sheets_fun(path): Evaluation error: archivo zip 'latitude.xlsx' no pueden ser cargadas.
```

```r
str(lat_list)
```

```
## List of 2
##  $ :Classes 'tbl_df', 'tbl' and 'data.frame':	246 obs. of  2 variables:
##   ..$ country      : chr [1:246] "Afghanistan" "Akrotiri and Dhekelia" "Albania" "Algeria" ...
##   ..$ latitude_1700: num [1:246] 34.6 34.6 41.3 36.7 -14.3 ...
##  $ :Classes 'tbl_df', 'tbl' and 'data.frame':	246 obs. of  2 variables:
##   ..$ country      : chr [1:246] "Afghanistan" "Akrotiri and Dhekelia" "Albania" "Algeria" ...
##   ..$ latitude_1900: num [1:246] 34.6 34.6 41.3 36.7 -14.3 ...
```

---

## Importando datos desde `Excel`
Existen una serie de argumentos en la función `read_excel()` que ayudan a facilitar la carga de los datos.


```r
latitude_3 <- read_excel("data/latitude_nonames.xlsx", sheet = 1, 
                       col_names = FALSE)
latitude_4 <- read_excel("data/latitude_nonames.xlsx", sheet = 2, 
                         col_names = c("country","latitude")) 
summary(latitude_3)
```

```
##      X__1                X__2        
##  Length:246         Min.   :-51.750  
##  Class :character   1st Qu.:  2.557  
##  Mode  :character   Median : 16.755  
##                     Mean   : 18.026  
##                     3rd Qu.: 39.051  
##                     Max.   : 78.000  
##                     NA's   :4
```

---

## Importando datos desde `Excel`


```r
latitude_3 <- read_excel("data/latitude_nonames.xlsx", sheet = 1, 
                       col_names = FALSE)

latitude_4 <- read_excel("data/latitude_nonames.xlsx", sheet = 2, 
                         col_names = c("country", "latitude")) 

summary(latitude_4)
```

```
##    country             latitude      
##  Length:246         Min.   :-51.750  
##  Class :character   1st Qu.:  2.557  
##  Mode  :character   Median : 16.755  
##                     Mean   : 18.026  
##                     3rd Qu.: 39.051  
##                     Max.   : 78.000  
##                     NA's   :4
```

---

## Importando datos desde `Excel`

Se pueden especificar los nombres de columnas para especificar la carga de los datos.


```r
cols <- c("country", paste0("year_", 1960:1966))

pop_b <- read_excel("data/urbanpop_nonames.xlsx", sheet = 1, col_names = cols)

summary(pop_b)
```

```
##    country            year_1960           year_1961        
##  Length:209         Min.   :     3378   Min.   :     1028  
##  Class :character   1st Qu.:    88978   1st Qu.:    70644  
##  Mode  :character   Median :   580675   Median :   570159  
##                     Mean   :  4988124   Mean   :  4991613  
##                     3rd Qu.:  3077228   3rd Qu.:  2807280  
##                     Max.   :126469700   Max.   :129268133  
##                     NA's   :11                             
##    year_1962           year_1963           year_1964        
##  Min.   :     1090   Min.   :     1154   Min.   :     1218  
##  1st Qu.:    74974   1st Qu.:    81870   1st Qu.:    84953  
##  Median :   593968   Median :   619331   Median :   645262  
##  Mean   :  5141592   Mean   :  5303711   Mean   :  5468966  
##  3rd Qu.:  2948396   3rd Qu.:  3148941   3rd Qu.:  3296444  
##  Max.   :131974143   Max.   :134599886   Max.   :137205240  
##                                                             
##    year_1965           year_1966        
##  Min.   :     1281   Min.   :     1349  
##  1st Qu.:    88633   1st Qu.:    93638  
##  Median :   679109   Median :   735139  
##  Mean   :  5637394   Mean   :  5790281  
##  3rd Qu.:  3317422   3rd Qu.:  3418036  
##  Max.   :139663053   Max.   :141962708  
## 
```

---

## Importando datos desde `Excel`

Otro argumento que puede ser de bastante utilidad cuando se trata de leer archivos `Excel` es `skip`, con él se puede especificar el número de filas que se quieren omitir en la carga.


```r
urbanpop_sel <- read_excel("data/urbanpop.xlsx", sheet = 2, 
                           col_names = FALSE, skip = 21)

head(urbanpop_sel, 1)[1,]
```

```
## # A tibble: 1 x 9
##    X__1     X__2     X__3     X__4     X__5     X__6     X__7     X__8
##   <chr>    <dbl>    <dbl>    <dbl>    <dbl>    <dbl>    <dbl>    <dbl>
## 1 Benin 382022.1 411859.5 443013.1 475611.4 515819.5 557937.6 602093.2
## # ... with 1 more variables: X__9 <dbl>
```

---

## Importando datos desde `Excel`

Con el paquete `gadata()` se pueden leer archivos `.xls` de forma similar a la paquete `readxl`.


```r
suppressMessages(suppressWarnings(library(gdata)))

urban_pop <- read.xls("data/urbanpop.xls", sheet = "1967-1974")

head(urban_pop, 3)
```

```
##       country     X1967     X1968     X1969     X1970     X1971     X1972
## 1 Afghanistan 1119067.2 1182159.1 1248900.8 1319848.8 1409001.1 1502401.8
## 2     Albania  621179.8  639964.5  658853.1  677839.1  698932.2  720206.6
## 3     Algeria 4826104.2 5017298.6 5219331.9 5429743.1 5619041.5 5815734.5
##     X1973     X1974
## 1 1598835 1696444.8
## 2  741681  763385.5
## 3 6020647 6235114.4
```

---

## Importando datos desde `Excel`

Las funciones de este paquete se asemeja a la carga de la función `read.table()`.


```r
columns <- c("country", paste0("year_", 1967:1974))
urban_pop <- read.xls("data/urbanpop.xls", sheet = 2,
                      skip = 50, header = FALSE, stringsAsFactors = FALSE,
                      col.names = columns)
head(urban_pop, 3)
```

```
##          country year_1967 year_1968 year_1969 year_1970 year_1971
## 1         Cyprus  231929.7  237831.4  243983.3  250164.5  261213.2
## 2 Czech Republic 6204409.9 6266304.5 6326369.0 6348794.9 6437055.2
## 3        Denmark 3777552.6 3826785.1 3874314.0 3930043.0 3981360.1
##   year_1972 year_1973 year_1974
## 1    272408  283774.9  295379.8
## 2   6572632 6718465.5 6873458.2
## 3   4028248 4076867.3 4120201.4
```

---

## Importando datos desde `Excel`

Cuando se trata de trabajar con `XLconnect`, el primer paso es cargar el libro de trabajo 


```r
suppressMessages(suppressWarnings(library("XLConnect")))

my_book <- loadWorkbook("data/urbanpop.xlsx")

class(my_book)
```

```
## [1] "workbook"
## attr(,"package")
## [1] "XLConnect"
```

---

## Importando datos desde `Excel`

`XLconnect` permite listar las hojas del documento e importarlas directamente al espacio de trabajo


```r
my_book <- loadWorkbook("data/urbanpop.xlsx")

getSheets(my_book)
```

```
## [1] "1960-1966" "1967-1974" "1975-2011"
```

```r
readWorksheet(my_book, "1967-1974")
```

```
##                            country        X1967        X1968        X1969
## 1                      Afghanistan 1.119067e+06 1.182159e+06 1.248901e+06
## 2                          Albania 6.211798e+05 6.399645e+05 6.588531e+05
## 3                          Algeria 4.826104e+06 5.017299e+06 5.219332e+06
## 4                   American Samoa 1.734866e+04 1.799551e+04 1.861868e+04
## 5                          Andorra 1.543962e+04 1.672699e+04 1.808832e+04
## 6                           Angola 7.574963e+05 7.984593e+05 8.412620e+05
## 7              Antigua and Barbuda 2.208625e+04 2.214939e+04 2.218292e+04
## 8                        Argentina 1.775328e+07 1.812410e+07 1.851046e+07
## 9                          Armenia 1.337032e+06 1.392892e+06 1.449641e+06
## 10                           Aruba 2.941472e+04 2.957609e+04 2.973787e+04
## 11                       Australia 9.934404e+06 1.015397e+07 1.041239e+07
## 12                         Austria 4.803149e+06 4.831817e+06 4.852208e+06
## 13                      Azerbaijan 2.446990e+06 2.495725e+06 2.542062e+06
## 14                         Bahamas 9.868390e+04 1.036697e+05 1.084730e+05
## 15                         Bahrain 1.619616e+05 1.663785e+05 1.714590e+05
## 16                      Bangladesh 4.173453e+06 4.484842e+06 4.790505e+06
## 17                        Barbados 8.819371e+04 8.858041e+04 8.902489e+04
## 18                         Belarus 3.556448e+06 3.696854e+06 3.838003e+06
## 19                         Belgium 8.950504e+06 8.999366e+06 9.038506e+06
## 20                          Belize 5.879024e+04 5.971173e+04 6.049220e+04
## 21                           Benin 3.820221e+05 4.118595e+05 4.430131e+05
## 22                         Bermuda 5.200000e+04 5.300000e+04 5.400000e+04
## 23                          Bhutan 1.437897e+04 1.561689e+04 1.694642e+04
## 24                         Bolivia 1.527065e+06 1.575177e+06 1.625173e+06
## 25          Bosnia and Herzegovina 8.516924e+05 8.902697e+05 9.294496e+05
## 26                        Botswana 3.431976e+04 4.057616e+04 4.722223e+04
## 27                          Brazil 4.719352e+07 4.931688e+07 5.148910e+07
## 28                          Brunei 6.128905e+04 6.622218e+04 7.150276e+04
## 29                        Bulgaria 4.019906e+06 4.158186e+06 4.300669e+06
## 30                    Burkina Faso 2.968238e+05 3.086611e+05 3.209607e+05
## 31                         Burundi 7.616560e+04 7.881625e+04 8.135573e+04
## 32                        Cambodia 8.357562e+05 9.263155e+05 1.017799e+06
## 33                        Cameroon 1.157892e+06 1.231243e+06 1.308158e+06
## 34                          Canada 1.510423e+07 1.546449e+07 1.579236e+07
## 35                      Cape Verde 4.724476e+04 4.923400e+04 5.135658e+04
## 36                  Cayman Islands 8.875000e+03 9.002000e+03 9.216000e+03
## 37        Central African Republic 4.303721e+05 4.529338e+05 4.761054e+05
## 38                            Chad 3.315042e+05 3.605791e+05 3.909776e+05
## 39                 Channel Islands 4.329456e+04 4.344349e+04 4.358417e+04
## 40                           Chile 6.606825e+06 6.805959e+06 7.005123e+06
## 41                           China 1.343974e+08 1.368900e+08 1.396005e+08
## 42                        Colombia 1.033119e+07 1.078053e+07 1.123560e+07
## 43                         Comoros 3.978906e+04 4.183902e+04 4.396565e+04
## 44                Congo, Dem. Rep. 5.161472e+06 5.475208e+06 5.802069e+06
## 45                     Congo, Rep. 4.506698e+05 4.733352e+05 4.972107e+05
## 46                      Costa Rica 6.217858e+05 6.499164e+05 6.782539e+05
## 47                   Cote d'Ivoire 1.243350e+06 1.330719e+06 1.424438e+06
## 48                         Croatia 1.608233e+06 1.663051e+06 1.717607e+06
## 49                            Cuba 4.927341e+06 5.032014e+06 5.137260e+06
## 50                          Cyprus 2.319297e+05 2.378314e+05 2.439833e+05
## 51                  Czech Republic 6.204410e+06 6.266305e+06 6.326369e+06
## 52                         Denmark 3.777553e+06 3.826785e+06 3.874314e+06
## 53                        Djibouti 7.778804e+04 8.469435e+04 9.204577e+04
## 54                        Dominica 2.755036e+04 2.952732e+04 3.147562e+04
## 55              Dominican Republic 1.535485e+06 1.625456e+06 1.718315e+06
## 56                         Ecuador 2.059355e+06 2.151395e+06 2.246891e+06
## 57                           Egypt 1.379817e+07 1.424834e+07 1.470386e+07
## 58                     El Salvador 1.345529e+06 1.387218e+06 1.429379e+06
## 59               Equatorial Guinea 7.536450e+04 7.729503e+04 7.844574e+04
## 60                         Eritrea 2.025150e+05 2.121646e+05 2.221863e+05
## 61                         Estonia 8.283882e+05 8.472205e+05 8.662579e+05
## 62                        Ethiopia 2.139904e+06 2.249670e+06 2.365149e+06
## 63                  Faeroe Islands 9.878976e+03 1.017780e+04 1.047732e+04
## 64                            Fiji 1.632216e+05 1.690663e+05 1.749364e+05
## 65                         Finland 2.822234e+06 2.872371e+06 2.908120e+06
## 66                          France 3.486791e+07 3.554830e+07 3.622608e+07
## 67                French Polynesia 5.087720e+04 5.421077e+04 5.768190e+04
## 68                           Gabon 1.380242e+05 1.478459e+05 1.582525e+05
## 69                          Gambia 7.036836e+04 7.628527e+04 8.261546e+04
## 70                         Georgia 1.863610e+06 1.900576e+06 1.938616e+06
## 71                         Germany 5.546852e+07 5.576506e+07 5.625874e+07
## 72                           Ghana 2.219604e+06 2.311442e+06 2.408851e+06
## 73                          Greece 4.300274e+06 4.415310e+06 4.518763e+06
## 74                       Greenland 2.879686e+04 3.040882e+04 3.206093e+04
## 75                         Grenada 3.004680e+04 3.019593e+04 3.031077e+04
## 76                            Guam 4.629560e+04 4.844571e+04 5.065242e+04
## 77                       Guatemala 1.739459e+06 1.802725e+06 1.868309e+06
## 78                          Guinea 5.618868e+05 5.962425e+05 6.304226e+05
## 79                   Guinea-Bissau 8.719596e+04 8.804516e+04 8.932212e+04
## 80                          Guyana 1.979563e+05 2.033071e+05 2.081042e+05
## 81                           Haiti 8.205857e+05 8.567168e+05 8.934834e+05
## 82                        Honduras 6.700552e+05 7.041621e+05 7.396318e+05
## 83                Hong Kong, China 3.236781e+06 3.316190e+06 3.379661e+06
## 84                         Hungary 6.013289e+06 6.079237e+06 6.147720e+06
## 85                         Iceland 1.661399e+05 1.693063e+05 1.717736e+05
## 86                           India 9.936339e+07 1.025948e+08 1.059532e+08
## 87                       Indonesia 1.786885e+07 1.862152e+07 1.940053e+07
## 88                            Iran 1.024223e+07 1.074839e+07 1.127204e+07
## 89                            Iraq 4.785700e+06 5.053788e+06 5.335012e+06
## 90                         Ireland 1.448735e+06 1.472843e+06 1.499153e+06
## 91                     Isle of Man 2.974060e+04 3.041582e+04 3.107182e+04
## 92                          Israel 2.257543e+06 2.323491e+06 2.403561e+06
## 93                           Italy 3.322924e+07 3.369844e+07 3.414982e+07
## 94                         Jamaica 7.040407e+05 7.257254e+05 7.482876e+05
## 95                           Japan 6.997406e+07 7.101819e+07 7.332929e+07
## 96                          Jordan 7.024333e+05 7.513107e+05 7.991228e+05
## 97                      Kazakhstan 6.018757e+06 6.209379e+06 6.396692e+06
## 98                           Kenya 9.424282e+05 1.010199e+06 1.082085e+06
## 99                        Kiribati 9.944575e+03 1.054187e+04 1.115324e+04
## 100                    North Korea 6.359134e+06 6.797010e+06 7.252939e+06
## 101                    South Korea 1.067144e+07 1.142358e+07 1.219746e+07
## 102                         Kuwait 4.812897e+05 5.332849e+05 5.878232e+05
## 103                Kyrgyz Republic 9.987404e+05 1.037698e+06 1.075216e+06
## 104                            Lao 2.214381e+05 2.333150e+05 2.458144e+05
## 105                         Latvia 1.343553e+06 1.374667e+06 1.404423e+06
## 106                        Lebanon 1.253621e+06 1.320402e+06 1.390579e+06
## 107                        Lesotho 7.042371e+04 7.636722e+04 8.253367e+04
## 108                        Liberia 3.145211e+05 3.336211e+05 3.536543e+05
## 109                          Libya 7.048490e+05 7.933851e+05 8.884915e+05
## 110                  Liechtenstein 3.771201e+03 3.835222e+03 3.893073e+03
## 111                      Lithuania 1.415402e+06 1.462854e+06 1.508107e+06
## 112                     Luxembourg 2.442931e+05 2.465394e+05 2.493815e+05
## 113                   Macao, China 2.193452e+05 2.292781e+05 2.376078e+05
## 114                 Macedonia, FYR 6.524718e+05 6.802103e+05 7.086757e+05
## 115                     Madagascar 7.919615e+05 8.337642e+05 8.775250e+05
## 116                         Malawi 2.242118e+05 2.398927e+05 2.565303e+05
## 117                       Malaysia 3.168042e+06 3.324289e+06 3.484442e+06
## 118                       Maldives 1.252289e+04 1.289746e+04 1.330701e+04
## 119                           Mali 7.656009e+05 7.972307e+05 8.302079e+05
## 120                          Malta 2.796928e+05 2.763384e+05 2.730307e+05
## 121               Marshall Islands 8.640897e+03 9.323270e+03 1.007123e+04
## 122                     Mauritania 1.236419e+05 1.367608e+05 1.505604e+05
## 123                      Mauritius 3.058232e+05 3.195152e+05 3.332923e+05
## 124                         Mexico 2.691017e+07 2.808642e+07 2.931700e+07
## 125          Micronesia, Fed. Sts. 1.354285e+04 1.419170e+04 1.477304e+04
## 126                        Moldova 8.569232e+05 8.959091e+05 9.356514e+05
## 127                         Monaco 2.304600e+04 2.323400e+04 2.344800e+04
## 128                       Mongolia 5.089148e+05 5.307544e+05 5.535133e+05
## 129                     Montenegro 1.244879e+05 1.292181e+05 1.340713e+05
## 130                        Morocco 4.639516e+06 4.848380e+06 5.061952e+06
## 131                     Mozambique 4.491451e+05 4.803006e+05 5.127060e+05
## 132                        Myanmar 5.297725e+06 5.512884e+06 5.737830e+06
## 133                        Namibia 1.504638e+05 1.578102e+05 1.656184e+05
## 134                          Nepal 4.268625e+05 4.411255e+05 4.559937e+05
## 135                    Netherlands 7.699643e+06 7.803192e+06 7.917513e+06
## 136                  New Caledonia 4.587712e+04 4.868702e+04 5.183153e+04
## 137                    New Zealand 2.173205e+06 2.204526e+06 2.236624e+06
## 138                      Nicaragua 9.730101e+05 1.022348e+06 1.073928e+06
## 139                          Niger 3.039535e+05 3.295439e+05 3.563980e+05
## 140                        Nigeria 1.131884e+07 1.186224e+07 1.242960e+07
## 141       Northern Mariana Islands 7.518953e+03 8.073316e+03 8.655527e+03
## 142                         Norway 2.297185e+06 2.376327e+06 2.456007e+06
## 143                           Oman 1.682955e+05 1.833677e+05 1.995581e+05
## 144                       Pakistan 1.316562e+07 1.366756e+07 1.419101e+07
## 145                          Palau 6.521346e+03 6.627161e+03 6.736073e+03
## 146                         Panama 6.330562e+05 6.609825e+05 6.897512e+05
## 147               Papua New Guinea 1.626460e+05 1.865556e+05 2.117910e+05
## 148                       Paraguay 8.397317e+05 8.662660e+05 8.931292e+05
## 149                           Peru 6.560955e+06 6.884271e+06 7.220337e+06
## 150                    Philippines 1.045064e+07 1.085199e+07 1.126489e+07
## 151                         Poland 1.628965e+07 1.657536e+07 1.683567e+07
## 152                       Portugal 3.340476e+06 3.360472e+06 3.364395e+06
## 153                    Puerto Rico 1.435077e+06 1.480203e+06 1.529021e+06
## 154                          Qatar 7.500451e+04 8.116982e+04 8.804065e+04
## 155                        Romania 7.568698e+06 7.775433e+06 7.962558e+06
## 156                         Russia 7.677947e+07 7.832602e+07 7.988771e+07
## 157                         Rwanda 1.005126e+05 1.065866e+05 1.129610e+05
## 158            St. Kitts and Nevis 1.516557e+04 1.522598e+04 1.528050e+04
## 159                      St. Lucia 2.232508e+04 2.291663e+04 2.351565e+04
## 160 St. Vincent and the Grenadines 2.564178e+04 2.633043e+04 2.703429e+04
## 161                          Samoa 2.636036e+04 2.727841e+04 2.815593e+04
## 162                     San Marino 1.030941e+04 1.071427e+04 1.109522e+04
## 163          Sao Tome and Principe 1.684635e+04 1.841719e+04 2.006490e+04
## 164                   Saudi Arabia 2.195007e+06 2.382635e+06 2.586258e+06
## 165                        Senegal 1.035987e+06 1.096955e+06 1.161241e+06
## 166                         Serbia 2.505613e+06 2.595006e+06 2.683242e+06
## 167                     Seychelles 1.771880e+04 1.876104e+04 1.983538e+04
## 168                   Sierra Leone 5.281695e+05 5.535685e+05 5.797787e+05
## 169                      Singapore 1.978000e+06 2.012000e+06 2.043000e+06
## 170                Slovak Republic 1.719618e+06 1.768967e+06 1.818929e+06
## 171                       Slovenia 5.795047e+05 6.000206e+05 6.187531e+05
## 172                Solomon Islands 1.151482e+04 1.237527e+04 1.329659e+04
## 173                        Somalia 7.047038e+05 7.433007e+05 7.810217e+05
## 174                   South Africa 9.830232e+06 1.006591e+07 1.030848e+07
## 175                          Spain 2.064974e+07 2.123678e+07 2.176544e+07
## 176                      Sri Lanka 2.151152e+06 2.249555e+06 2.344592e+06
## 177                          Sudan 1.466502e+06 1.571927e+06 1.683562e+06
## 178                       Suriname 1.638993e+05 1.673102e+05 1.698198e+05
## 179                      Swaziland 3.199762e+04 3.554773e+04 3.929612e+04
## 180                         Sweden 6.187907e+06 6.285731e+06 6.393453e+06
## 181                    Switzerland 3.324087e+06 3.404449e+06 3.481651e+06
## 182                          Syria 2.377889e+06 2.499429e+06 2.626816e+06
## 183                     Tajikistan 9.611929e+05 1.000669e+06 1.041608e+06
## 184                       Tanzania 8.384494e+05 9.108258e+05 9.872961e+05
## 185                       Thailand 6.919690e+06 7.176231e+06 7.440174e+06
## 186                    Timor-Leste 6.802067e+04 7.108209e+04 7.435281e+04
## 187                           Togo 3.221940e+05 3.621139e+05 4.040164e+05
## 188                          Tonga 1.563131e+04 1.614767e+04 1.661674e+04
## 189            Trinidad and Tobago 1.232921e+05 1.208498e+05 1.181071e+05
## 190                        Tunisia 1.992479e+06 2.070869e+06 2.149857e+06
## 191                         Turkey 1.191986e+07 1.244807e+07 1.299329e+07
## 192                   Turkmenistan 9.517698e+05 9.822601e+05 1.013434e+06
## 193       Turks and Caicos Islands 2.798837e+03 2.804887e+03 2.829033e+03
## 194                         Tuvalu 1.415014e+03 1.480186e+03 1.545270e+03
## 195                         Uganda 5.120829e+05 5.499091e+05 5.891064e+05
## 196                        Ukraine 2.416635e+07 2.475757e+07 2.534887e+07
## 197           United Arab Emirates 1.280378e+05 1.390527e+05 1.555970e+05
## 198                 United Kingdom 4.260294e+07 4.273308e+07 4.283308e+07
## 199                  United States 1.442017e+08 1.463404e+08 1.484759e+08
## 200                        Uruguay 2.247503e+06 2.273438e+06 2.295858e+06
## 201                     Uzbekistan 3.913188e+06 4.067599e+06 4.227790e+06
## 202                        Vanuatu 9.208354e+03 9.621427e+03 1.005774e+04
## 203                      Venezuela 6.678933e+06 6.994264e+06 7.324840e+06
## 204                        Vietnam 6.865532e+06 7.169607e+06 7.487421e+06
## 205          Virgin Islands (U.S.) 3.342853e+04 3.661847e+04 4.004103e+04
## 206                          Yemen 6.973814e+05 7.369436e+05 7.769681e+05
## 207                         Zambia 9.841980e+05 1.069557e+06 1.160044e+06
## 208                       Zimbabwe 7.416051e+05 7.927728e+05 8.467739e+05
## 209                    South Sudan 3.157901e+05 3.210970e+05 3.268101e+05
##            X1970        X1971        X1972        X1973        X1974
## 1   1.319849e+06 1.409001e+06 1.502402e+06 1.598835e+06 1.696445e+06
## 2   6.778391e+05 6.989322e+05 7.202066e+05 7.416810e+05 7.633855e+05
## 3   5.429743e+06 5.619042e+06 5.815734e+06 6.020647e+06 6.235114e+06
## 4   1.920639e+04 1.975202e+04 2.026267e+04 2.074197e+04 2.119438e+04
## 5   1.952896e+04 2.092873e+04 2.240584e+04 2.393705e+04 2.548198e+04
## 6   8.864016e+05 9.550101e+05 1.027397e+06 1.103830e+06 1.184486e+06
## 7   2.218087e+04 2.256087e+04 2.290776e+04 2.322129e+04 2.350292e+04
## 8   1.891807e+07 1.932972e+07 1.976308e+07 2.021142e+07 2.066473e+07
## 9   1.507620e+06 1.564368e+06 1.622104e+06 1.680498e+06 1.739063e+06
## 10  2.990157e+04 3.008136e+04 3.027976e+04 3.046742e+04 3.060287e+04
## 11  1.066409e+07 1.104771e+07 1.126995e+07 1.146112e+07 1.177293e+07
## 12  4.872871e+06 4.895910e+06 4.925699e+06 4.954325e+06 4.964026e+06
## 13  2.586413e+06 2.660993e+06 2.734825e+06 2.807955e+06 2.880447e+06
## 14  1.130101e+05 1.171566e+05 1.209989e+05 1.246644e+05 1.283499e+05
## 15  1.775008e+05 1.844398e+05 1.923163e+05 2.014935e+05 2.124162e+05
## 16  5.078286e+06 5.456170e+06 5.812548e+06 6.161815e+06 6.530579e+06
## 17  8.956543e+04 9.055245e+04 9.164208e+04 9.277639e+04 9.387156e+04
## 18  3.978504e+06 4.132164e+06 4.286801e+06 4.440936e+06 4.592935e+06
## 19  9.061057e+06 9.089909e+06 9.137946e+06 9.179155e+06 9.220531e+06
## 20  6.114133e+04 6.183991e+04 6.240329e+04 6.294338e+04 6.362671e+04
## 21  4.756114e+05 5.158195e+05 5.579376e+05 6.020932e+05 6.484097e+05
## 22  5.500000e+04 5.460000e+04 5.420000e+04 5.380000e+04 5.340000e+04
## 23  1.838141e+04 2.017266e+04 2.209976e+04 2.415974e+04 2.634254e+04
## 24  1.677184e+06 1.731437e+06 1.787719e+06 1.845894e+06 1.905749e+06
## 25  9.695495e+05 1.008630e+06 1.048738e+06 1.089648e+06 1.130966e+06
## 26  5.428641e+04 6.186900e+04 6.992963e+04 7.852997e+04 8.775392e+04
## 27  5.371642e+07 5.600051e+07 5.834048e+07 6.074473e+07 6.322438e+07
## 28  7.714802e+04 8.088400e+04 8.478142e+04 8.880798e+04 9.291945e+04
## 29  4.440047e+06 4.554372e+06 4.665864e+06 4.780947e+06 4.904324e+06
## 30  3.336985e+05 3.475107e+05 3.618362e+05 3.767243e+05 3.922410e+05
## 31  8.369155e+04 9.049313e+04 9.717071e+04 1.038732e+05 1.108747e+05
## 32  1.107998e+06 9.614523e+05 8.076237e+05 6.470452e+05 4.811320e+05
## 33  1.388878e+06 1.523689e+06 1.665342e+06 1.814545e+06 1.972201e+06
## 34  1.613246e+07 1.637385e+07 1.663528e+07 1.691758e+07 1.722167e+07
## 35  5.364682e+04 5.638241e+04 5.931521e+04 6.221562e+04 6.475257e+04
## 36  9.545000e+03 1.000400e+04 1.058100e+04 1.125300e+04 1.199000e+04
## 37  4.997496e+05 5.268630e+05 5.546158e+05 5.832534e+05 6.131560e+05
## 38  4.229151e+05 4.628673e+05 5.049060e+05 5.488032e+05 5.940966e+05
## 39  4.371195e+04 4.368323e+04 4.363962e+04 4.355859e+04 4.341204e+04
## 40  7.204920e+06 7.398470e+06 7.592419e+06 7.785880e+06 7.977602e+06
## 41  1.423868e+08 1.463523e+08 1.499932e+08 1.534576e+08 1.566609e+08
## 42  1.169300e+07 1.214719e+07 1.260270e+07 1.306371e+07 1.353659e+07
## 43  4.615440e+04 4.811136e+04 5.012270e+04 5.227286e+04 5.468356e+04
## 44  6.140904e+06 6.282834e+06 6.425372e+06 6.570538e+06 6.721175e+06
## 45  5.224066e+05 5.497894e+05 5.786398e+05 6.088504e+05 6.402364e+05
## 46  7.067986e+05 7.335459e+05 7.604308e+05 7.879183e+05 8.166588e+05
## 47  1.525425e+06 1.638738e+06 1.760508e+06 1.891241e+06 2.031395e+06
## 48  1.773046e+06 1.826422e+06 1.879428e+06 1.932436e+06 1.984976e+06
## 49  5.244279e+06 5.407254e+06 5.572975e+06 5.738231e+06 5.898512e+06
## 50  2.501645e+05 2.612132e+05 2.724080e+05 2.837749e+05 2.953798e+05
## 51  6.348795e+06 6.437055e+06 6.572632e+06 6.718466e+06 6.873458e+06
## 52  3.930043e+06 3.981360e+06 4.028248e+06 4.076867e+06 4.120201e+06
## 53  9.984522e+04 1.077997e+05 1.160982e+05 1.253916e+05 1.366062e+05
## 54  3.332825e+04 3.476152e+04 3.604999e+04 3.726005e+04 3.850147e+04
## 55  1.814060e+06 1.915590e+06 2.020157e+06 2.127714e+06 2.238204e+06
## 56  2.345864e+06 2.453818e+06 2.565645e+06 2.681525e+06 2.801693e+06
## 57  1.516286e+07 1.560366e+07 1.604781e+07 1.649863e+07 1.696083e+07
## 58  1.472181e+06 1.527985e+06 1.584758e+06 1.642099e+06 1.699471e+06
## 59  7.841107e+04 7.705529e+04 7.459606e+04 7.143896e+04 6.817926e+04
## 60  2.325927e+05 2.420318e+05 2.517894e+05 2.620127e+05 2.729047e+05
## 61  8.847697e+05 9.015668e+05 9.191148e+05 9.354101e+05 9.510326e+05
## 62  2.487032e+06 2.609266e+06 2.738496e+06 2.870320e+06 2.998291e+06
## 63  1.077427e+04 1.106567e+04 1.135462e+04 1.164494e+04 1.194279e+04
## 64  1.809345e+05 1.868715e+05 1.929448e+05 1.991372e+05 2.054102e+05
## 65  2.934402e+06 2.976176e+06 3.032239e+06 3.088022e+06 3.142947e+06
## 66  3.691751e+07 3.740758e+07 3.790747e+07 3.840573e+07 3.888504e+07
## 67  6.125900e+04 6.368624e+04 6.613374e+04 6.861999e+04 7.117748e+04
## 68  1.694483e+05 1.845557e+05 2.007952e+05 2.181618e+05 2.365466e+05
## 69  8.942094e+04 9.676352e+04 1.047188e+05 1.132281e+05 1.221660e+05
## 70  1.904782e+06 1.943501e+06 2.058124e+06 2.096168e+06 2.134461e+06
## 71  5.649607e+07 5.664462e+07 5.696131e+07 5.718614e+07 5.725360e+07
## 72  2.515296e+06 2.601135e+06 2.695926e+06 2.795186e+06 2.892229e+06
## 73  4.616575e+06 4.686154e+06 4.766545e+06 4.838297e+06 4.906384e+06
## 74  3.375322e+04 3.449046e+04 3.545317e+04 3.612819e+04 3.665970e+04
## 75  3.040587e+04 3.039084e+04 3.037836e+04 3.034479e+04 3.025489e+04
## 76  5.291621e+04 5.791466e+04 6.308539e+04 6.843879e+04 7.399464e+04
## 77  1.936380e+06 2.002850e+06 2.071676e+06 2.142378e+06 2.214270e+06
## 78  6.636291e+05 7.000651e+05 7.353800e+05 7.696670e+05 8.032624e+05
## 79  9.123325e+04 9.389158e+04 9.722136e+04 1.011893e+05 1.057146e+05
## 80  2.120772e+05 2.155336e+05 2.181112e+05 2.201426e+05 2.221226e+05
## 81  9.307198e+05 9.535772e+05 9.764460e+05 9.996672e+05 1.023722e+06
## 82  7.769459e+05 8.163257e+05 8.577454e+05 9.014120e+05 9.475283e+05
## 83  3.473191e+06 3.564807e+06 3.650021e+06 3.771147e+06 3.870519e+06
## 84  6.214324e+06 6.276071e+06 6.338877e+06 6.403550e+06 6.476603e+06
## 85  1.735679e+05 1.757064e+05 1.790372e+05 1.825107e+05 1.857581e+05
## 86  1.094455e+08 1.137519e+08 1.182288e+08 1.228790e+08 1.277043e+08
## 87  2.020553e+07 2.127053e+07 2.237329e+07 2.351361e+07 2.469105e+07
## 88  1.181219e+07 1.239191e+07 1.299286e+07 1.362195e+07 1.428880e+07
## 89  5.627633e+06 5.924798e+06 6.232252e+06 6.551369e+06 6.884387e+06
## 90  1.529549e+06 1.558990e+06 1.593945e+06 1.631517e+06 1.670769e+06
## 91  3.166567e+04 3.182827e+04 3.189547e+04 3.190477e+04 3.190731e+04
## 92  2.503959e+06 2.598970e+06 2.681284e+06 2.808059e+06 2.909400e+06
## 93  3.459238e+07 3.490238e+07 3.525021e+07 3.564021e+07 3.602531e+07
## 94  7.723456e+05 7.935444e+05 8.162612e+05 8.398898e+05 8.633533e+05
## 95  7.500006e+07 7.678337e+07 7.868950e+07 8.017343e+07 8.256444e+07
## 96  8.440427e+05 8.861825e+05 9.252900e+05 9.628976e+05 1.001686e+06
## 97  6.585936e+06 6.756162e+06 6.928193e+06 7.100036e+06 7.268241e+06
## 98  1.158426e+06 1.261182e+06 1.370525e+06 1.486815e+06 1.610388e+06
## 99  1.177903e+04 1.253191e+04 1.329569e+04 1.407663e+04 1.488213e+04
## 100 7.721750e+06 8.009574e+06 8.299056e+06 8.584095e+06 8.857069e+06
## 101 1.299394e+07 1.374559e+07 1.451567e+07 1.530510e+07 1.611498e+07
## 102 6.451490e+05 7.009110e+05 7.585954e+05 8.180756e+05 8.792009e+05
## 103 1.108956e+06 1.136687e+06 1.165919e+06 1.195227e+06 1.226436e+06
## 104 2.590287e+05 2.739823e+05 2.898053e+05 3.060341e+05 3.219629e+05
## 105 1.432319e+06 1.459146e+06 1.487488e+06 1.516637e+06 1.546838e+06
## 106 1.465634e+06 1.541721e+06 1.622874e+06 1.705275e+06 1.783166e+06
## 107 8.892443e+04 9.542557e+04 1.021606e+05 1.091860e+05 1.165855e+05
## 108 3.746759e+05 3.980213e+05 4.225051e+05 4.482161e+05 4.752605e+05
## 109 9.904397e+05 1.087657e+06 1.191671e+06 1.302852e+06 1.421573e+06
## 110 3.941192e+03 4.016945e+03 4.084375e+03 4.146087e+03 4.206141e+03
## 111 1.555873e+06 1.614349e+06 1.671308e+06 1.727112e+06 1.782930e+06
## 112 2.522550e+05 2.566740e+05 2.618327e+05 2.667899e+05 2.723674e+05
## 113 2.435455e+05 2.467800e+05 2.476067e+05 2.466418e+05 2.448335e+05
## 114 7.381837e+05 7.584522e+05 7.793806e+05 8.010906e+05 8.237298e+05
## 115 9.233980e+05 9.783692e+05 1.035964e+06 1.096280e+06 1.159402e+06
## 116 2.742784e+05 2.974752e+05 3.221866e+05 3.484584e+05 3.762949e+05
## 117 3.649615e+06 3.835042e+06 4.026657e+06 4.224277e+06 4.427442e+06
## 118 1.376876e+04 1.548045e+04 1.732799e+04 1.930163e+04 2.137255e+04
## 119 8.646754e+05 9.031346e+05 9.433393e+05 9.851630e+05 1.028372e+06
## 120 2.714740e+05 2.715449e+05 2.713466e+05 2.711483e+05 2.709913e+05
## 121 1.091076e+04 1.170290e+04 1.258814e+04 1.354212e+04 1.452511e+04
## 122 1.650886e+05 1.839591e+05 2.038400e+05 2.247698e+05 2.467774e+05
## 123 3.471843e+05 3.551136e+05 3.629438e+05 3.708224e+05 3.789698e+05
## 124 3.061321e+07 3.194150e+07 3.333305e+07 3.478046e+07 3.627178e+07
## 125 1.523980e+04 1.553743e+04 1.571629e+04 1.584482e+04 1.602333e+04
## 126 9.764706e+05 1.015915e+06 1.056411e+06 1.097293e+06 1.137827e+06
## 127 2.368900e+04 2.396800e+04 2.428200e+04 2.460500e+04 2.490200e+04
## 128 5.773571e+05 6.041172e+05 6.320703e+05 6.610724e+05 6.908953e+05
## 129 1.392938e+05 1.454891e+05 1.521163e+05 1.591069e+05 1.663149e+05
## 130 5.278427e+06 5.516718e+06 5.759042e+06 6.006727e+06 6.261899e+06
## 131 5.464057e+05 6.150199e+05 6.864334e+05 7.611387e+05 8.399119e+05
## 132 5.973271e+06 6.178716e+06 6.392781e+06 6.613581e+06 6.838424e+06
## 133 1.739636e+05 1.814829e+05 1.894921e+05 1.977924e+05 2.060961e+05
## 134 4.714710e+05 5.035432e+05 5.369944e+05 5.718580e+05 6.081574e+05
## 135 8.039946e+06 8.176234e+06 8.299848e+06 8.409656e+06 8.516996e+06
## 136 5.533056e+04 5.909833e+04 6.291106e+04 6.663068e+04 7.014487e+04
## 137 2.279646e+06 2.323472e+06 2.374612e+06 2.431429e+06 2.492750e+06
## 138 1.127855e+06 1.171246e+06 1.216288e+06 1.263026e+06 1.311513e+06
## 139 3.845578e+05 4.198226e+05 4.568167e+05 4.956246e+05 5.363483e+05
## 140 1.302354e+07 1.367088e+07 1.434773e+07 1.506111e+07 1.582041e+07
## 141 9.250286e+03 9.855667e+03 1.050168e+04 1.115197e+04 1.175108e+04
## 142 2.534594e+06 2.574218e+06 2.615935e+06 2.656406e+06 2.695182e+06
## 143 2.170597e+05 2.378383e+05 2.603733e+05 2.850917e+05 3.125531e+05
## 144 1.473699e+07 1.533278e+07 1.595552e+07 1.661011e+07 1.730286e+07
## 145 6.855879e+03 6.993553e+03 7.145486e+03 7.295512e+03 7.421072e+03
## 146 7.192792e+05 7.438996e+05 7.689286e+05 7.943853e+05 8.203103e+05
## 147 2.385030e+05 2.558776e+05 2.743358e+05 2.938021e+05 3.141259e+05
## 148 9.201416e+05 9.528178e+05 9.860213e+05 1.020057e+06 1.055359e+06
## 149 7.570234e+06 7.894058e+06 8.229659e+06 8.577138e+06 8.936488e+06
## 150 1.169151e+07 1.222076e+07 1.276980e+07 1.333929e+07 1.392968e+07
## 151 1.702627e+07 1.729526e+07 1.764742e+07 1.801889e+07 1.840518e+07
## 152 3.368354e+06 3.388266e+06 3.417132e+06 3.452290e+06 3.535363e+06
## 153 1.585301e+06 1.635614e+06 1.693250e+06 1.755806e+06 1.818827e+06
## 154 9.580697e+04 1.046010e+05 1.144858e+05 1.249279e+05 1.351680e+05
## 155 8.164758e+06 8.352698e+06 8.536653e+06 8.714774e+06 8.901463e+06
## 156 8.146468e+07 8.297123e+07 8.449242e+07 8.602837e+07 8.757920e+07
## 157 1.196576e+05 1.296515e+05 1.401857e+05 1.513041e+05 1.630587e+05
## 158 1.532931e+04 1.530592e+04 1.531596e+04 1.529062e+04 1.526421e+04
## 159 2.424170e+04 2.484224e+04 2.542559e+04 2.606504e+04 2.668730e+04
## 160 2.775738e+04 2.852298e+04 2.931059e+04 3.011692e+04 3.093551e+04
## 161 2.897331e+04 2.960049e+04 3.015656e+04 3.065566e+04 3.112000e+04
## 162 1.144333e+04 1.199178e+04 1.250465e+04 1.300464e+04 1.352865e+04
## 163 2.173410e+04 2.255666e+04 2.335055e+04 2.415061e+04 2.501460e+04
## 164 2.809100e+06 3.050817e+06 3.315971e+06 3.607779e+06 3.929807e+06
## 165 1.228874e+06 1.300559e+06 1.375866e+06 1.453826e+06 1.533013e+06
## 166 2.770952e+06 2.834711e+06 2.898614e+06 2.962223e+06 3.025922e+06
## 167 2.094045e+04 2.221236e+04 2.351875e+04 2.485369e+04 2.620824e+04
## 168 6.067908e+05 6.355432e+05 6.652061e+05 6.959255e+05 7.279029e+05
## 169 2.075000e+06 2.113000e+06 2.152000e+06 2.193000e+06 2.230000e+06
## 170 1.863258e+06 1.918549e+06 1.982845e+06 2.050451e+06 2.120507e+06
## 171 6.382787e+05 6.619232e+05 6.860343e+05 7.106715e+05 7.335425e+05
## 172 1.429003e+04 1.487728e+04 1.550905e+04 1.617813e+04 1.687314e+04
## 173 8.166815e+05 8.475888e+05 8.745210e+05 9.078108e+05 9.626845e+05
## 174 1.055957e+07 1.081953e+07 1.108419e+07 1.135223e+07 1.162297e+07
## 175 2.233044e+07 2.282103e+07 2.327235e+07 2.373034e+07 2.420854e+07
## 176 2.441982e+06 2.475540e+06 2.508101e+06 2.552143e+06 2.588945e+06
## 177 1.802344e+06 1.912728e+06 2.030472e+06 2.155450e+06 2.287267e+06
## 178 1.710630e+05 1.743836e+05 1.764727e+05 1.777444e+05 1.788532e+05
## 179 4.325858e+04 4.845133e+04 5.395107e+04 5.977098e+04 6.591935e+04
## 180 6.517403e+06 6.589874e+06 6.636926e+06 6.675974e+06 6.723052e+06
## 181 3.545846e+06 3.564515e+06 3.591810e+06 3.618437e+06 3.637988e+06
## 182 2.760217e+06 2.878588e+06 3.002034e+06 3.130344e+06 3.263171e+06
## 183 1.084708e+06 1.111673e+06 1.139645e+06 1.168044e+06 1.196054e+06
## 184 1.068227e+06 1.195298e+06 1.330036e+06 1.472583e+06 1.622882e+06
## 185 7.711257e+06 8.156822e+06 8.618420e+06 9.093762e+06 9.579568e+06
## 186 7.788066e+04 8.202655e+04 8.651331e+04 9.088243e+04 9.445747e+04
## 187 4.462997e+05 4.679159e+05 4.881497e+05 5.073627e+05 5.262916e+05
## 188 1.703157e+04 1.728917e+04 1.748268e+04 1.763734e+04 1.779015e+04
## 189 1.149191e+05 1.151237e+05 1.150568e+05 1.148504e+05 1.146878e+05
## 190 2.229322e+06 2.307379e+06 2.389032e+06 2.475875e+06 2.569238e+06
## 191 1.355938e+07 1.410119e+07 1.466411e+07 1.524684e+07 1.584676e+07
## 192 1.045665e+06 1.075185e+06 1.105506e+06 1.136380e+06 1.167443e+06
## 193 2.878290e+03 2.961101e+03 3.073893e+03 3.205822e+03 3.342540e+03
## 194 1.611030e+03 1.683666e+03 1.756818e+03 1.830905e+03 1.905153e+03
## 195 6.294769e+05 6.557359e+05 6.822662e+05 7.093838e+05 7.375558e+05
## 196 2.594411e+07 2.648578e+07 2.703029e+07 2.757233e+07 2.810411e+07
## 197 1.800752e+05 2.128010e+05 2.533435e+05 3.021131e+05 3.593418e+05
## 198 4.292583e+07 4.316876e+07 4.337887e+07 4.352637e+07 4.361748e+07
## 199 1.509224e+08 1.528638e+08 1.545305e+08 1.560341e+08 1.574881e+08
## 200 2.313813e+06 2.326524e+06 2.334879e+06 2.341153e+06 2.348533e+06
## 201 4.395765e+06 4.595966e+06 4.805551e+06 5.022305e+06 5.242853e+06
## 202 1.052469e+04 1.103796e+04 1.158368e+04 1.215890e+04 1.275908e+04
## 203 7.674281e+06 8.023652e+06 8.391094e+06 8.777606e+06 9.184011e+06
## 204 7.819407e+06 8.043735e+06 8.277023e+06 8.518466e+06 8.766839e+06
## 205 4.384296e+04 5.021305e+04 5.460843e+04 6.130639e+04 6.670296e+04
## 206 8.172839e+05 8.485446e+05 8.800627e+05 9.133326e+05 9.504883e+05
## 207 1.256178e+06 1.337898e+06 1.424498e+06 1.515871e+06 1.611725e+06
## 208 9.039055e+05 9.620288e+05 1.023588e+06 1.088377e+06 1.155992e+06
## 209 3.330133e+05 3.396491e+05 3.466912e+05 3.542318e+05 3.623528e+05
```

---


## Importando datos desde `Excel`

`XLconnect` permite personalizar la carga de los datos proveniente de las hojas del documentos `Excel`


```r
my_book <- loadWorkbook("data/urbanpop.xlsx")

urbanpop_sel <- readWorksheet(my_book, sheet = 2, startCol = 3, endCol = 5)

countries <- readWorksheet(my_book, sheet = 2, startCol= 1, endCol= 1)

selection <- cbind(countries, urbanpop_sel)
```

---

## Importando datos desde `Excel`

Se pueden añadir más hojas de trabajo


```r
my_book <- loadWorkbook("data/urbanpop.xlsx")

createSheet(my_book, name = "data_summary")

getSheets(my_book)
```

```
## [1] "1960-1966"    "1967-1974"    "1975-2011"    "data_summary"
```

---

## Importando datos desde `Excel`

Se pueden renombrar las hojas de trabajo


```r
my_book <- loadWorkbook("data/urbanpop.xlsx")

renameSheet(my_book, "data_summary", "summary")
```

```
## Error: IllegalArgumentException (Java): Sheet index (-1) is out of range (0..2)
```

```r
getSheets(my_book)
```

```
## [1] "1960-1966" "1967-1974" "1975-2011"
```

```r
saveWorkbook(my_book, "renamed.xlsx")
```

---

## Importando datos desde `Excel`

Se pueden eliminar las hojas de trabajo


```r
my_book <- loadWorkbook("data/urbanpop.xlsx")

removeSheet(my_book, "summary")

saveWorkbook(my_book, "clean.xlsx")
```

---

## Get/set el directorio de trabajo

* Un componente básico del trabajo con datos es el conocer su directorio de trabajo
* Los comandos principales son ```getwd()``` y ```setwd()```. 
* Tenga en cuenta
  * __Relativos__ - ```setwd("./data")```, ```setwd("../")```
  * __Absolutos__ - ```setwd("/Users/su_nombre/data/")```
* Importantes diferencias con Windows  ```setwd("C:\\Users\\su_nombre\\Downloads")```

---

## Revisar la creación de directorios 

* ```file.exists("Nombre_directorio")``` confirmara si el directorio existe
* ```dir.create("Nombre_directorio")``` Creara el directorio sino existe 
* Aquí un ejemplo para la verificación y creación de un directorio "data" en caso de no existir. 


```r
if (!file.exists("data")) {
  dir.create("data")
}
```

---

## Descargando datos de la web - `download.file()`

* Descarga una archivo de la web
* Incluso si se puede hacer a mano, ayuda con la reproducibilidad 
* Parámetros importantes son _url_, _destfile_, _method_
* Útil para descargar archivo delimitados por _tab_, _csv_, y otros archivos


---

## Descargando datos de la web


```r
#fileUrl <- "https://data.baltimorecity.gov/api/views/dz54-2aru/rows.csv?accessType=DOWNLOAD"
#download.file(fileUrl,destfile="./cameras.csv",method="curl")
#list.files()
#dateDownloaded <- date()
#dateDownloaded
```

---

## Algunas recomendaciones para `download.file()`

* Si la url comienza con _http_ se pude usar `download.file()`
* Si la url comienza con _https_ en Windows puede funcionar
* Si la url comienza con _https_ en Mac necesitas configurar el _method="curl"_
* Si el archivo es grande, esto puede tomar tiempo
* Asegúrate de revisar si la descarga fue exitosa.

---

## Existe un paquete para eso 

* en general la mejor forma de encontrar un paquete en `R` es en Google "Herramienta R package" 

* Por ejemplo: "MySQL R package"

---

## Interactuando con archivos directamente 

* file - abre una conexión a un archivo de texto
* url - abre una conexión a una url
* gzfile - abre una conexión a un archivo .gz
* bzfile - abre una conexión a un archivo .bz2 
* _?connections_ Para más información
* <redtext> Recuerde cerrar las conexiones siempre </redtext>

---

## paquete foreign

* Carga datos de Minitab, S, SAS, SPSS, Stata,Systat
* Funciones básicas _read.foo_
  * read.arff (Weka)
  * read.dta (Stata)
  * read.mtp (Minitab)
  * read.octave (Octave)
  * read.spss (SPSS)
  * read.xport (SAS)
* Vea la pagina de ayuda para más detalles [http://cran.r-project.org/web/packages/foreign/foreign.pdf](http://cran.r-project.org/web/packages/foreign/foreign.pdf)


---

## Ejemplos de paquetes para bases de datos 

* RPostresSQL provee una conexión a la base de datos tipo DBI-complaint. Tutorial-[https://code.google.com/p/rpostgresql/](https://code.google.com/p/rpostgresql/), archivo de ayuda-[http://cran.r-project.org/web/packages/RPostgreSQL/RPostgreSQL.pdf](http://cran.r-project.org/web/packages/RPostgreSQL/RPostgreSQL.pdf)
* RODBC provee interfaces a múltiples bases de datos incluyendo PostgreQL, MySQL, Microsoft Access y SQLite. Tutorial - [http://cran.r-project.org/web/packages/RODBC/vignettes/RODBC.pdf](http://cran.r-project.org/web/packages/RODBC/vignettes/RODBC.pdf), archivo de ayuda - [http://cran.r-project.org/web/packages/RODBC/RODBC.pdf](http://cran.r-project.org/web/packages/RODBC/RODBC.pdf)
* RMongo [http://cran.r-project.org/web/packages/RMongo/RMongo.pdf](http://cran.r-project.org/web/packages/RMongo/RMongo.pdf) (ejemplo de Rmongo [http://www.r-bloggers.com/r-and-mongodb/](http://www.r-bloggers.com/r-and-mongodb/)) y [rmongodb](http://cran.r-project.org/web/packages/rmongodb/rmongodb.pdf) provee interfaces a MongoDb. 


---

## Leyendo imagenes 

- jpeg - [http://cran.r-project.org/web/packages/jpeg/index.html](http://cran.r-project.org/web/packages/jpeg/index.html)
- readbitmap - [http://cran.r-project.org/web/packages/readbitmap/index.html](http://cran.r-project.org/web/packages/readbitmap/index.html)
- png - [http://cran.r-project.org/web/packages/png/index.html](http://cran.r-project.org/web/packages/png/index.html)
- EBImage (Bioconductor) - [http://www.bioconductor.org/packages/2.13/bioc/html/EBImage.html](http://www.bioconductor.org/packages/2.13/bioc/html/EBImage.html)

---

## Leyendo datos tipo GIS 

- rgdal - [http://cran.r-project.org/web/packages/rgdal/index.html](http://cran.r-project.org/web/packages/rgdal/index.html)
- rgeos - [http://cran.r-project.org/web/packages/rgeos/index.html](http://cran.r-project.org/web/packages/rgeos/index.html)
- raster - [http://cran.r-project.org/web/packages/raster/index.html](http://cran.r-project.org/web/packages/raster/index.html)

---

## Leyendo datos de musica

- tuneR - [http://cran.r-project.org/web/packages/tuneR/](http://cran.r-project.org/web/packages/tuneR/)
- seewave - [http://rug.mnhn.fr/seewave/](http://rug.mnhn.fr/seewave/)

---
