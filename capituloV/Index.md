---
title       : Visualización de datos en R
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

## Principios del análisis gráfico 

- Principio 1: Mostrar comparaciones

 - La evidencia de una hipótesis siempre es __*relativa*__ con respecto a una hipótesis
 - Siempre pregunta "¿Comparado con que?"

---

##  Mostrar comparaciones

<img src="assets/fig/unnamed-chunk-1-1.png" title="plot of chunk unnamed-chunk-1" alt="plot of chunk unnamed-chunk-1" style="display: block; margin: auto;" />

---
## Mostrar comparaciones 

<img src="assets/fig/unnamed-chunk-2-1.png" title="plot of chunk unnamed-chunk-2" alt="plot of chunk unnamed-chunk-2" style="display: block; margin: auto;" />

---

## Principios del análisis gráfico 

- Principio 1: Mostrar comparaciones

 - La evidencia de una hipótesis siempre es *relativa* con respecto a otra hipótesis
 - Siempre pregunta "¿Comparado con que?"

- Principio 2: Mostrar causalidad, el mecanismo, la explicación, la estructura sistemática
 - ¿Cuál es su marco causal para pensar en una pregunta?

---

## Mostrar Causalidad
<img src="assets/fig/unnamed-chunk-3-1.png" title="plot of chunk unnamed-chunk-3" alt="plot of chunk unnamed-chunk-3" style="display: block; margin: auto;" />

---

## Principios del análisis gráfico 

* Principio 1: Mostrar comparaciones

- La evidencia de una hipótesis siempre es *relativa* con respecto a otra hipótesis
 - Siempre pregunta "¿Comparado con que?"
 
* Principio 2: Mostrar causalidad, el mecanismo, la explicación, la estructura sistemática
 - ¿Cuál es su marco causal para pensar en una pregunta?

* Principio 3: Mostrar datos multivariados

 - Multivariado = más de 2 variables
 - El mundo real es multivariado

---

## Mostrar datos multivariados

<img src="assets/fig/unnamed-chunk-4-1.png" title="plot of chunk unnamed-chunk-4" alt="plot of chunk unnamed-chunk-4" style="display: block; margin: auto;" />

---

## Mostrar datos multivariados

<img src="assets/fig/unnamed-chunk-5-1.png" title="plot of chunk unnamed-chunk-5" alt="plot of chunk unnamed-chunk-5" style="display: block; margin: auto;" />

---
## Principios del análisis gráfico 

* Principio 4: Integración de evidencias
 
 - Integrar palabras, números, imágenes, diagramas
 - Gráficos que deben hacer uso de muchos modos de presentación de datos

---

## Integrar diferentes formas de evidencias 

<img class=center src= assets/img/img1.png height=450/>

---

## Principios del análisis gráfico 

* Principio 4: Integración de evidencias
 
 - Integrar palabras, núemeros, imágenes, diagramas
 - Gráficos que deben hacer uso de muchos modos de presentación de datos
 
* Principio 5: Describir y documentar la evidencia con 
  las adecuadas etiquetas, escalas, fuentes, etc.

 - Un gráfico de datos debe contar una historia completa que sea creíble

---
## Principios del análisis gráfico 

* Principio 4: Integración de evidencias
 
 - Integrar palabras, núemeros, imágenes, diagramas
 - Gráficos que deben hacer uso de muchos modos de presentación de datos
 
* Principio 5: Describir y documentar la evidencia con 
  las adecuadas etiquetas, escalas, fuentes, etc.

 - Un gráfico de datos debe contar una historia completa que sea creíble

* Principio 6: El contenido manda

 - Representaciones analíticas en última instancia, suben o bajan de calidad 
   debido a la pertinencia e integridad de su contenido.  

---

## Resumen

> * Principio 1: Mostrar comparaciones
> * Principio 2: Mostrar la causalidad, el mecanismo, la explicación
> * Principio 3: Mostrar datos multivariados
> * Principio 4: Integrar diferentes formas de evidencias 
> * Principio 5: Describir y documentar la evidencia
> * Principle 6: El contenido manda

---

## El sistema base de gráficos 

- Modelo "Canvas"
- Comienza con un lienzo en blanco y construye el gráfico desde allí 
- Comienza con una función `plot` (o similar)
-Utiliza funciones de anotación (segundo nivel) para añadir/modificar capas del gráfico (`text`, `lines`, `points`, `axis`)

---

## El sistema base de gráficos 


- Tiene la ventaja de que permite la construcción de gráficos por capas extremadamente detallados 

- No puedes editar el código una vez hayas empezado (p.e. ajustar los margenes)

- Dificulta la trasmisión de código 

- Los gráficos se convierten en una serie de comandos 

---

## Gráfico Base

<img src="assets/fig/unnamed-chunk-6-1.png" title="plot of chunk unnamed-chunk-6" alt="plot of chunk unnamed-chunk-6" style="display: block; margin: auto;" />

---

## El sitema Lattice 

- Los gráficos son creados con una llamada a una sola función (`xyplot`, `bwplot`, etc.)

- Es mucho más versátil para gráficos condicionales: Donde se miran relaciones entre variable en presencia de una variable auxiliar (factor)

- Cosas como margenes o espaciado se generan automáticamente debido a que el gráfico se genera en una sola función 

- Excelente para mostrar gráficos en una sola ventana 

---

## El sitema Lattice 

- A veces no es tan cómodo crear un gráfico en una sola llamada a una función

- Las anotaciones (funciones de bajo nivel) no son tan intuitivas 

- El uso de funciones panel y subíndices son difíciles de manejar, requiere una buena preparación 

- No se puede adicionar nada al gráfico una vez se creo

---

## Gráfico Lattice 

<img src="assets/fig/unnamed-chunk-7-1.png" title="plot of chunk unnamed-chunk-7" alt="plot of chunk unnamed-chunk-7" style="display: block; margin: auto;" />

---

## El sistema ggplot2 

- Divide la diferencia entre el sistema base y lattice de muchas formas

- Trata de forma automática con el espaciado, texto, títulos pero de igual forma permite añadir capas al gráfico mediante el operador adición `+`

- De manera visual se comporta parecido a lattice pero generalmente es más intuitivo y fácil de utilizar

- Por defecto escoge muchas opciones del gráfico pero permite un alto grado de personalización

---

## Gráfico ggplot2 

<img src="assets/fig/unnamed-chunk-8-1.png" title="plot of chunk unnamed-chunk-8" alt="plot of chunk unnamed-chunk-8" style="display: block; margin: auto;" />

---

## Que es un dispositivo gráfico?

- Un dispositivo gráfico es algo donde se puede almacenar un gráfico
   
   - Una ventana en tu maquina (_screen device_)
  
   - Un archivo PDF (_file device_)

   - Un archivo PNG o JPEG (_file device_)

   - Un archivo vector escalable de gráficos (SVG) (_file device_)

- Cuando haces un gráfico R, ha sido enviado a un dispositivo gráfico en especifico

---

## Como se crea un gráfico en R?

Existen dos formas para gráficar. La primera es la más común:

1. Llamar a la función que gráfica como `plot`, `xyplot`, o `qplot`

2. El gráfico aparece en el dispositivo de pantalla

3. Haga anotaciones si lo considera necesario 

4. Disfrute de su gráfico 


```r
library(datasets)
with(faithful, plot(eruptions, 
                    waiting))  ## Hace que el gráfico aparezca en la pantalla 
title(main = "Old Faithful Geyser data")  ## Añade un titulo (Anotación)
```

---

## Como se crea un gráfico en R?

El segundo método es la más común cuando se trata de utilizar dispositivos tipo archivos.

1. Explicitamente lance un dispositivo gráfico

2. Llame la función que realiza el gráfico (Nota: si esta utilizando un dispositivo gráfico mediante un archivo el gráfico no aparecerá en su pantalla)

3. Realice anotaciones si lo considera necesario

3. Explicitamente cierre el dispositivo gráfico con la función `dev.off()` (es extremadamente importante!)


```r
pdf(file = "myplot.pdf")  ## Abre el dispositivo PDF; crea 'myplot.pdf' en el dt
## Crea el gráfico y lo envia al archivo 
with(faithful, plot(eruptions, waiting))  
title(main = "Old Faithful Geyser data")  ## Añade el titulo; nada en la pantalla
dev.off()  ## Cierra el archivo PDF 
## Ahora puede ver el archivo 'myplot.pdf' en su computadora 
```


---


## Gráficos Base 

Los gráficos de base se utilizan con mayor frecuencia y son muy potentes para crear gráficos en 2-D.

- Existen dos fases para crear un gráfico base 
  - Inicializar un gráfico (función plot)
  - Anotar (añadir capas) a un gráfico existente

- Llamar a la función `plot(x, y)` o `hist(x)` lanzara le dispositivo gráfico 

- Si los argumentos de `plot` no son de alguna clase especial, entonces se llama al método _default_  de`plot`; Esta función tiene muchos argumentos, permitiéndole definir el título, la etiqueta del eje x, la etiqueta del eje y, etc.
- El sistema de gráficos base tiene _muchos_ parámetros que se pueden establecer y ajustar; Estos parámetros están documentados en `?Par`.

---

## Gráficos Base: Histograma


```r
suppressMessages(suppressWarnings(library(datasets)))
hist(airquality$Ozone)  ## Dibuja un gráfico
```

<img src="assets/fig/unnamed-chunk-11-1.png" title="plot of chunk unnamed-chunk-11" alt="plot of chunk unnamed-chunk-11" style="display: block; margin: auto;" />

---

## Gráficos Base: Scatterplot


```r
suppressMessages(suppressWarnings(library(datasets)))
with(airquality, plot(Wind, Ozone))
```

<img src="assets/fig/unnamed-chunk-12-1.png" title="plot of chunk unnamed-chunk-12" alt="plot of chunk unnamed-chunk-12" style="display: block; margin: auto;" />

---

## Gráficos Base: Boxplot


```r
suppressMessages(suppressWarnings(library(datasets))) ;data(airquality)
airquality <- transform(airquality, Month = factor(Month))
boxplot(Ozone ~ Month, airquality, xlab = "Month", ylab = "Ozone (ppb)")
```

<img src="assets/fig/unnamed-chunk-13-1.png" title="plot of chunk unnamed-chunk-13" alt="plot of chunk unnamed-chunk-13" style="display: block; margin: auto;" />

---

## Algunos parámetros importantes 

Muchas funciones del sistema base comparten un conjunto de parámetros. Éstos son algunos de los principales:

- `pch`: símbolo a gráficar en puntos (un circulo por defecto)

- `lty`: el tipo de linea (una linea solida por defecto), puede ser discontinua, punteada, etc.

- `lwd`: el ancho de la linea, especificado como un entero 

- `col`: el color a gráficar, especificado como un número, carácter, o código hexadecimal; la función `colors()` regresa un vector de nombres de colores 

- `xlab`: Cadena de caracteres para la etiqueta del eje x

- `ylab`: Cadena de caracteres para la etiqueta del eje y


---

## Algunos parámetros importantes 

La función `par()` se utiliza para especificar los parámetros __*globales*__ de gráficos  que afectan a todas las gráficas de una sesión en R. Estos parámetros se pueden sobreescribir cuando se especifican como argumentos a funciones de gráficos específicas.

- `las`: La orientación de las etiquetas del eje en el gráfico
- `bg`: El color de fondo
- `mar`: El tamaño del margen
- `oma`: El tamaño del margen externo (por defecto es 0 en todos los lados)
- `mfrow`: Número de gráficos por fila, columna (gráficos por filas)
- `mfcol`: Número de gráficos por fila, columna (gráficos por columnas)

---

## Algunos parámetros importantes 

Valores por defecto para algunos parámetros de gráficos 


```r
par("lty")
```

```
## [1] "solid"
```

```r
par("col")
```

```
## [1] "black"
```

```r
par("pch")
```

```
## [1] 1
```

---

## Algunos parámetros importantes 

Valores por defecto para algunos parámetros de gráficos 


```r
par("bg")
```

```
## [1] "white"
```

```r
par("mar")
```

```
## [1] 5.1 4.1 4.1 2.1
```

```r
par("mfrow")
```

```
## [1] 1 1
```

---

## Funciones de Gráficos Base 

- `plot`: Hace un diagrama de dispersión u otro tipo de gráfico dependiendo de la clase del objeto que se está utilizando

- `lines`: Añade líneas a un gráfico, dado un vector `x` de valores  y un correspondiente vector de valores `y` (o una matriz de 2 columnas); Esta función sólo conecta los puntos

- `points`: Añade puntos a un gráfico
- `text`: Añade etiquetas de texto a un gráfico utilizando coordenadas `x`, `y` especificadas
- `title`: Añade anotaciones a las etiquetas del eje `x`, `y`, título, subtítulo, margen externo 
- `mtext`: Añade texto arbitrario a los márgenes (internos o externos) del gráfico
- `axis`: Añade etiquetas/etiquetas de eje

---

## Gráfico Base con anotación 


```r
suppressMessages(suppressWarnings(library(datasets)))
with(airquality, plot(Wind, Ozone))
title(main = "Ozone and Wind in New York City")  ## Add a title
```

<img src="assets/fig/unnamed-chunk-16-1.png" title="plot of chunk unnamed-chunk-16" alt="plot of chunk unnamed-chunk-16" style="display: block; margin: auto;" />

---

## Gráfico Base con anotación 


```r
suppressMessages(suppressWarnings(library(datasets)))
with(airquality, plot(Wind, Ozone, main = "Ozone and Wind in New York City"))
with(subset(airquality, Month == 5), points(Wind, Ozone, col = "blue"))
```

<img src="assets/fig/unnamed-chunk-17-1.png" title="plot of chunk unnamed-chunk-17" alt="plot of chunk unnamed-chunk-17" style="display: block; margin: auto;" />

---

## Gráfico Base con anotación 


```r
with(airquality, plot(Wind, Ozone, main = "Ozone and Wind in New York City", type = "n"))
with(subset(airquality, Month == 5), points(Wind, Ozone, col = "blue"))
with(subset(airquality, Month != 5), points(Wind, Ozone, col = "red"))
legend("topright", pch = 1, col = c("blue", "red"), legend = c("May", "Other Months"))
```

<img src="assets/fig/unnamed-chunk-18-1.png" title="plot of chunk unnamed-chunk-18" alt="plot of chunk unnamed-chunk-18" style="display: block; margin: auto;" />

---

## Multiples gráficos Base 


```r
par(mfrow = c(1, 2))
with(airquality, {
	plot(Wind, Ozone, main = "Ozone and Wind")
	plot(Solar.R, Ozone, main = "Ozone and Solar Radiation")
})
```

<img src="assets/fig/unnamed-chunk-19-1.png" title="plot of chunk unnamed-chunk-19" alt="plot of chunk unnamed-chunk-19" style="display: block; margin: auto;" />

---

## Multiples gráficos Base 


```r
par(mfrow = c(1, 3), mar = c(4, 4, 2, 1), oma = c(0, 0, 2, 0))
with(airquality, {
	plot(Wind, Ozone, main = "Ozone and Wind")
	plot(Solar.R, Ozone, main = "Ozone and Solar Radiation")
	plot(Temp, Ozone, main = "Ozone and Temperature")
	mtext("Ozone and Weather in New York City", outer = TRUE)
})
```

<img src="assets/fig/unnamed-chunk-20-1.png" title="plot of chunk unnamed-chunk-20" alt="plot of chunk unnamed-chunk-20" style="display: block; margin: auto;" />

---

## Sistema de gráficos lattice 

El sistema de gráficos lattice se implemento utilizando los siguientes paquetes:

- *lattice*: contiene el código para producir gráficos tipo _trellis_ el cual es independiente del sistema base, incluye funciones como: `xyplot`, `bwplot`, `levelplot`

- *grid*: implementa diferentes sistemas gráficos independientes del sistema base, el paquete *lattice* se construyó con base a *grid*

- El sistema lattice no tiene el sistema de dos fases, aspecto que lo separa bastante del sistema base 

---

## Funciones lattice

- `xyplot`: esta es la función principal, crea diagrama de puntos 
- `bwplot`: gráficos box-and-whiskers, (“boxplots”)
- `histogram`: histogramas
- `stripplot`: como un boxplot pero con puntos 
- `dotplot`: gráficos de puntos en forma de "violín"
- `splom`: scatterplot matrix; como `pairs` en el sistema base
- `levelplot`, `contourplot`: para graficar datos de "imágenes"

---

## Funciones lattice 

Las funciones de lattice generalmente toman una formula como primer argumento, 
de la siguiente forma:

```r
xyplot(y ~ x | f * g, data)
```

- Se utiliza la *notación de formula*, debido al `~`.

- Al lado izquierdo de `~` esta la variable del eje y, en el lado derecho esta la variable del eje x

- f y g son _variables condicionales_ — son opcionales 
  - el `*` indica interacción entre las variables 

- El segundo argumento es el data frame o lista del cual se obtendrán las variables de la formula 

- Sino se especifica ningún otro argumento, se establecerán el resto de valores por defecto.

---

## Gráficos Lattice 


```r
suppressMessages(suppressWarnings(library(lattice)))
suppressMessages(suppressWarnings(library(datasets)))
xyplot(Ozone ~ Wind, data = airquality)
```

<img src="assets/fig/unnamed-chunk-21-1.png" title="plot of chunk unnamed-chunk-21" alt="plot of chunk unnamed-chunk-21" style="display: block; margin: auto;" />

---

## Gráficos Lattice 


```r
suppressMessages(suppressWarnings(library(lattice)))
suppressMessages(suppressWarnings(library(datasets)))
airquality <- transform(airquality, Month = factor(Month)) 
xyplot(Ozone ~ Wind | Month, data = airquality, layout = c(5, 1))
```

<img src="assets/fig/unnamed-chunk-22-1.png" title="plot of chunk unnamed-chunk-22" alt="plot of chunk unnamed-chunk-22" style="display: block; margin: auto;" />

---

## Lattice Comportamiento 


```r
p <- xyplot(Ozone ~ Wind, data = airquality)  # Nada pasa!
print(p)  # Aparece el gráfico!
```

<img src="assets/fig/unnamed-chunk-23-1.png" title="plot of chunk unnamed-chunk-23" alt="plot of chunk unnamed-chunk-23" style="display: block; margin: auto;" />

```r
xyplot(Ozone ~ Wind, data = airquality)  ## Auto-impresión 
```

---

## Lattice funciones panel

- Las funciones lattice tienen ** funciones panel ** las cuales controlan lo que pasa dentro de cada panel del gráfico

- El paquete *lattice* viene con funciones panel por defecto, pero se pueden implementar algunos cambios para lo que pasa dentro de cada panel 

- Las funciones panel reciben las coordenadas x/y de los puntos en su panel


---

##  Lattice funciones panel


```r
set.seed(10)
x <- rnorm(100)
f <- rep(0:1, each = 50)
y <- x + f - f * x + rnorm(100, sd = 0.5)
f <- factor(f, labels = c("Group 1", "Group 2"))
xyplot(y ~ x | f, layout = c(2, 1))  ## gráfico con 2 paneles
```

<img src="assets/fig/unnamed-chunk-25-1.png" title="plot of chunk unnamed-chunk-25" alt="plot of chunk unnamed-chunk-25" style="display: block; margin: auto;" />

---

## Lattice funciones panel


```r
xyplot(y ~ x | f, panel = function(x, y, ...) {
       panel.xyplot(x, y, ...) ## primer llamado a la función panel por 'xyplot'
       panel.abline(h = median(y), lty = 2)  ## Añade una linea horizontal en la media 
})
```

<img src="assets/fig/unnamed-chunk-26-1.png" title="plot of chunk unnamed-chunk-26" alt="plot of chunk unnamed-chunk-26" style="display: block; margin: auto;" />

---

## Lattice funciones panel: Linea de regresión 


```r
xyplot(y ~ x | f, panel = function(x, y, ...) {
               panel.xyplot(x, y, ...)  
               panel.lmline(x, y, col = 2)  
       })
```

<img src="assets/fig/unnamed-chunk-27-1.png" title="plot of chunk unnamed-chunk-27" alt="plot of chunk unnamed-chunk-27" style="display: block; margin: auto;" />

---

## Paneles en Lattice 

<img src="assets/fig/unnamed-chunk-28-1.png" title="plot of chunk unnamed-chunk-28" alt="plot of chunk unnamed-chunk-28" style="display: block; margin: auto;" />

---

