
DATOS:
- Cualitativos
  - Nominal
  - Ordinal
- Cuantitativos
  - De razón
  - Intervalo

Yo decido cómo usar el dato: Cuantitativo > Ordinal > Nominal

---

Análisis:
- Dato suelto. Usa variable:
  - Nominal / Ordinal
    - Tabla frecuencias -> Gráfico barras / sectores
      - Ordinal : Acumulados                                                            + Mediana
      - Nominal, pero puedo ordenar los datos en base a otra cosa, también acumulados   + Moda
  - Cuantitativo: Agrupar en intervalos (ORDINAL)
      - Gráficos: Histograma / Boxplot / Violín
      - Medidas de tendencia central: Media         /       Mediana (en función de la simetría)
      - Medidas de dispersión:        Desviación típica     Rango intercuartílico (Q3-Q1)
- Relacionamos variables:
  - Nominal x Nominal | Nominal x Ordinal | Ordinal x Ordinal
    - Tabla de contingencia -> Gráfico de barras / sectores
  - Nominal x Cuantitativo | Ordinal x Cuantitativo
    - Siempre tengo la posibilidad de la cuantitativa tratarla de forma ordinal (intervalos). Mismo caso que arriba
    - Si quiero tratarla como ordinal
    - Gráfico de cajas(boxplot)
    - Comparativa de medias/medianas
  - Cuantitativa / Cuantitativa:
    - Gráfico de dispersión

Si tengo variable tiempo (que siempre voy a tenerla), debe entrar en los estudios de una forma u otra.

SOLAMENTE tiene sentido el meter 3 variables en un estudioo si tienen muy pocas modalidades (muy pocos valores posibles). Es imposible para un humano entender más de 100 datos juntos.

    VARIABLE 1 x VARIABLE 2 x VARIABLE 3
        5      x     4      x      6        = 120

El orden en el que junte las variables es importante de cara al estudio concreto que quiero hacer:
No es lo mismo:
  Genero x Plataforma
    Quiero ver si para las distintas plataformas los generos sin distintos
  Plataforma x Genero
    Quiero ver si para los distintos generos las plataformas son distintas

---


En el mundo BI, cuando montamos un Datawarehouse, lo ideal si trabajo con archivos:
 SI PARQUET
    - Binario (ocupen menos que si os guardo como texto)
    - Formato orientado a columnas (más eficiente para hacer cálculos de BI)

          EJEMPLO
            Nombre         | Super Mario | Zelda    | Sonic         | Final Fantasy | Crash Bandicoot
            Plataforma     | Nintendo    | Nintendo | Sega          | Sony          | Sony
            Año            | 1985        | 1986     | 1991          | 1997          | 1996
            Género         | Plataformas | Aventura | Plataformas   | RPG           | Plataformas

 NO CSV (si tengo pocos datos)
    - Formato orientado a filas

          EJEMPLO:
            Nombre         |     Plataforma   | Año      | Género
            ------------------------------------------------------
            Super Mario    |     Nintendo     | 1985     | Plataformas
            Zelda          |     Nintendo     | 1986     | Aventura
            Sonic          |     Sega         | 1991     | Plataformas
            Final Fantasy  |     Sony         | 1997     | RPG
            Crash Bandicoot|     Sony         | 1996     | Plataformas

---

# Qué es un INDICE? 

Igual que el indice de un libro

Copia ORDENADA de los datos + Ubicación de los datos

Para qué sirven?
- Sirven para buscar DATOS CONCRETOS!
  Cuando tengo un INDICE puedo aplicar una búsqueda BINARIA!

Aunque haya un indice, una BBDD decidirá si le trae cuenta usar el índice o no!
Y normalmente en el mundo BI, los índices se van a usar poco!


            Nombre         |     Plataforma   | Año      | Género
            ------------------------------------------------------
        1   Super Mario    |     Nintendo     | 1985     | Plataformas
            Final Fantasy  |     Sony         | 1997     | RPG
        3   Zelda          |     Nintendo     | 1986     | Aventura
            Sonic          |     Sega         | 1991     | Plataformas
            Crash Bandicoot|     Sony         | 1996     | Plataformas

            INDICE de PLATAFORMA
            Nintendo -> 1, 3
            Sony     -> 2, 5
            Sega     -> 4

Quiero traer el año y el género de aquellos datos cuya plataforma sea Nintendo.
- OPCION 1: Usar el índice:
  - Enseguida encuentra que Nintendo está en la posición 1 y 3
  - Pero no le sirve con eso... Ahora tiene que llevar la aguja del HDD a la fila 1, para traer los datos de Año y Género
  - Luego tiene que llevar la aguja del HDD a la fila 3, para traer los datos de Año y Género
- OPCION 2: No usar el índice: FULLSCAN
  - Va fila por fila buscando la palabra Nintendo en la columna Plataforma
  - Cuando la encuentra, trae los datos de Año y Género

Y muchas veces, la opción 2 es más rápida que la opción 1! Sobre todo si traigo un buen puñado de datos.

---

Muchas herramientas de BI son capaces de hacer los calculos apoyándose en la BBDD.

    SELECT AÑO, GENERO, COUNT(*)
    FROM JUEGOS
    GROUP BY AÑO, GENERO;

    Si tengo un indice de AÑO, GENERO JUNTOS, la BBDD hace un FULLSCAN del INDICE y va mucho más rápido.
    Si filtro además, por plataforma

    SELECT AÑO, GENERO, COUNT(*)
    FROM JUEGOS
    WHERE PLATAFORMA = 'Nintendo'
    GROUP BY AÑO, GENERO;

    Si tengo creado un indice de PLATAFORMA, AÑO, GENERO, la BBDD hace un FULLSCAN del INDICE y va mucho más rápido.

    SELECT AÑO, PLATAFORMA, COUNT(*)
    FROM JUEGOS
    WHERE GENERO = 'Aventura'
    GROUP BY AÑO, PLATAFORMA;

    El problema es que para esta, el indice de arriba no vale... no es rentable... y necesito otro indice, con el orden de las columnas cambiado. (OTRA COPIA DE LOS DATOS EN EL HDD)

En otros casos, traen los datos y hacen ellas las cuentas. <<<<<< ESTA SUELE SER LA OPCION MAS ENCONTRADA EN EL MUNDO BI

    DEBO PREPROCESAR LOS DATOS UN HUEVO en el DW. Me interesa tener preconsolidados datos.

    SELECT *, COUNT(*)
    FROM VENTAS
    GROUP BY JUEGO, AÑO;

---

Hemos metido el campo importe: CUANTITATIVO (de razón)

CUANTI   CUALI
Precio x Genero

Cuál es el género que tiende a ser más caro? Que el genero tiene una relación con el precio (lo hemos dado por supuesto).
Cuál es el género que es más caro de promedio?
Una cosa es una relación a nivel de la BBDD... cada registro tiene un precio y un género.
Otra cosa es si a nivel ESTADISTICO tienen relación:
    - Distintos géneros tienen precios distintos.
      - Donde puede haber un género más caro
      - Donde puede haber un género más barato
Si todos los géneros tienen el mismo precio, es que el género no tiene INFLUENCIA en el precio (NO GUARDAN RELACION LAS VARIABLES)

Los juegos de por si, tienen distintos precios. Tenemos una variable ALEATORIA.

    TABLA                 GENEROS y el precio medio (mediano) de cada género

                                                                Q2
    Género               | Precio (tendencia central... media/mediana) | desv. típica | Rango 
    ------------------------------------------------------------------
    Aventura             | 50                                     Q1 Q3  
    Plataformas          | 40
    RPG                  | 60
    Deportes             | 30
    Estrategia           | 70

    GRAFICO
        BARRAS RUINA(solo pondríamos la media/mediana)
            Eje X: Género
            Eje Y: Precio
        Cajas (Boxplot) (podemos poner: La mediana, la media, el Q3, el Q1 y el máximo y mínimo)
            Eje X: Género
            Eje Y: Precio
    ESTADISTICOS: Mediana o media, desviación típica, rango intercuartílico

Precio x Plataforma
Precio x Editorial
Precio x Nombre
    Entradas en nuestra tabla que compartan nombre... cuantas tendremos? como mucho 6
    Con 6 datos... qué estadística hago? NINGUNA




CUANTI   CUANTI
    Precio x Año : Tiene muchos datos... Los años van en progresión (con respecto a la fecha)
        Y a fecha más alta, el año es más alto??? SI.
            10-10-2001
            10-01-2010
    Precio x Mes : MES TIENE 12 valores (y digo... la trato como ordinal)
        Y a fecha más alta el mes es más alto??? NO

        Y lo que me interesa si acaso es medir la tendencia en base a la fecha!
        El mes no sirve... para mirar la tendencia.

    La variable AÑO no es sino la variable FECHA medida de forma AGRUPADA (En intervalos)

    TABLA? NINGUNA !
    GRAFICA??? DISPERSION
        Eje X: Año
        Eje Y: Precio
        Y en ese diagrama MUY A DIFERENCIA DE OTROS DIAGRAMS, que pintamos? qué representamos?
            REGISTROS/DATOS
            En el resto de diagramas que pintamos? SUS FRECUENCIAS ! o ESTADISTICOS

    Y en ese diagrama puedo identificar TENDENCIAS ! (si las hay)
        Del tipo: A más año más pasta
        Del tipo: A más año menos pasta

    Madrid / Tenerife

    Cual tiene una temperatura más alta CUANDO?
    En enero: Tenerife
    En Julio: Madrid -5 -> 43

    En promedio cual tiene una temperatura más alta? TENERIFE



El número de juegos que se publican en una determinada plataforma de un determinado género, la podemos determinar a priori... o es aleatoria?
- Dónde está la aleatoriedad?
  Los juegos los sacan las editoriales... y una editorial decide hacer un juego de un determinado género para una determinada plataforma PORQUE LE VIENE EN GANA al direfctivo de turno...
  Se chupa el dedo... mira para ver por donde sopla el aire... y dice: DE AVENTURAS PARA LA PS.
    Otra cosa... es que no sea tan tan tan aleatorio... y que haya factores que hayan influido en su decisión... por ejemplo?
        - El exito pasado que haya demostrado un determinado género en esa plataforma...
          PERO ES UN CONDICIONANTE ABSOLUTO? NO
            Un directivo puede decir... pues vamos a probar un genero en esa plataforma
            Otro directivo puede decir... pues vamos a hacer uino de los de siempre RIESGO 0

Hay un problema grande con los MODELOS DE MACHINE LEARNING (IA)... cuando nos metemos en el mundillo, pensamos ignorantemente que podemos hacer un modelo que llegue a predecir al 100% un dato... y eso es IMPOSIBLE.

Soy una compañía de seguros... y tengo 100.000.000 de datos pasados.. de personas que me han contrado el seguro un determinado año. De ellas conozco si me dieron un parte o no!
También conozco MUCHISIMOS DATOS DE ESAS PERSONAS (demográficos, genéticos, sociales, comportamiento...)
Cuando llegue una persona nueva a contratarme el seguro, voy a capaz de predecir si me va a dar un parte?

El tema es que realmente ME DA IGUAL de si esa persona me va a dar un parte o no!
    ^^^^
    TASAR LA PROBABILIDAD DE EQUIVOCARME EN LA DECISION!
    Si me equivoco en un 20% de los casos

    Me da igual equivocarme en un 20% de los casos... o en un 80.... lo que importa es conocer ese número.

    COSTES NORMALES DE UN SEGURO 1000
    COSTES DE UN SEGURO CON PARTE SON 2000
    Si me equivoco en un 20%... es decir, en mi caso si tengo 10.000 clientes nuevos... me equivoco en 2.000... 2000 x 1000 = 2.000.000 / 10.000 = 200 más que le cobro a cada uno... Y YA GANO PASTA.

No hacemos estudios a nivel de INDIVIDUO... me la trae el peiro... Lo que miro es EL COLECTIVO !

---

Influye la altura de una persona en sus conocimientos de matemáticas?
- NO

En cambio si tomo un conjunto de datos con personas comprendidas entre 0 y 15 años, veré en ese conjunto una relación entre altura y conocimientos de matemáticas?
- SI = GIGANTESCA

DONDE ESTA EL TRUCO????
El problema es que existe lo que en estadística llamamos una VARIBLE MEDIADORA...que está generando una relación espuria entre las variables.

    ALTURA <- EDAD -> CONOCIMIENTOS DE MATEMATICAS

    ALTURA <-> CONOCIMIENTOS DE MATEMATICAS

    Si tengo una variable mediadora... y hago un estudio de correlación entre las variables... me va a dar que la altura influye en los conocimientos de matemáticas... y no es así.

    Si tengo una variable mediadora... y hago un estudio de correlación entre las variables... me va a dar que la altura influye en los conocimientos de matemáticas... y no es así.

    Si tengo una variable mediadora... y hago un estudio de correlación entre las variables... me va a dar que la altura influye en los conocimientos de matemáticas... y no es así.

    Si tengo una variable mediadora... y hago un estudio de correlación entre las variables... me va a dar que la altura influye en los conocimientos de matemáticas... y no es así.

    Si tengo una variable mediadora... y hago un estudio de correlación entre las variables... me va a dar que la altura influye en los conocimientos de matemáticas... y no es así.

    Si tengo una variable mediadora... y hago un estudio de correlación entre las variables... me va a dar que la altura influye en los conocimientos de matemáticas... y no es así.

    Si tengo una variable mediadora... y hago un estudio de correlación entre las variables... me va a dar que la altura influye en los conocimientos de matemáticas... y no es así.

    Si tengo una variable mediadora... y hago un estudio de correlación entre las variables... me va a dar que la altura influye en los conocimientos de matemáticas... y no es así.

    Si tengo una variable mediadora... y hago un estudio de correlación entre las variables... me va a dar que la altura influye en los conocimientos de matemáticas... y no es así.

    Si tengo una variable mediadora... y hago un estudio de correlación entre las variables... me va a dar que la altura influye en los conocimientos de matemáticas... y no es así.

    Si tengo una variable mediadora... y hago un estudio de correlación entre las variables... me va a dar que la altura influye