dw-2020-parcial-1
================
Tepi
9/3/2020

# Examen parcial

Indicaciones generales:

-   Usted tiene el período de la clase para resolver el examen parcial.

-   La entrega del parcial, al igual que las tareas, es por medio de su
    cuenta de github, pegando el link en el portal de MiU.

-   Pueden hacer uso del material del curso e internet (stackoverflow,
    etc.). Sin embargo, si encontramos algún indicio de copia, se
    anulará el exámen para los estudiantes involucrados. Por lo tanto,
    aconsejamos no compartir las agregaciones que generen.

## Sección I: Preguntas teóricas.

-   Existen 10 preguntas directas en este Rmarkdown, de las cuales usted
    deberá responder 5. Las 5 a responder estarán determinadas por un
    muestreo aleatorio basado en su número de carné.

-   Ingrese su número de carné en `set.seed()` y corra el chunk de R
    para determinar cuáles preguntas debe responder.

``` r
set.seed(20190613) 
v<- 1:10
preguntas <-sort(sample(v, size = 6, replace = FALSE ))

paste0("Mis preguntas a resolver son: ",paste0(preguntas,collapse = ", "))
```

    ## [1] "Mis preguntas a resolver son: 2, 4, 5, 6, 7, 9"

### Listado de preguntas teóricas

1.  Para las siguientes sentencias de `base R`, liste su contraparte de
    `dplyr`:
    -   `str()`
    -   `df[,c("a","b")]`
    -   `names(df)[4] <- "new_name"` donde la posición 4 corresponde a
        la variable `old_name`
    -   `df[df$variable == "valor",]`
2.  Al momento de filtrar en SQL, ¿cuál keyword cumple las mismas
    funciones que el keyword `OR` para filtrar uno o más elementos una
    misma columna?

El keyword que cumple la misma funcion que ‘or’ es: ‘\|’

3.  ¿Por qué en R utilizamos funciones de la familia apply
    (lapply,vapply) en lugar de utilizar ciclos?

4.  ¿Cuál es la diferencia entre utilizar `==` y `=` en R?

El simbolo ‘=’ se utiliza para asignar un valor a una variable y el ‘==’
se utiliza representando una equivalencia.

Ejemplo ‘=’ X = (1,2,3) es decir que ‘x’ contiene los valores 1 2 y 3

Ejemplo ‘==’ a == b representando que son iguales

5.  ¿Cuál es la forma correcta de cargar un archivo de texto donde el
    delimitador es `:`?

Primero se instala y corre la libreria ‘readr’ De la misma utilizaremos
la funcion ‘read\_delim()’ Dentro de la cual se especifica la direccion
del archrchivo y el separador que en este caso es ‘:’

Ejemplo DF &lt;- read\_delim(file(), sep = ‘:’)

6.  ¿Qué es un vector y en qué se diferencia en una lista en R?

Los vectores en R son objetos que puede contener datos numéricos, cadena
de caracteres o datos lógicos,etc; pero solo puede tener elementos del
homogeneos o del mismo tipo. Una lista en cambio puede tener diferentes
objetos y sus elementos pueden ser heterogeneos.

7.  ¿Qué pasa si quiero agregar una nueva categoría a un factor que no
    se encuentra en los niveles existentes?

R dara un resultado de N/A en la posicion de la nueva categoria ya que
no existe en ninguno de los niveles.

8.  Si en un dataframe, a una variable de tipo `factor` le agrego un
    nuevo elemento que *no se encuentra en los niveles existentes*,
    ¿cuál sería el resultado esperado y por qué?
    -   El nuevo elemento
    -   `NA`
9.  En SQL, ¿para qué utilizamos el keyword `HAVING`?

El Keyword ‘having’ se utiliza para filtrar por agrgacion ya que cuando
se utilizan agregaciones no se puedee utilizar ‘Where’

10. Si quiero obtener como resultado las filas de la tabla A que no se
    encuentran en la tabla B, ¿cómo debería de completar la siguiente
    sentencia de SQL?

    -   SELECT \* FROM A \_\_\_\_\_\_\_ B ON A.KEY = B.KEY WHERE
        \_\_\_\_\_\_\_\_\_\_ = \_\_\_\_\_\_\_\_\_\_

Extra: ¿Cuántos posibles exámenes de 5 preguntas se pueden realizar
utilizando como banco las diez acá presentadas? (responder con código de
R.)

## Sección II Preguntas prácticas.

-   Conteste las siguientes preguntas utilizando sus conocimientos de R.
    Adjunte el código que utilizó para llegar a sus conclusiones en un
    chunk del markdown.

A. De los clientes que están en más de un país,¿cuál cree que es el más
rentable y por qué?

El cliente mas rentable es el ‘c53868a0’ porque es que tiene mas ingreso
operando en mas de 1 pais.

B. Estrategia de negocio ha decidido que ya no operará en aquellos
territorios cuyas pérdidas sean “considerables”. Bajo su criterio,
¿cuáles son estos territorios y por qué ya no debemos operar ahí?

### Los top 5 territorios con mayor perdida son:

### 1. 68de9759 - 5.74%

### 2. 8682908b - 5.05%

### 3. 45c0376d - 4.27%

### 4. 0320288f - 3.82%

### 5. 4814799f - 2.67%

\#\#\#Si se tuviese que eliminar alguno eliminaria los primeros 3 ya que
representan perdidas mayores al 4%

### I. Preguntas teóricas

## A

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
parcial_anonimo <- readRDS("~/GitHub/DataWrangling/Parcial1_Wrangling/data-wrangling-2020-parcial-1/parcial_anonimo.rds")

parcial_anonimo %>%
  group_by(Cliente, Venta) %>%
  summarise(Client_multpais=n_distinct(Pais)) %>% 
  filter(Client_multpais>1) %>% 
  summarise(sum(Venta))
```

    ## `summarise()` has grouped output by 'Cliente'. You can override using the `.groups` argument.

    ## # A tibble: 7 x 2
    ##   Cliente  `sum(Venta)`
    ##   <chr>           <dbl>
    ## 1 044118d4         73.8
    ## 2 a17a7558         91.2
    ## 3 bf1e94e9          0  
    ## 4 c53868a0        197. 
    ## 5 f2aab44e          0  
    ## 6 f676043b          0  
    ## 7 ff122c3f         59.0

## B

``` r
Perdida <- parcial_anonimo %>% 
  group_by(Territorio) %>% 
  filter(Venta<0) %>% 
  summarise(perdida_terr=sum(Venta))
```

``` r
Ingreso <- parcial_anonimo %>% 
  group_by(Territorio) %>% 
  filter(Venta>0) %>% 
  summarise(Venta_terr=sum(Venta))
```

``` r
Tabla <- left_join(Ingreso,Perdida, by = "Territorio")
```

``` r
library(formattable)

Tabla %>% 
  mutate(Porcentaje = percent((-perdida_terr)/Venta_terr)) %>% 
  arrange(-Porcentaje)
```

    ## # A tibble: 104 x 4
    ##    Territorio Venta_terr perdida_terr Porcentaje
    ##    <chr>           <dbl>        <dbl> <formttbl>
    ##  1 68de9759        3930.       -226.  5.74%     
    ##  2 8682908b        4857.       -245.  5.05%     
    ##  3 45c0376d       11121.       -475.  4.27%     
    ##  4 0320288f         879.        -33.6 3.82%     
    ##  5 4814799f       24928.       -664.  2.67%     
    ##  6 5a464f3f        4921.       -126.  2.57%     
    ##  7 0bbe6418        9110.       -218.  2.39%     
    ##  8 d43e8f6a        5847.       -136.  2.33%     
    ##  9 6eff1266        7616.       -176.  2.31%     
    ## 10 c31adb2f       10114.       -230.  2.28%     
    ## # ... with 94 more rows
