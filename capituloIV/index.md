---
title       : Programación en R
subtitle    : 
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

## Motivación 

### **Que hace este código?**

<img class=center src= assets/img/img1.png height=450/>

---

## Motivación 

### **Que hace este código?**

<img class=center src= assets/img/img2.png height=450/>

---

## Motivación 

### **Que hace este código?**

<img class=center src= assets/img/img3.png height=450/>

---

## Motivación 

### **Que hace este código?**

<img class=center src= assets/img/img4.png height=450/>

---

## Motivación 

### **Que hace este código?**

<img class=center src= assets/img/img5.png height=450/>

---

## Motivación 

### **Que hace este código?**

<img class=center src= assets/img/img6.png height=450/>

---

## Motivación 

### **Que hace este código?**

<img class=center src= assets/img/img7.png height=450/>

---

## Motivación 

### **Que hace este código?**

<img class=center src= assets/img/img8.png height=450/>

---

## Motivación

<q> Si ha tenido que copiar y pegar más de dos veces, entonces es tiempo de escribir una función</q>

---

## Estructuras de Control

Las estructuras de control en R le permiten controlar el flujo de ejecución del programa, dependiendo de las condiciones de ejecución. Las estructuras más comunes son:

- `if`, `else`: Prueban una condición

- `for`: Ejecutan un ciclo un número fijo de veces

- `while`: Ejecutan un ciclo **_Mientras_** una condición es verdadera 

- `repeat`: Ejecutan un ciclo un número infinito de veces 

- `break`: Detienen la ejecución de un ciclo

- `next`: Omiten una iteración de un ciclo

- `return`: Salir de una función

La mayoría de las estructuras de control no se utilizan en las sesiones interactivas, sino más bien al escribir funciones o expresiones más largas.

---

## Estructuras de control: if

```r
if(<condition>) {
        ## Hacer algo
} else {
        ## Hacer algo más
}
if(<condition1>) {
        ## Hacer algo
} else if(<condition2>)  {
        ## Hacer algo diferente
} else {
        ## Hacer algo diferente
}
```

---

## if

Esta es una estructura valida if/else 

```r
if(x > 3) {
        y <- 10
} else {
        y <- 0
}
```

Esta también.

```r
y <- if(x > 3) {
        10
} else { 
        0
}
```

---

## if

Por supuesto, la sentencia else no es necesaria. 

```r
if(<condition1>) {

}

if(<condition2>) {

}
```

---

## for

Los bucles `for` toman una variable iteradora y le asignan valores sucesivos de una secuencia o vector. Los bucles `for` se utilizan más comúnmente para iterar sobre los elementos de un objeto (lista, vector, etc.)
```r
for(i in 1:10) {
        print(i)
}
```

Este bucle toma la variable `i` y en cada iteración del bucle toma los valores 1, 2, 3, ..., 10. 

---

## for

Estos tres ciclos tienen el mismo comportamiento.

```r
x <- c("a", "b", "c", "d")

for(i in 1:4) {
        print(x[i])
}

for(i in seq_along(x)) {
        print(x[i])
}

for(letter in x) {
        print(letter)
}

for(i in 1:4) print(x[i])
```

---

## For anidados

Los bucles `for` pueden ser anidados.

```r
x <- matrix(1:6, 2, 3)
for(i in seq_len(nrow(x))) {
        for(j in seq_len(ncol(x))) {
                print(x[i, j])
        }   
}
```

Tenga cuidado con la anidación. Anidación más allá de 2-3 niveles es a menudo muy difícil de leer/entender.

---

## while

Ciclos `While` comienzan probando una condición. Si esta es cierta, entonces ejecutan el cuerpo del ciclo Una vez que se ejecuta el cuerpo del ciclo, la condición se comprueba de nuevo, y así sucesivamente.

```r
count <- 0
while(count < 10) {
        print(count)
        count <- count + 1
}
```

Ciclos `While` potencialmente pueden resultar en ciclos infinitos si no está escrito correctamente. Use con cuidado!

---

## while

A veces habrá más de una condición en la prueba.

```r
z <- 5
while(z >= 3 && z <= 10) {
        print(z)
        coin <- rbinom(1, 1, 0.5)
        
        if(coin == 1) {  
                z <- z + 1
        } else {
                z <- z - 1
        } 
}
```

Las condiciones siempre se evalúan de izquierda a derecha.

---

## repeat

`Repeat` inicia un ciclo infinito; en estadística aplicada no es comúnmente usado, pero si tiene sus usos. La única forma de salir de un ciclo `repeat` es llamando a un `break`.  

```r
x0 <- 1
tol <- 1e-8
repeat {
        x1 <- computeEstimate()
        
        if(abs(x1 - x0) < tol) {
                break
        } else {
                x0 <- x1
        } 
}
```

---

## repeat

El bucle en la diapositiva anterior es un poco peligroso porque no hay garantía de que se detendrá. Mejor establece un límite estricto sobre el número de iteraciones (por ejemplo, utilizando un bucle for) y luego pregunta si la convergencia se logró o no.

---

## next, return

`next` se utiliza para omitir una iteración de un ciclo

```r
for(i in 1:100) {
        if(i <= 20) {
                ## Skip the first 20 iterations
                next 
        }
        ## Do something here
}
```

`return` señales de que una función debe salir y regresar un valor dado

---

## Estructuras de control

Resumen

- Estructuras de control como `if`, `while`, y `for` te permiten tener el control sobe la ejecución en R

- Ciclos infinitos deben ser evitados en general, incluso si son teóricamente correcto.

- Las estructuras de control mencionadas aquí son principalmente útiles para escribir programas; para la línea de comandos de trabajo interactivo, las funciones apply son más útiles.

---

## Funciones

Las funciones son creadas usando la sentencia `function()` y es grabada como cualquier otro objeto en memoria de R.

En particular, son un objeto de la clase `function`.

```r
f <- function(<arguments>) {
        ## Código de programación
}
```
En R las funciones son “objetos de primera clase”, lo que significa que pueden ser tratados como cualquier otro objeto R. Es importante destacar que,
- Las funciones pueden ser pasadas como argumentos a otras funciones
- Las funciones se pueden anidar, por lo que se puede definir una función dentro de otra función
- El valor de retorno de una función es la última expresión a evaluar en el cuerpo de la función.

---

## Definición de funciones 

```r
my_fun <- function(arg1, arg2) {
    ## Cuerpo de la función 
}
```


```r
add <- function(x, y = 1) {
    x + y
}
```

---

## Anatomía de una función 


```r
add <- function(x, y = 1) {
    x + y
}
formals(add)
```

```
## $x
## 
## 
## $y
## [1] 1
```

```r
body(add)
```

```
## {
##     x + y
## }
```

---

## Anatomía de una función 


```r
add <- function(x, y = 1) {
    x + y
}
formals(add)
```

```
## $x
## 
## 
## $y
## [1] 1
```

```r
environment(add)
```

```
## <environment: R_GlobalEnv>
```

---

## Salidas: el valor devuelto `return`

La última expresión evaluada en una función es el valor de retorno


```r
f <- function(x) {
    if (x < 0) {
        -x
        } else {
            x
        }
}
f(-5)
```

```
## [1] 5
```

```r
f(15)
```

```
## [1] 15
```

---

## Salidas: el valor devuelto `return`

`return(value)` fuerza la función a terminar la ejecución y devuelve el el último valor.


```r
f <- function(x) {
    if (x < 0) {
        -x
        } else {
            x
        }
    return(x)
}
f(-5)
```

```
## [1] -5
```

```r
f(15)
```

```
## [1] 15
```

---

## Las funciones son objetos 


```r
mean2 <- mean
mean2(1:10)
```

```
## [1] 5.5
```

```r
function(x) { x + 1 }
```

```
## function(x) { x + 1 }
```

```r
(function(x) { x + 1 })(2)
```

```
## [1] 3
```

---

## Resumen 

- Hay tres partes en una función 
    + Argumentos 
    + Cuerpo
    + Ambiente 
    
- El valor devuelto es la última expresión ejecutada, o la primera sentencia `return()` encontrada

- Las funciones pueden ser tratadas como objetos comunes en R.

---

## Argumentos de funciones

Las funciones pueden tener _argumentos nombrados_ lo cuales potencialmente tienen _valores por defecto_.
- Los _argumentos formales_ son los argumentos incluidos en la definición de la función 
- La función `formals` devuelve una lista de todos los argumentos formales de una función. 
- No todas las llamada a la función en R hacen uso de todos los argumentos formales
- Los argumentos de funciones pueden ser _vacios_ o pueden tener valores por defectos 

---

## Ajuste de argumentos 

Los argumentos de funciones en R pueden ser ajustados posicionalmente o por nombres. Así que las siguientes llamadas a las funciones `sd` son equivalentes.

```r
> mydata <- rnorm(100)
> sd(mydata)
> sd(x = mydata)
> sd(x = mydata, na.rm = FALSE)
> sd(na.rm = FALSE, x = mydata)
> sd(na.rm = FALSE, mydata)
```

A pesar de que es correcto, no se recomienda probar con el orden de los argumentos, ya que puede dar lugar a cierta confusión.

---

## Ajuste de argumentos

Se puede mezclar ajuste posicional con ajuste por nombre. Cuando un argumento es igualado por su nombre, se "saca" de la lista de argumentos y los argumentos sin nombre restantes se corresponden en el orden en que aparecen en la definición de función .

```r
> args(lm)
function (formula, data, subset, weights, na.action,
          method = "qr", model = TRUE, x = FALSE,
          y = FALSE, qr = TRUE, singular.ok = TRUE,
          contrasts = NULL, offset, ...)
```

---

## Ajuste de argumentos 

Las siguientes dos llamadas a la función son equivalentes.

```r
lm(data = mydata, y ~ x, model = FALSE, 1:100)
lm(y ~ x, mydata, 1:100, model = FALSE)
```

---

## Ajuste de argumentos

Las argumentos de funciones también pueden ser _parcialmente_ pareados, lo cual es útil para trabajo interactivo.  

1. Compruebe si hay coincidencia exacta para un argumento con nombre
2. Compruebe si hay una coincidencia parcial
3. Compruebe si hay una coincidencia posicional

---

## El argumento "..." 

El argumento "..." indica una variable de un número de argumentos que pertenecen a otras funciones.

- "...", es bastante utilizado para extender funciones y en caso de que no se requiera copiar todos los argumentos de la función original.

```r
myplot <- function(x, y, type = "l", ...) {
plot(x, y, type = type, ...)
}
```
- Las funciones genéricas hacen uso del "...", así argumentos extra se le pueden pasar a los métodos

```r
mean
function (x, ...)
UseMethod("mean")
```


---

## Definiendo una función 

```r
f <- function(a, b = 1, c = 2, d = NULL) {

}
```

Además de no especificar un valor por defecto , también puede establecer un valor de argumento `NULL`.

---

## Evaluaciones 

Se evalúan los argumentos a funciones de la siguiente manera, sólo cuando sea necesario.


```r
f <- function(a, b) {
        a^2
    } 
f(2)
```

```
## [1] 4
```

Esta función nunca usa el argumento `b`, así que llamar a `f(2)` no produce error debido a que el 2 obtiene posicionalmente a `a`

---

## Evaluaciones


```r
f <- function(a, b) {
        print(a)
        print(b)
}
f(45)
```

```
## [1] 45
```

```
## Error in print(b): el argumento "b" está ausente, sin valor por omisión
```

Observe que "45" quedó impreso primero antes de que se desencadenara el error. Esto es por que `b` no tiene que ser evaluado hasta después `print(a)`. Una vez que la función trató de evaluar `print(b)` tuvo que mostrar un error.

---

## Scoping en R

El `scoping` en R describe como R busca los valores por nombres en los ambientes de trabajo. 


```r
x <- 10
x
```

```
## [1] 10
```

```r
f <- function() {
    x <- 1
    y <- 2
    c(x, y)
}
f()
```

```
## [1] 1 2
```

---

## Scoping en R

Si el nombre no esta definido dentro la función, R buscara en un nivel superior 

```r
x <- 2
g <- function() {
    y <- 1
    c(x, y)
}
g()
```

```
## [1] 2 1
```

---

## Scoping en R

Si el nombre no esta definido dentro la función de manera local, o en un nivel superior, ocurrirá un error.  

```r
rm(x)
g <- function() {
    y <- 1
    c(x, y)
}
g()
```

```
## Error in g(): objeto 'x' no encontrado
```

---

## Scoping en R

El `scoping` describe donde, no cuando, buscar un valor 


```r
f <- function() x
x <- 15
f()
```

```
## [1] 15
```

```r
x <- 20
f()
```

```
## [1] 20
```

---

## Scoping en R

El `scoping` trabaja igual para las funciones 


```r
l <- function(x) x + 1
m <- function() {
    l <- function(x) x * 2
    l(10)
}

m()
```

```
## [1] 20
```

```r
c <- 3
c(c = c)
```

```
## c 
## 3
```

---

## Scoping en R

Cada llamado a una nueva función tiene su propio y limpio ambiente.


```r
j <- function() {
    if (!exists("a")) {
        a <- 1
        } else {
            a <- a + 1
            }
    print(a)
}
j(); a
```

```
## [1] 1
```

```
## Error in eval(expr, envir, enclos): objeto 'a' no encontrado
```

---

## Programación funcional 

La programación funcional es una filosofía de programación que se basa en [lambda calculus](https://en.wikipedia.org/wiki/Lambda_calculus), es un enfoque de programación que ha tomado mucha popularidad en los últimos años, la programación funcional se concentra en cuatro constructores 

1. Datos (números,caracteres, etc.)
2. Variables (argumentos de funciones)
3. Funciones 
4. Aplicación de funciones (evaluación de funciones dados argumentos y/o datos)

---

## Programación funcional 

Por ahora hemos tratado las variables dentro de funciones como datos, ya sean valores como números o cadenas de caracteres, o sean estructuras de datos como listas y vectores. Con la programación funcional también puede considerar la posibilidad de que se pueda proporcionar una función como un argumento de otra función, y una función puede devolver otra función como su resultado.


---

## Programación funcional 


```r
adder_maker <- function(n){
  function(x){
    n + x
  }
}

add2 <- adder_maker(2)
add3 <- adder_maker(3)

add2(5)
```

```
## [1] 7
```

```r
add3(5)
```

```
## [1] 8
```

---

## Operaciones vectorizadas en R

Muchas operaciones en R son _vectorizadas_ haciendo el código más eficiente, conciso y fácil del leer 


```r
> x <- 1:4; y <- 6:9 
> x + y
[1] 7 9 11 13 
> x > 2
[1] FALSE FALSE  TRUE  TRUE
> x >= 2
[1] FALSE  TRUE  TRUE  TRUE
> y == 8
[1] FALSE FALSE TRUE FALSE 
> x * y
[1] 6 14 24 36
> x / y
[1] 0.1666667 0.2857143 0.3750000 0.4444444
```

---

## Operaciones vectorizadas en Matrices

```r
> x <- matrix(1:4, 2, 2); y <- matrix(rep(10, 4), 2, 2)
> x * y       ## 
     [,1] [,2]
[1,]   10   30
[2,]   20   40
> x / y
     [,1] [,2]
[1,]  0.1  0.3
[2,]  0.2  0.4
> x %*% y     ## 
     [,1] [,2]
[1,]   40   40
[2,]   60   60
```

---

## Familia `apply`

La escritura de un `for` y/o `while` es útil cuando se programa pero no lo son en otros casos, especialmente cuando se trabaja interactivamente con la línea de comandos. Hay algunas funciones que implementan ciclos para hacer esta tarea más fácil.

- `lapply`: Realiza un ciclo sobre una lista y evalúa una función en cada uno de sus elementos 
- `sapply`: Hace lo mismo que `lapply` pero trata de simplificar los resultados
- `apply`: Aplica una función sobre los margenes de un arreglo 
- `tapply`: Aplica una función sobre subconjuntos de un vector 
- `mapply`: Versión multivariada de `lapply`

La función auxiliar `split` es útil cuando se implementa en conjunto con la función `lapply`.

---

## `lapply` 

La función `lapply` toma tres argumentos, (1) una lista `X`, (2) una función `FUN` (nombre de la función), (3) otros argumentos vía `"..."`; Si `X` no es una lista, será obligada a convertirse en una. 


```r
lapply
```

```
## function (X, FUN, ...) 
## {
##     FUN <- match.fun(FUN)
##     if (!is.vector(X) || is.object(X)) 
##         X <- as.list(X)
##     .Internal(lapply(X, FUN))
## }
## <bytecode: 0x1d39ab8>
## <environment: namespace:base>
```

El ciclo real se realiza internamente en código `C`.


---


## lapply

`lapply` siempre devuelve una lista, sin importar la clase del objeto de entrada.


```r
x <- list(a = 1:5, b = rnorm(10))
lapply(x, mean)
```

```
## $a
## [1] 3
## 
## $b
## [1] 0.4580701
```

---

## lapply


```r
x <- list(a = 1:4, b = rnorm(10), c = rnorm(20, 1), d = rnorm(100, 5))
lapply(x, mean)
```

```
## $a
## [1] 2.5
## 
## $b
## [1] -0.2385197
## 
## $c
## [1] 0.7297901
## 
## $d
## [1] 5.043952
```

---

## lapply


```r
 x <- 1:4
lapply(x, runif)
```

```
## [[1]]
## [1] 0.947849
## 
## [[2]]
## [1] 0.9116104 0.7586608
## 
## [[3]]
## [1] 0.9798410 0.4627595 0.1115601
## 
## [[4]]
## [1] 0.01904155 0.15124433 0.23550737 0.75581686
```

---

## lapply


```r
 x <- 1:4
lapply(x, runif, min = 0, max = 10) # Acción de "..."
```

```
## [[1]]
## [1] 2.10245
## 
## [[2]]
## [1] 6.431024 7.433153
## 
## [[3]]
## [1] 2.218102 2.116330 4.728084
## 
## [[4]]
## [1] 6.318866 0.228269 3.898799 4.325862
```

---

## lapply

La función `lapply` y sus familiares hacen buen uso  de las funciones _anónimas_. Veamos una función anónima que extrae la primera columna en cada matriz.


```r
x <- list(a = matrix(1:4, 2, 2), b = matrix(1:6, 3, 2)) 
lapply(x, function(elt) elt[,1])
```

```
## $a
## [1] 1 2
## 
## $b
## [1] 1 2 3
```

---

## sapply

`sapply` tratara de simplificar el resultado de `lapply` si es posible.

- Si el resultado es una lista donde cada elemento es de longitud 1, entonces devuelve un vector

- Si el resultado es una lista donde cada elemento es un vector del mismo tamaño (> 1), devuelve una matriz.

- Sino puede resolver otra cosa, devuelve una lista.

---

## sapply


```r
x <- list(a = 1:4, b = rnorm(10), c = rnorm(20, 1), d = rnorm(100, 5))
lapply(x, mean)
```

```
## $a
## [1] 2.5
## 
## $b
## [1] 0.4161629
## 
## $c
## [1] 1.091888
## 
## $d
## [1] 4.898319
```

---

## sapply


```r
x <- list(a = 1:4, b = rnorm(10), c = rnorm(20, 1), d = rnorm(100, 5))
sapply(x, mean) 
```

```
##           a           b           c           d 
##  2.50000000 -0.03274642  0.76464161  5.02792989
```

```r
mean(x)
```

```
## Warning in mean.default(x): argument is not numeric or logical: returning
## NA
```

```
## [1] NA
```

---

## apply

`apply` se utiliza para evaluar una función (inclusive si es anónima) sobre los margenes de una arreglo.

- Comúnmente se utiliza para aplicar funciones sobre las filas o columnas de una matriz

- Se puede utilizar para cualquier arreglo en general, p.e, para calcular la media un arreglo de matrices.

- En realidad no es más rápida que escribir un ciclo, pero funciona con una sola linea de código.

---

## apply


```r
str(apply)
```

```
## function (X, MARGIN, FUN, ...)
```

- `X` es un array (arreglo)
- `MARGIN` es un vector entero indicando cual margen se debe "conservar". 
- `FUN` es la función a ser aplicada 
- `...` es para los otros argumentos a ser pasados a `FUN`

---

## apply


```r
x <- matrix(rnorm(200), 20, 10)
apply(x, 2, mean)
```

```
##  [1]  0.22388419 -0.17796319 -0.16461311 -0.22620730 -0.15328446
##  [6] -0.07008950  0.19106410  0.04482706 -0.15629645  0.20805424
```

```r
apply(x, 1, sum)
```

```
##  [1]  1.9359067 -2.9546620 -1.7753754  2.4999424 -4.0226454  0.3038791
##  [7]  1.8594786 -4.8530099 -1.2787070 -3.3747590 -0.1595906 -2.3613460
## [13] -3.7013051  6.4709163  1.1521151 -1.7276096  2.0484049  1.5080250
## [19]  1.5609207  1.2569327
```

---

## sumas y medias por filas/columnas

Para calcular las sumas y medias de una matriz se tienen las funciones. 

- `rowSums` = `apply(x, 1, sum)`
- `rowMeans` = `apply(x, 1, mean)`
- `colSums` = `apply(x, 2, sum)`
- `colMeans` = `apply(x, 2, mean)`

Las funciones son _mucho_ más rápidas, pero no se notara a menos que la aplique a matrices de grandes tamaños. 

---

## apply

Quantiles de las filas de una matriz.


```r
x <- matrix(rnorm(200), 20, 10)
apply(x, 1, quantile, probs = c(0.25, 0.75))
```

```
##           [,1]       [,2]       [,3]       [,4]       [,5]       [,6]
## 25% -0.2080651 -0.5293381 -0.3376532 -0.8951794 -0.8327757 -0.7633328
## 75%  1.0756413  0.4520678  0.6576990  0.6672075  0.5102312 -0.2322947
##           [,7]        [,8]       [,9]      [,10]      [,11]      [,12]
## 25% -0.8896295 -0.90307446 -0.4499636 -0.4144795 -0.3486109 -0.3263437
## 75%  0.6540020  0.02121337  0.5079890  0.8781901  0.6515486  0.3178363
##          [,13]      [,14]      [,15]      [,16]      [,17]      [,18]
## 25% -0.6184834 -0.9352254 -0.8127385 -0.2331308 -0.9806811 -1.1745378
## 75%  2.3975371  0.6371347  0.3372447  0.6158165  0.2762542  0.0959574
##           [,19]      [,20]
## 25% -0.31439097 -0.4537732
## 75%  0.05569096  0.4799344
```

---

## apply

Promedio de una matriz en un array


```r
a <- array(rnorm(2 * 2 * 10), c(2, 2, 10))
apply(a, c(1, 2), mean)
```

```
##             [,1]        [,2]
## [1,]  0.14885932 -0.09060326
## [2,] -0.07616305 -0.22827703
```

```r
rowMeans(a, dims = 2)
```

```
##             [,1]        [,2]
## [1,]  0.14885932 -0.09060326
## [2,] -0.07616305 -0.22827703
```

---



## tapply

`tapply` se utiliza para aplicar una función a subconjuntos de un vector.


```r
str(tapply)
```

```
## function (X, INDEX, FUN = NULL, ..., default = NA, simplify = TRUE)
```

- `X` es un vector
- `INDEX` es un factor o una lista de factores (se ven obligados a ser factores) 
- `FUN` es la función a ser aplicada
- `...` contiene otros argumentos a ser pasados a `FUN`
- `simplify`, debemos simplificar el resultado?

---

## tapply

Calcular las medias por grupos.


```r
x <- c(rnorm(10), runif(10), rnorm(10, 1))
f <- gl(3, 10)
f
```

```
##  [1] 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 3 3 3 3 3 3 3 3 3 3
## Levels: 1 2 3
```

```r
tapply(x, f, mean)
```

```
##           1           2           3 
## -0.09781739  0.47935805  1.09677291
```

---

## tapply

Calcular las medias por grupos, sin simplificar.


```r
tapply(x, f, mean, simplify = FALSE)
```

```
## $`1`
## [1] -0.09781739
## 
## $`2`
## [1] 0.479358
## 
## $`3`
## [1] 1.096773
```

---

## tapply

Hallar el rango por grupos.


```r
tapply(x, f, range)
```

```
## $`1`
## [1] -1.397644  0.637522
## 
## $`2`
## [1] 0.01579334 0.97329203
## 
## $`3`
## [1] -0.3225411  2.6415123
```

---

## split

La función `split` toma un vector u otros objetos y los divide en grupos determinados por un factor o lista de factores.


```r
str(split)
```

```
## function (x, f, drop = FALSE, ...)
```

- `x` es un vector (o lista) o data frame
- `f` es un factor (o se vuelve uno) o una lista de factores
- `drop` indica si los niveles de factores vacíos deben ser descartados

---

## split


```r
x <- c(rnorm(10), runif(10), rnorm(10, 1))
f <- gl(3, 10)
split(x, f)
```

```
## $`1`
##  [1]  0.8988963  1.7089348 -2.4020881  1.1398272 -0.3802320 -0.6992362
##  [7]  0.1898858  1.4863328  0.3399308  1.2220339
## 
## $`2`
##  [1] 0.4258578 0.7063204 0.6098785 0.3421009 0.4084894 0.3956378 0.5086673
##  [8] 0.6481148 0.1823343 0.6232481
## 
## $`3`
##  [1]  1.5707574  0.7643981  0.5607071  1.7039914  2.6102714 -0.1932340
##  [7]  0.9235666  1.6676393 -0.7472197  1.3850646
```

---

## split

Un uso común para `split` se da en compañía de `lapply`.


```r
lapply(split(x, f), mean)
```

```
## $`1`
## [1] 0.3504285
## 
## $`2`
## [1] 0.4850649
## 
## $`3`
## [1] 1.024594
```

---

## `split` en Data Frame


```r
suppressMessages(suppressWarnings(library(datasets)))
head(airquality)
```

```
##   Ozone Solar.R Wind Temp Month Day
## 1    41     190  7.4   67     5   1
## 2    36     118  8.0   72     5   2
## 3    12     149 12.6   74     5   3
## 4    18     313 11.5   62     5   4
## 5    NA      NA 14.3   56     5   5
## 6    28      NA 14.9   66     5   6
```

---

## `split` en Data Frame


```r
suppressMessages(suppressWarnings(library(datasets)))
s <- split(airquality, airquality$Month)
lapply(s, function(x) colMeans(x[, c("Ozone", "Solar.R", "Wind")]))
```

```
## $`5`
##    Ozone  Solar.R     Wind 
##       NA       NA 11.62258 
## 
## $`6`
##     Ozone   Solar.R      Wind 
##        NA 190.16667  10.26667 
## 
## $`7`
##      Ozone    Solar.R       Wind 
##         NA 216.483871   8.941935 
## 
## $`8`
##    Ozone  Solar.R     Wind 
##       NA       NA 8.793548 
## 
## $`9`
##    Ozone  Solar.R     Wind 
##       NA 167.4333  10.1800
```

---

## `split` en Data Frame


```r
sapply(s, function(x) colMeans(x[, c("Ozone", "Solar.R", "Wind")],
                                 na.rm = TRUE))
```

```
##                 5         6          7          8         9
## Ozone    23.61538  29.44444  59.115385  59.961538  31.44828
## Solar.R 181.29630 190.16667 216.483871 171.857143 167.43333
## Wind     11.62258  10.26667   8.941935   8.793548  10.18000
```

---

## mapply

`mapply` es una versión multivariada de apply, la cual aplica una función en paralelo sobre un conjunto de argumentos.


```r
str(mapply)
```

```
## function (FUN, ..., MoreArgs = NULL, SIMPLIFY = TRUE, USE.NAMES = TRUE)
```

- `FUN` es la función a aplicar 
- `...` contiene los argumentos a aplicar en la función original
- `MoreArgs` es una lista de otros argumentos para `FUN`.
- `SIMPLIFY` indica si el resultado debe ser simplificado 

---

## mapply

El siguiente código es tedioso de hacer 

`list(rep(1, 4), rep(2, 3), rep(3, 2), rep(4, 1))`

En este caso se podría hacer lo siguiente 


```r
mapply(rep, 1:4, 4:1)
```

```
## [[1]]
## [1] 1 1 1 1
## 
## [[2]]
## [1] 2 2 2
## 
## [[3]]
## [1] 3 3
## 
## [[4]]
## [1] 4
```

---

## Vectorizando una Función


```r
noise <- function(n, mean, sd) {
    rnorm(n, mean, sd)
    }
noise(5, 1, 2)
```

```
## [1] -0.4370720  3.0052535 -0.4674793  1.0482003 -1.3917352
```

```r
noise(1:5, 1:5, 2)
```

```
## [1] -0.04392019  1.02451927  2.51797074  3.64840662  2.28191341
```

---

## Vectorización instantanea 


```r
mapply(noise, 1:5, 1:5, 2)
```

```
## [[1]]
## [1] -2.7766
## 
## [[2]]
## [1] -0.2180762  4.3791053
## 
## [[3]]
## [1] 1.7634420 0.7118847 3.1938846
## 
## [[4]]
## [1] 4.696728 4.129925 1.319194 4.278245
## 
## [[5]]
## [1] 3.744128 5.775888 8.209449 5.548869 6.843793
```

---

## Estándar de código para R

1. Siempre utilice archivos de texto/editores

2. Indente su código

3. Limite el ancho se su código (80 columnas)

---

## Indentado

- Indentado mejora la lectura del código

- El ajuste del ancho de la columna previene el anidado y la funciones muy largas

- Sugerencia: Indente mínimo 4 espacios; 8 espacios es lo ideal


---

## Por que escribir funciones

- Si copio y pego dos veces es hora de escribir una función 
- la creación de funciones reduce los errores del copiado y pegado 
- Hace la actualización del código muy fácil 

---
