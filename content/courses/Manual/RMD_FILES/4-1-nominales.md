Punto de partida
----------------

Como sabemos, los estadísticos de tendencia central van a depender del
tipo de variable que estemos analizando.

Para practicar este apartado vamos a utilizar la base de datos de Lapop
del año 2017. [Lapop](https://www.vanderbilt.edu/lapop-espanol/) es el
Proyecto de Opinión Pública de América Latina implementado por la
Universidad de Vanderbilt, el cual es un proyecto de investigación
multinacional especializado en el desarrollo, implementación y análisis
de encuestas de opinión pública.

Abrimos la base de datos desde nuestro repositorio:

    library(rio)
    link="https://github.com/DataPolitica/salidas/raw/master/Data/sub_lapop.sav"
    lapop=import(link)

Esta base de datos ha sido editada con fines didácticos (idioma,
recodificación, etc).

Preparamos la variable
----------------------

Selecciones la variable *urbanorural* y procedemos a configurarla
adecuadamente.

Primero identifiquemos cómo el R está leyendo la variable.

    str(lapop$urbanorural)

    ##  num [1:2153] 2 2 2 2 2 2 2 2 2 2 ...
    ##  - attr(*, "label")= chr "Urbano / Rural"
    ##  - attr(*, "format.spss")= chr "F8.2"
    ##  - attr(*, "display_width")= int 9
    ##  - attr(*, "labels")= Named num [1:2] 1 2
    ##   ..- attr(*, "names")= chr [1:2] "Urbano" "Rural"

    class(lapop$urbanorural)

    ## [1] "numeric"

Luego, al ver que aún se encuentra como *numérico*, entonces lo
convertimos en factor:

    lapop$urbanorural=as.factor(lapop$urbanorural)
    class(lapop$urbanorural)

    ## [1] "factor"

Verificamos los niveles y, de acuerdo con la metadata, le asignamos las
etiquetas a cada uno de los mismos.

    levels(lapop$urbanorural)

    ## [1] "1" "2"

    levels(lapop$urbanorural)<-c("Urbano","Rural")
    levels(lapop$urbanorural)

    ## [1] "Urbano" "Rural"

Ya tenemos la variable lista para aplicar técnicas de análisis
descriptivo.

1. Estadísticos descriptivos de una nominal
-------------------------------------------

Lo primero que podemos hacer es solicitar la función `summary`. Esta
función nos va a dar una mirada rápida al contenido de nuestra variable.
En el caso de ser factor, nos va a mostrar las frecuencias (incluye los
NA si los hubiera):

    summary(lapop$urbanorural)

    ## Urbano  Rural 
    ##   1324    829

Como sabemos el estadístico más idóneo para variables nominales es la
moda. Por ello, hacemos uso del paquete `DescTools` y de la función
`Mode()`:

    library(DescTools)
    Mode(lapop$urbanorural)

    ## [1] Urbano
    ## attr(,"freq")
    ## [1] 1324
    ## Levels: Urbano Rural

Lo que nos dice el resultado es que la moda es la categoría *Urbano* y
nos indica la frecuencia.

Otra opción también es solicitar una tabla simple para ver la frecuencia
de cada categoría.

    table (lapop$urbanorural)

    ## 
    ## Urbano  Rural 
    ##   1324    829

Asimismo, podemos solicitar una tabla que nos muestre las proporciones
por cada categoría. Para ello, utilizamos la función `prop.table()`.
Cuando escribimos esta función hay que tener en cuenta que la tenemos
que aplicar *sobre* la tabla de frecuencias normal (que aplicamos
anteriormente):

    prop.table(table(lapop$urbanorural))

    ## 
    ##    Urbano     Rural 
    ## 0.6149559 0.3850441

Si queremos los porcentaje hacemos lo mismo pero al final le decimos que
lo multiple por 100 (\*100):

    prop.table(table(lapop$urbanorural))*100

    ## 
    ##   Urbano    Rural 
    ## 61.49559 38.50441

2. Análisis gráfico de una variable nominal
-------------------------------------------

Lo primero que podemos solicitar es un diagrama de pie o de sectores.
Nótese que primero debemos crear un objeto que sea la tabla de la
variable que deseamos graficar, en este caso, de urbano rural. De esa
manera, el programa podrá utilizar los datos y plasmarlos en un gráfico.

    mi_tabla <- table(lapop$urbanorural)
    pie(mi_tabla)

![](4-1-nominales_files/figure-markdown_strict/unnamed-chunk-10-1.png)

También podemos solicitar un diagrama de barras.

    barplot(mi_tabla)

![](4-1-nominales_files/figure-markdown_strict/unnamed-chunk-11-1.png)

3. Agregando más detalles a nuestros gráficos
---------------------------------------------

Hasta este momento hemos visto que podemos solicitar gráficos de una
forma muy práctica. Sin embargo, nosotros podemos solicitar un producto
más elaborado en la medida en que agreguemos más características al
código.

Por ejemplo, si deseamos un diagrama de barras azul escribimos:

    barplot(mi_tabla, col="blue")

![](4-1-nominales_files/figure-markdown_strict/unnamed-chunk-12-1.png)

Si deseamos un diagrama de barras azul y con título escribimos:

    barplot(mi_tabla,col="blue", 
            main="Casos según su lugar de procedencia")

![](4-1-nominales_files/figure-markdown_strict/unnamed-chunk-13-1.png)

Como nos damos cuenta, nosotros podemos ir agregando más características
a nuestro código. Es posible que recuerdes ejemplos de códigos de
programación que son líneas interminables. Eso es así porque los
programadores elaboran productos muy específicos y que, en su mayoría de
veces, requieren la especificación de muchas características.

Nuestro objetivo será avanzar en el uso de estos códigos de programación
para poder, al igual que los programadores, elaborar análisis
estadísticas cada vez más complejos y específicos.