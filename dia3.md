
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