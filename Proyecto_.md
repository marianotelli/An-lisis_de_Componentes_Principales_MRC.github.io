Análisis Componentes Principales
================
2022-09-19

# Análisis de Componentes Principales

### ***Fenómeno a explicar***

El análisis de componentes principales transforma un conjunto de
variables correlacionadas en un nuevo conjunto de variables no
correlacionadas **(Almenara et al., 1998).** Es lo que conocemos como
técnica de reducción de dimensionalidad dentro de la <span
style="color:red">estadística multivariada</span>, buscando la pérdida
mínima de la información.

### ***Explicación Problema***

Nuestras 8 varibles son: *ingresos*, *nivel de educación*, *edad*,
*tiempo viviendo en la residencia actual*, *tiempo trabajando para el
empleador actual*, *ahorros*, *deuda* y *número de tarjetas de crédito*.
Lo que se pretende es analizarlas, para agruparlas y explicarlas de
mejor manera, de modo que, el *gerente* de una institución financiera
pueda tomar mejores decisiones y simplificar el modelado en un futuro.

    ## [1] "la cantidad de observaciones que tenemos es: 30"

### *Resumen de datos*

``` r
head(x)
```

    ## # A tibble: 6 × 8
    ##   Ingresos Educación  Edad Residencia Empleo Ahorros Deuda `Tarj Crédito`
    ##      <dbl>     <dbl> <dbl>      <dbl>  <dbl>   <dbl> <dbl>          <dbl>
    ## 1    50000        16    28          2      2    5000  1200              2
    ## 2    72000        18    35         10      8   12000  5400              4
    ## 3    61000        18    36          6      5   15000  1000              2
    ## 4    88000        20    35          4      4     980  1100              4
    ## 5    91100        18    38          8      9   20000     0              1
    ## 6    45100        14    41         15     14    3900 22000              4

### *Análisis Descriptivo*

``` r
summary(x)
```

    ##     Ingresos       Educación          Edad        Residencia         Empleo    
    ##  Min.   :21240   Min.   :12.00   Min.   :26.0   Min.   : 1.000   Min.   : 1.0  
    ##  1st Qu.:37055   1st Qu.:14.00   1st Qu.:29.0   1st Qu.: 2.250   1st Qu.: 2.0  
    ##  Median :41100   Median :16.00   Median :33.0   Median : 5.000   Median : 4.0  
    ##  Mean   :48646   Mean   :15.27   Mean   :32.8   Mean   : 5.933   Mean   : 5.5  
    ##  3rd Qu.:58125   3rd Qu.:16.00   3rd Qu.:36.0   3rd Qu.: 8.000   3rd Qu.: 8.0  
    ##  Max.   :91100   Max.   :20.00   Max.   :41.0   Max.   :15.000   Max.   :14.0  
    ##     Ahorros          Deuda        Tarj Crédito  
    ##  Min.   :    0   Min.   :    0   Min.   :1.000  
    ##  1st Qu.: 1950   1st Qu.:  800   1st Qu.:2.000  
    ##  Median : 4750   Median : 1200   Median :2.500  
    ##  Mean   : 8319   Mean   : 3812   Mean   :2.867  
    ##  3rd Qu.:14050   3rd Qu.: 6600   3rd Qu.:3.750  
    ##  Max.   :34000   Max.   :22000   Max.   :6.000

### *Analizando la relación entre las variables*

``` r
round(cor(x = x, method = "pearson"), 3)
```

    ##              Ingresos Educación   Edad Residencia Empleo Ahorros  Deuda
    ## Ingresos        1.000     0.549  0.515      0.347  0.334   0.210 -0.196
    ## Educación       0.549     1.000  0.229      0.108  0.049   0.447 -0.457
    ## Edad            0.515     0.229  1.000      0.838  0.848   0.552  0.032
    ## Residencia      0.347     0.108  0.838      1.000  0.952   0.570  0.186
    ## Empleo          0.334     0.049  0.848      0.952  1.000   0.539  0.247
    ## Ahorros         0.210     0.447  0.552      0.570  0.539   1.000 -0.393
    ## Deuda          -0.196    -0.457  0.032      0.186  0.247  -0.393  1.000
    ## Tarj Crédito   -0.059    -0.296 -0.130      0.053  0.023  -0.410  0.474
    ##              Tarj Crédito
    ## Ingresos           -0.059
    ## Educación          -0.296
    ## Edad               -0.130
    ## Residencia          0.053
    ## Empleo              0.023
    ## Ahorros            -0.410
    ## Deuda               0.474
    ## Tarj Crédito        1.000

### *Correlación Entre Variables*

``` r
pairs.panels(x, 
             pch=20,
             stars=T,
             main="Gráfico de Correlación")
```

![](Proyecto__files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

### **Análisis de Componentes Principales**

Cabe recalcar que tuvimos que <span
style="color:blue">estandarizar</span> las variables dentro de la
función PCA(), debido a que no todas se encuentran la misma escala.

Por definición, el número máximo de componentes principales que podemos
obtener es igual al número de variables que tenemos, en este caso 8.

R, nos arroja, en esta matriz, el componente, su eigenvalor (o valor
propio), el porcentaje de varianza que explica el componente y
finalmente el % de varianza acumulada, que al final es del 100% en el
Componente 8.

    ##        eigenvalue percentage of variance cumulative percentage of variance
    ## comp 1 3.54756813             44.3446017                          44.34460
    ## comp 2 2.13199433             26.6499291                          70.99453
    ## comp 3 1.04473533             13.0591916                          84.05372
    ## comp 4 0.53151226              6.6439033                          90.69763
    ## comp 5 0.41120264              5.1400331                          95.83766
    ## comp 6 0.16648803              2.0811004                          97.91876
    ## comp 7 0.12535593              1.5669491                          99.48571
    ## comp 8 0.04114334              0.5142918                         100.00000

#### **Escogiendo el Número de Componentes**

Tenemos dos criterios principales. El primero de ellos es el de Kaiser y
este consiste en que deben conservarse solamente aquellos *Componentes*
cuyos eigenvalues sean mayores a 1.

El segundo es con la gráfica de sedimentación y en donde notemos un
codo.

En ambos casos, tenemos que, podemos tomar hasta

Como buscamos reducir dimensionalidad y agrupar, vemos que con tan solo
Dos Componentes podemos explicar el 70.9% de la variabilidad total del
conjunto de datos, y por este motivo es que para este ejemplo tomaré
solamente dos Componentes.

![](Proyecto__files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

#### **Variables en los Componentes**

Extrayendo las contribuciones que tiene cada variable por cada uno de
los componentes principales, tenemos que en el **CP1** tienen mayor peso
las variables Ingresos, edad, residencia, empleo y ahorros. Por su
parte, el **CP2**, se encuentra influido por la Educación, deuda y
tarjeta de crédito.

    ##                   Dim.1     Dim.2
    ## Ingresos      9.8534065  2.092211
    ## Educación     5.6150446 19.722667
    ## Edad         23.4217757  1.825084
    ## Residencia   21.7521808  7.649999
    ## Empleo       21.0610173  9.269407
    ## Ahorros      16.3270485  4.793461
    ## Deuda         0.4518949 34.228892
    ## Tarj Crédito  1.5176318 20.418279

Igualmente, de manera gráfica lo podemos visualizar en el siguiente
gráfico de correlación, partiendo de que no existen correlaciones
negativas.

![](Proyecto__files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

Este biplot explica el 71% de la variabilidad total de los datos. Entre
entre más juntas esten las líneas entre ellas, quiere decir que tienen
una mayor correlación.

Claramente hay una clara relación entre el tiempo que llevan trabajando
en el mismo *\[Empleo\_ - \_Residencia\]*. Asimismo, Hay una fuerte
entre *\[Ingreso\_ - \_Ahorro\]* y *\[Deuda\_ - \_# de tarjetas de
crédito\]*.

La escala de colores está de acuerdo al cos2 o bien, la calidad de
representación.

El **CP1** tiene fuerte relación (como se mencionó previamente) con las
variables: *Edad*, *Residencia*, *Empleo* y *Ahorros*, aspectos
relacionados a la estabilidad financiera a largo plazo.

El **CP2** mide el historial crediticio de la persona que solicita el
préstamo por su relación con la *Deuda* y *Tarjetas de crédito*.

![](Proyecto__files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

La siguiente gráfica nos señala la distribución de las observaciones a
lo largo del plano, para efecto de individuos rotulados de interés, nos
puede ayudar a clasificarlos (en este caso a ver qué clase de
potenciales clientes son).

![](Proyecto__files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

Finalmente, podemos ver la comparación de las observaciones y variables
en el plano; por ejemplo la observación *16* es un potencial cliente
responsable por la estabilidad que muestra.

![](Proyecto__files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->
