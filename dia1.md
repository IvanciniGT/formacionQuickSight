
# Quicksight

## Introducción a AWS QuickSight

- Conceptos básicos y características principales
- Integración con el ecosistema de AWS
- Beneficios y casos de uso

## Preparación y Transformación de Datos

- Conexión con fuentes de datos (S3, RDS, Redshift, Athena, entre otras.)
- Limpieza y transformación de datos
- Creación de datasets

## Visualizaciones Básicas

- Uso de gráficos y tablas en QuickSight
- Tipos de visualizaciones disponibles
- Personalización de gráficos y elementos visuales

## Análisis Avanzado y Predictivo

- Uso de “ML Insights” para predicciones automáticas
- Creación de métricas y KPI personalizados
- Incorporación de análisis predictivo en los informes

## Creación de Dashboards y Reportes Interactivos

- Diseño de dashboards personalizados
- Uso de filtros y controles interactivos
- Publicación y compartición de dashboards

## Colaboración y Automatización

- Compartir informes con equipos y usuarios finales
- Configuración de alertas y reportes automáticos
- Integración con otras herramientas de BI y plataformas externas.

---

Seguridad del dato en QuickSight
Monitorización




---

# Quicksight

Herramienta de Business Intelligence de AWS.

## Business Intelligence

Origen de datos EXCEL.
BUCKETS de S3. <- EXCEL, PARQUET

El análisis principalmente será de tipo DESCRIPTIVO:
- A nivel de una variable
- Y a nivel de varias variables (2,3, 4 ya son muchas) para buscar correlaciones entre ellas.

Con qué tipos de datos permiten trabajar estas herramientas?
- Texto
- Número
- Fecha
- ...

Y eso está bien? Desde el punto de vista del análisis ESTADÍSTICO, la naturaleza de dato (ese tipo) que ponen estos programas me da igual... NO ME APORTA NADA ! DE NADA !

Me interesa clasificar los datos de otra forma:
- Categóricos/Nominales
- Ordinales/Cuasi-cuantitativos
- Cuantitativos:
  - Escalas de razón
  - Escalas de intervalo

Ésto es lo que condiciona TODO lo que puedo hacer sobre el dato:
- Métricas que puedo sacar
- Los gráficos que puedo pintar

CUIDADO CON ESTO! Dicen que la estadística lo aguanta todo... como el papel.

## Tipos de datos para un análisis BI.

Las herramientas de BI típicamente clasifican los datos en:
- Números
- Texto
- Fechas
- ...

Desde el punto de vista de la estadística los clasificamos en :
- Cualitativos (---> Dimensiones)       ---> No son cantidades
  - Categóricos/Nominales               ---->  Son solo nombres... que me permitirán CLASIFICAR/Separar en grupos
                                                     Color del pelo
                                                     Sexo
                                                     Nombre de una persona
                                                     DNI
  - Ordinales/Cuasi-cuantitativos       ---->  También son nombre... y por ende me permiten CLASIFICAR/Separar en grupos
                                               Pero tiene una característica adicional, esos nombres tienen una relación de orden... 
                                               En base a la magnitud de la variable que representan.
                                                     Número de la calle
                                               Los datos ordinales, además de clasificar a los sujetos de estudio, nos permiten ORDENARLOS.
- Cuantitativos                         ---> Cantidad
                                              Una cantidad ... no es sino una nombre: Salario "2750€/mes" = NOMBRE
                                                Y por ende, los datos cuantitativos me permiten clasificar a los sujetos.
                                              Además, esos nombres tienen un orden entre si, en base a la magnitud de la variable que representan. Puedo decir que una persona tiene más salario que otra? SI
                                              Pero además, al ser cantidades, puedo realizar operaciones con ellos.
                                              Qué operaciones?
                                                - Sumar, restar, dividir? DEPENDE de la naturaleza de la variable.
  - Escalas de razón                           Es cuando en la escala de medición, el valor 0 representa ausencia de la propiedad 
                                               que estoy midiendo.

                                                    Número de hijos. Si tengo 0 hijos que significa? Que no tengo hijos.
                                                    Si una familia tiene 5 hijos y otra 10, tiene sentido afirmar que la segunda familia tiene el doble de hijos que la primera? SI
                                                        10/5 = 2

  - Escalas de intervalo                       Es cuando en la escala de medición, el valor 0 NO representa ausencia de la propiedad 
                                               que estoy midiendo, simplemente es un dato más.
                                                Cuando uso una escala de intervalo, las multiplicaciones y divisiones no tienen sentido.

                                                    Temperatura de una ciudad en grados centígrados.
                                                    Si en una ciudad hace -10 grados y en otra 20 grados, tiene sentido afirmar que en la segunda ciudad hace el doble de calor que en la primera? NO
                                                        20/-10 = -2

                                                En estos casos puedo sumar y restar... pero no multiplicar ni dividir.


Por definición las variables cuantitativas son ordinales.
Y por definición las variables ordinales son categóricas.

Pero las variables categóricas no son ordinales.
Y las variables ordinales no son cuantitativas.

    MAS INFORMACION ----------------> MENOS INFORMACION
    cuantitativas --> ordinales ----> categóricas

Muchas veces elijo la forma de representar el dato... siempre que pueda, me interesa tener de un dato la mayor cantidad posible de información: Tenerla medida de forma cuantitativa.

Otra cosa es cómo usaré el dato... un dato que tengo medido en forma cuantitativa, podré usarlo como dato ordinal o categórico... al revés nunca.

Una transformación de dato típica en los programas de BI es pasar de un dato cuantitativo a un dato cualitativo.

    Número de hijos: 1 hijo, 3 hijos... 0 hijo, 5 hijos       <- Medición cuantitativa
                                                                    vvvvv
    Número de hijos: Ninguno < pocos <> muchos < demasiados   <- Medición cualitativa / Ordinal (Generar INTERVALOS)
                                                                    vvvvv
    Número de hijos: No tiene / Tiene hijos                   <- Medición cualitativa / Categórica


Las Cantidades son diferentes a los números. Las cantidades tienen una UNIDAD DE MEDIDA asociada.
Las cantidad podemos operarlas... por el hecho de tener cantidad de medida.
Los números también se pueden operar... porque las matemáticas lo permiten.... las matemáticas aguantan todo.

    5 - 2 = 3
    (13 + 5 ) / 2 = 9

Yo vivo en el portal de la calle número 13... y mi primo en el 5... En que portal vivimos de media? No tiene sentido. Los números de los portales.. aún siendo números... no se pueden operar.... porque no son cantidades.

OJO, no nos quedemos solo con las unidades de medida del SI... hay muchas más unidades de medida.

Cuántos hijos tiene una familia? Cuál es la unidad de medida? hij@s.

Código postal es un número... pero no una cantidad
El número del DNI es un número... pero no una cantidad
El número de la calle es un número... pero no una cantidad
Un número de teléfono es un número... pero no una cantidad

Es más... desde el punto de vista de una herramienta de BI, me interesaría que prácticamente TODOS LOS DATOS sean de tipo NUMÉRICO.

Color del pelo: Moreno, Rubio, Castaño, Pelirrojo
En una herramienta de BI me interesa que ese dato sea de tipo numérico:
    Moreno:     1
    Rubio:      2
    Castaño:    3
    Pelirrojo:  4
Pero no serán cantidades... Qué color de media tienen las personas de este pueblo?

---

# Cómo analizo datos en cualquier herramienta de BI? Estadística DESCRIPTIVA

El objetivo SIEMPRE es entender los datos que tengo... y es que entender los datos es muy diferente de tener los datos.
Y qué hacemos? RESUMIR LA INFORMACIÓN... eso es lo que nos da la estadística... técnicas para resumir la información. 
La primera de esas técnicas es: CATEGORIZAR... al categorizar, puedo contar cuánta sujetos tengo en cada categoría (FRECUENCIA).
Y podemos montar una TABLA DE FRECUENCIAS:

                                                                    <----------------- Esto tiene sentido? ------------------------->
                                                                    En este caso ninguno. Solo tiene sentido si la variable es ORDINAL
                                                                    o he ordenado a los sujetos por otro dato.
        | Color de pelo | Frecuencia absoluta | Frecuencia relativa | Frecuencia absoluta acumulada | Frecuencia relativa acumulada |
        |---------------|---------------------|---------------------|-------------------------------|-------------------------------|
        | Moreno        | 10                  | 25%                 | 10                            | 25%                           |
        | Rubio         | 5                   | 12.5%               | 15                            | 37.5%                         |
        | Castaño       | 5                   | 12.5%               | 20                            | 50%                           |
        | Pelirrojo     | 20                  | 50%                 | 40                            | 100%                          |


        | Número de hijos | Frecuencia absoluta | Frecuencia relativa | Frecuencia absoluta acumulada | Frecuencia relativa acumulada |
        |-----------------|---------------------|---------------------|-------------------------------|-------------------------------|
        | 0               | 10                  | 25%                 | 10                            | 25%                           |
        | 1               | 5                   | 12.5%               | 15                            | 37.5%                         |
        | 2               | 5                   | 12.5%               | 20                            | 50%                           |
        | 3               | 20                  | 50%                 | 40                            | 100%                          |
        
        Aquí si tienen sentido los datos acumulados

    Cuando tengo una variable ordinal o cuantitativa con muchos valores diferentes (en estadística a esos potenciales valores los llamamos MODALIDADES), la tabla de frecuencias no resulta práctica... NO ME AYUDA A ENTENDER MEJOR LOS DATOS.
    Me interesaría AGRUPAR LOS DATOS (GENERAR INTERVALOS)... dicho de otra forma, cambiar la escala de medición de la variable.
        CUANTITATIVA -> ORDINAL

    Si ahora veo la tabla... voy a tener más información de los salarios del banco? 
    - No tengo más información... El máximo detalle de información que puedo tener son las nóminas de los empleados.
    - Lo que si pasa, es que he aprendido un dato nuevo, que antes no tenía... Estoy entendiendo la distribución de los salarios en el banco.


    Esa tabla la podría también pintar/dibujar:
    - Diagrama de barras     (represento las frecuencias absolutas)
          Dependiendo de si me interesa conocer el dato exacto 
    - Diagrama de sectores   (represento las frecuencias relativas)
          O de si lo que me interesa es comparar las proporciones de las categorías

    Si tuviera una variable CUANTITATIVA
    - No tendría sentido hacer una tabla de frecuencias 
    - Tendría sentido representarla gráficamente? SI... con un gráfico HISTOGRAMA / Diagrama de caja y bigote (boxplot)
                                                                                  / Diagrama de violín. 

A veces la resumimos más aún... De hecho a un simple NÚMERO: ESTADÍSTICOS
- Estadísticos de tendencia central... Por donde van los tiros?                                     Para calcular esto, necesito?
  - Moda            Valor que más se repite... con la frecuencia más alta.                            Grupos / Frecuencia / 
                                                                                                      Clasificar a los sujetos

            Se puede calcular a variables de tipo CUALITATIVA / CUANTITATIVA

  - Mediana         Valor que divide en 2 partes "iguales" a los sujetos. (juego del limbo)           Ordenar a los sujetos  

            Se puede calcular a variables de tipo ORDINAL / CUANTITATIVA
            Recibe otro nombre: Percentil 50, Segundo cuartil

  - Media           Calculo matemático: Sumo todos los valores y divido por el número de valores.     Operar con los valores

            Se puede calcular a variables de tipo CUANTITATIVA
- Estadísticos de dispersión.......... Cuánto cambia la cosa con respecto al "por dónde van los tiros?"

    Y cuidado: JAMAS PUEDO GENERAR UN INFORMA QUE INCLUYA UN ESTADÍSTICO DE TENDENCIA CENTRAL Y NO UNO DE DISPERSIÓN... 
    Puedo estar (consciente o inconscientemente) engañando al personal
        - Rango intercuartílico -> Cuánto se alejan los datos de la mediana (Q3-Q1)
        - Desviación típica     -> Cuánto se alejan los datos de la media

- Medidas de posición:
  - Mediana: Divide a los sujetos en 2 partes iguales... 50% de la gente en cada uno
  - Cuartiles: Dividen a los sujetos en 4 partes iguales... 25% de la gente en cada uno
    - Hay 3 cuartiles, que dividen en 4 partes:
           |------|-----|-----|------|
                 Q1    Q2     Q3
                     Mediana
                       P50
                 P25          P75 
  - Percentiles: Dividen a los sujetos en 100 partes iguales... 1% de la gente en cada uno

    Esos datos salen de las frecuencias relativas acumuladas... y de la tabla de frecuencias.

Con variables NOMINALES, solo puedo calcular la MODA.
Con variables ORDINALES, puedo calcular la MODA y la MEDIANA.
Con variables CUANTITATIVAS, puedo calcular la MODA, la MEDIANA y la MEDIA. La moda generalmente nunca me interesa (poca información)
    Entre mediana y media, cuál elijo? DEPENDE DE LA SIMETRÍA de la distribución de datos.
    ** Ver ejemplo 1


- Puedo analizar 1 variable aislada
    Tabla de frecuencias (valores absolutos y relativos, acumulados de ambos solo si la variable es ordinal)
    Podemos representar gráficamente la tabla de frecuencias:
        - Diagrama de barras        - Variables Cualitativas
        - Diagrama de sectores      /
        - Histograma                - Variables Cuantitativas
        - Diagrama de caja y bigote //
        - Diagrama de violín        /

    ** Diagramas de caja: Salarios en el banco


        ----1300------2300---------4500---5700----------------8000----------> Variable
                       ^             ^     ^
                      Q1            Q2    Q3
            |          +-------------+----+                  |
            |----------|             |    |------------------|   o      o     (Valores atípicos: outliers)
            |          +-------------+----+                  |

            Mínimo                                          Máximo 
            "razonable"                                     "razonable"
            de la variable                                  de la variable

    Se toma el Rango Intercuartílico (Q3-Q1): 5700-2300 = 3400 
    Se multiplica por 1.5: 3400 * 1.5 = 5100
    Se suman y restan esos valores al Q1 y al Q3: 2300-5100 = -2800 y 5700+5100 = 10800
    Esos son los máximos y mínimos "razonables" de la variable.
    Como mucho, el bigote del gráfico llega hasta ahí... si hay valores más allá, se representan como puntos.

- Puedo analizar 1 variable en relación con otras variables: Estudio de correlaciones
    Queremos ver si dos variables evolucionan en paralelo...
    O si para valores de una variable hay un cambio en los valores de otra variable.

En el 90% de los paneles que ponga en una herramienta de BI, voy a juntar variables.
Como habitualmente querré representar esa información en un gráfico... podremos trabajar con un máximo de 3/4 variables.

Otra cosa será que filtre los datos que represento en ese gráfico... mediante 800 variables si quiero..
En ese acaso no hago un estudio de 800 variables entre si... sino un estudio de 3/4 variables de un grupúsculo de mi población de estudio.

### 2 variables nominales
    Hay relación entre dónde se encuentra una persona y su sexo biológico en mis datos?
    Imaginad que vivo en un sitio donde el 50% de las personas son de sexo: Hombre y el 50% de las personas son de sexo: Mujer.

        Y sé que en un bus hay 50 personas
        La pregunta es... cuánta gente espero en el bus de cada sexo?

                        VALORES ESPERADOS A PRIORI                  El tema es que ahora voy... y cuento
                        si la ubicación 
                        NO ESTUVIERA RELACIONADA CON EL SEXO

                                Hombre    |    Mujer                Hombre    |    Mujer
                    BUS         25        |    25                     11      |    39            Me escama? Me resulta raro?
                    TREN        50        |    50                     46      |    54
                    TIENDA      100       |    100                    92      |    108
                    MEDICO      25        |    25                      3      |    47 ****
                                                                                Ginecólogo
                    Hay una relación entre estar en el médico y el sexo.
                    En el resto de casos no hay relación.


                                Hombre    |    Mujer
                    BUS         50%       |    50%
                    TREN        50%       |    50%
                    TIENDA      50%       |    50%
                    MEDICO      50%       |    50%




        En estadística están los coeficientes de correlación: Chi-cuadrado -> Coef. Phi, V de Cramer

        Esas variables que he puesto... de qué tipo son? cualitativas / nominales
        Y usamos esos coeficientes, la tabla de frecuencias de doble entrada, y podemos representarlo gráficamente mediante
                        frecuencias relativas     frecuencias absolutas
        - Diagrama de barras apiladas            / agrupadas
        - Diagrama de sectores divididos

###  Cuando tengo una variable nominal y una ordinal... puedo hacer un estudio de correlación? SI
    Siempre podré calcular esa misma tabla, y esos mismos estadísticos... y el mismo gráfico. ya que cualquier variable ordinal, la puedo tratar por definición como si fuera nominal

    Pero aquí puedo hacer un estudio adicional. Podemos hacer un estudio de comparación de medianas.

        Número de hijos (ninguno, pocos, muchos, demasiados) -> ORDINAL
        Población                                            -> NOMINAL

                            NINGUNO     POCOS      MUCHOS      DEMASIADOS     |      MEDIANA
            Madrid           1           5          3           1             |         POCOS
            Barcelona        2           3          4           1             |         MUCHOS
            Sevilla          3           2          3           2             |         MUCHOS
            Valencia         10          4          2           3             |         NINGUNO

        Y diría, como las medianas de cada grupo (clasificando por población) no coinciden, es que la población guarda relación con el número de hijos.

            Madrid suelo encontrar familias con pocos hijos
            Barcelona y Sevilla  suelo encontrar familias con muchos hijos
            Pero en Valencia suelo encontrar familias sin hijos.

        Podría representar un diagrama de barras... donde cada barra representase la mediana de cada grupo.

        Aunque mucho más habitual sería representar un diagrama de caja y bigote... agrupado por población.

### Variable nominal con una cuantitativa

Puedo hacer la misma tabla de frecuencias y estadísticos que con 2 nominales? Si pero no aporta
Puedo hacer la misma tabla de frecuencias y estadísticos que con 1 nominal y una ordinal? SI... siempre puedo crear intervalos en la cuantitativa.
Pero puedo hacer una cosa más: Un estudio de diferencia de medias

                    Media del Salario
        Madrid      2000
        Barcelona   2500
        Sevilla     3000
        Valencia    3500

        Por tanto, la gente de Madrid suele tener un salario menor que la gente de Barcelona, que a su vez tiene un salario menor que la gente de Sevilla, que a su vez tiene un salario menor que la gente de Valencia.

        Hay una clara relación entre la ciudad de residencia y el salario.

        Diagrama: 
            Boxplot múltiple
            Violín múltiple
            Gráfico de pirámide poblacional (está limitado a variables nominales DICOTÓMICAS: con solo 2 valores posibles)

### Variable ordinal con una cuantitativa

    Trato la ordinal como si fuera nominal al representarla

### 2 variables cuantitativas

    Diagrama de dispersión
      En el resto de diagramas se muestran datos agrupados (habitualmente sumas de frecuencias)

      En el de dispersión se representan casos sueltos (no agrupaciones)

### Podemos juntar más variables... 3

Al trabajar con cualquier herramienta de BI:
- Preparación de los datos en base a las preguntas que queremos responder
- Mostrar las tablas de datos / gráficos que nos ayuden a responder esas preguntas

En muchas ocasiones, montaremos gráficos (Dashboards, cuadros de mando) interactivos, que permitan:
- Representar datos en tiempo real... si es que estoy leyendo de las fuentes de datos en tiempo real
- Filtrar los datos que se representan, mediante formularios donde elijamos los valores de las variables que queremos representar


# Preparación de datos.

Partimos de datos muy preparados... Las herramientas de BI no son herramientas para hacer una preparación de datos profunda.
Quiero trabajar con un "modelo de datos" con la información bastante limpia y preparada...
Pero siempre me tocará hacerle retoques específicos a las preguntas que quiera responder:
- Reducir la escala de medición de una variable:
  - Cuantitativa -> Ordinal (Generar intervalos)
  - Ordinal -> Categórica
- Agrupar datos de una forma no contemplada en el modelo de datos

    Ventas por provincia:
        Madrid:             1000
        Barcelona:          1200
        Valencia:            800
        Sevilla:            1100
        Otros:              500 (Murcia, Toledo... )
    
    Provincias destacada/ más relevantes:
        Todas las que venden más de 800.

- Homogeneizar los datos para compararlos bien entre si:
  -  Cambios de escala \
  -  Cambios de base   / Cambio en unidad de medida
  -  Calcular datos nuevos en base a otros datos
        Ventas por persona que hago en cada comunidad autónoma
            Ventas globales por comunidad --> Ventas / Población ---> Ventas por persona
            Población de cada comunidad

- Tratamiento de VALORES PERDIDOS / AUSENTES

    Qué pasa si un dato no lo tengo en una columna de mi tabla?... que hago con el registro?
    - Puedo reemplazarlo por un valor (calculado desde la media/mediana/algoritmo de machine learning basado en árboles de decisión para rellenar el dato)...
    - O a veces no... simplemente el dato NO APLICA


    Persona         Número de mascotas      Tipo de la primera mascota
    Juan            1                       Perro
    María           2                       Gato
    Pedro           0                       No aplica
    Ana             1                       Gato
    Luis            3                       No lo conozco
    Carmen          2                       Gato

    El "no aplica"... es un valor más... a tratar o no.
    El "no lo conozco" que aporta a mi estudio? INCERTIDUMBRE!

    Tabla de frecuencias de los primeros tipos de mascota:
            Perro: 1
            Gato:  3
            No lo conozco: 1
            Total de registros: 5

        Cuál es la primera mascota que tiene la gente?
            Perro:  20%-40%             1 o 2 personas tienen perro
            Gato:   60%-80%             3 o 4 personas tienen gato
            Otros:  0-20%               0 o 1 personas tienen otro tipo de mascota

            Perros: 1
            Gatos:  3
            No sé:  1
                TOTAL: 5

Muchas veces, MUCHISIMAS, hay datos que me vienen vacíos... y he de clasificarlos para mi estudio.

- Formatos... Los formatos, a fechas... o a números... o a textos incluso, tienen que ver con la forma en la que se representan los datos... Habitualmente no forma parte de la transformación inicial de preparación

---


# Palabrotas en el mundo del dato:

- BBDD: Programas y los datos que gestionan esos programas en los entornos de PRODUCCIÓN de una empresa
   v
   ETL: Proceso de extraer datos de una fuente, transformarlos y cargarlos en otra fuente.
            Procesos online
            Procesos batch
            Hay muchas variantes de proceso ETL:
                ETL: Extraer, Transformar, Cargar
                ELT: Extraer, Cargar, Transformar
                ETLT: Extraer, Transformar, Cargar, Transformar
                TELT: Transformar, Extraer, Cargar, Transformar
   v
         Hay poca T... en las ETLs.. guardamos los datos bastante en bruto.
   v
- Datalake: Es un almacén de datos (muchas veces lo implementamos con un programa de BBDD) históricos y no pensado para la actualización de los datos. Guarda datos en bruto.
   v
   ETL: La T es muy fuerte
   v
- Datawarehouse: Es un almacén de datos históricos, no preparado para su actualización. Pero con una diferencia con respecto al datalake... LOS GUARDO YA CON UN PROPOSITO muy concreto.
    Muchas veces también usamos programas de gestión de BBDD para almacenar los datos, pero los configuramos de otra forma... y los modelos de datos que montamos son muy diferentes a los de una BBDD de producción (muy orientada a la actualización de los datos) 

Los datos cumplen una función principal.. inherente al dato.
En ocasiones, podemos usar esos datos para otro tipo de funciones.

| - Business Intelligence   Toma de decisiones informadas (basadas en datos) dentro de la empresa
|                           Es la forma más básica de análisis del dato.
|                           Aplicamos técnicas de estadística descriptiva (nivel instituto)
|                           - Se analiza variable a variable y **correlaciones entre variables**
|                           El problema del estudio de relaciones entre datos que hacemos en BI es que
|                           lo hacemos los humanos... y nuestro cerebro está muy limitado 
|                           (nos da para pensar en 3/4 variables máximo)
|                           Lo que vamos a querer es usar datos pasados para tomar decisiones 
|                           (a día de hoy o de cara al futuro)
|                           - Eso es hacer una predicción: De entrada supongo que el mundo seguirá igual!
|                             Lo que me ofrecen los datos es la capacidad de TASAR la probabilidad de
|                             cometer un error...  (para limitarla)
|                             (ojo... siempre y cuando, el mundo siga igual.. que ya es mucho suponer)
|                           Aquí entra el mundo de la estadística INFERENCIAL
-----.-----.-----.-----.-----.----- ^^^ como mucho 3/4 variables a la vez en un análisis
-----.-----.-----.-----.-----.----- vvv a partir de 4/5 variables... hasta decenas o incluso cientos/miles
| - Data mining             Es cuando queremos buscar relaciones complejas NO EVIDENTES 
|                           entre las variables. NO SE A PRIORI NI LO QUE BUSCO NI LO QUE VOY A ENCONTRAR
| - Machine learning        Trata de aprender de los datos y hacer predicciones a futuro
|                           - Predecir un dato
|                           - Clasificar un sujeto
|                           - ...
v
Por orden de complejidad en el análisis de los datos.

- Bigdata
  - Un volumen "grande" de datos.
    - 1 millón de datos es grande?
    - 10 millones de datos es grande?
    - 100 millón de datos es grande?
    - 1000 millón de datos es grande?
    - 100000 millón de datos es grande?

Dónde pongo el corte???

El concepto de bigdata es:
Cuando tengo que hacer un trabajo (el que sea) con datos y las técnicas (herramientas de software, computadoras) que hay a mi disposición y que hemos usado tradicionalmente dejan de servirme... tengo un problema.
Y entonces busco soluciones alternativas. Los primeros que hicieron esto fue la gente de Google.

Se basa en utilizar granjas de computadoras pequeñas (commodity hardware) y hacer que trabajen como si fueran una. Para qué?

Trabajo el que sea:
- Almacenar un dato
- Analizar un dato
- Transmitir un dato
- ...

---

4 jugadores:
1 jugador en una partida puede hacer fácilmente 2 movimientos por segundo.
De su teléfono deben salir mensajes que al final lleguen a los teléfonos de los otros participantes.
2 mensajes por segundo... por persona... somo somos 4 jugando: 24 mensajes por segundo.
    Mis 2 mensajes emitidos, se convierten en 6 mensajes recibidos.
Si tenemos en cuenta que durante muchos años fué el juego más juagdo de Apple Store... y de Google play... y que un momento dado podía haber más de 50k partidas en paralelo a nivel mundial: 
50k * 24 = 1.2 millones de mensajes por segundo.
Qué computadora es capaz de procesar tal cantidad de mensajes por segundo? NO EXISTE

---

Si tengo un pincho USB de 16 Gbs... vacío... recién sacado de la caja.. y quiero guardar en él un fichero de 5Gbs, puedo? Depende del formato.
    FAT16/FAT32... no entra, aunque haya espacio disponible TOTAL
    NTFS, EXT4.... ahí si... pero incluso esos formatos tienen un límite.
    Si quiero guardar un fichero de 2Pbs... no puedo... no hay formato tradicional que lo soporte.














----

Ejemplo 1:

- Sease villa-arriba de arriba...
    Viven 10 vecinos... cada uno con su coche... que contaminan:
        5 5 5 10 10 10 10 15 15 15                                         X
                                                                        X  X  X
            Cuánto vale la media? 10                                    X  X  X
            Cuánto vale la mediana? 10                                  X  X  X
                                                                      ------------> 
- Sease villa-arriba de abajo                                           5 10  15
   Viven 10 personas... y están muy sensibilizadas con el medio ambiente:
        5 5 5 5 5 5 5 5 5 1000
                                                                        X
                                                                        X
                                                                        X
                                                                        X
            Cuánto vale la media? 104,5                                 X                                             x  
            Cuánto vale la mediana? 5                                 --------------------------------------------------->
                                                                        5 10 15           ...                        1000
    Qué dato representa mejor la esencia del grupo (pueblo), lo sensibilizado que está con el medio ambiente? La mediana


    Cuando tengo datos que se distribuyen simétricamente (como en el primer caso), la media y la mediana son iguales.

    La media es muy engañosa... se ve muy afectada por los valores extremos.


---

Ejemplo 2: Estadísticos de dispersión:

- Portal 12 de la calle... donde viven 4 personas... y quieren comprar unos bancos... para sentarse. 2 bancos... Hay talla S, M, L

    En ese portal las alturas de las personas son:
        1.70 1.70 1.70 1.70

            Media? 1.70 ... cómo compro los bancos atendiendo a la media? M

        0     0     0    0  ... y ahora podría hacer la media de estas diferencias a la media? 0
                                                    Dev. típica = raíz cuadrada de la varianza = 0
                                                    Al menos el 50% de la gente mide entre: 1.70 - 0 y 1.70 + 0
                                                                                                        1.70 y 1.70

- Portal 14... mismo rollo

    En ese portal las alturas de las personas son:
        1.50 1.90 1.90 1.50

            Media? 1.70 ... cómo compro los bancos atendiendo a la media? M

        400   400   400  400  ... y ahora podría hacer la media de estas diferencias a la media? 400
                                                    Dev. típica = raíz cuadrada de la varianza = 20
                                                    Al menos el 50% de la gente mide entre: 1.70 - 20 y 1.70 + 20
                                                                                                        1.50 y 1.90

- Portal 16... misma aventura

    En ese portal las alturas de las personas son:
        1.50 1.70 1.70 1.90

            Media? 1.70 ... cómo compro los bancos atendiendo a la media? M

        400   0    0    400  ... y ahora podría hacer la media de estas diferencias a la media? 200:
                                                    Dev. típica = raíz cuadrada de la varianza = 14.14
                                                    Al menos el 50% de la gente mide entre: 1.70 - 14.14 y 1.70 + 14.14
                                                                                                        1.56 y 1.84
            
- Portal 18... misma aventura

    En ese portal las alturas de las personas son:
        130 170 170 210  centimetros

            Media? 1.70 ... cómo compro los bancos atendiendo a la media? M

        1600   0    0   1600  ... y ahora podría hacer la media de estas diferencias a la media? 800 m2 <- VARIANZA (metros cuadrados)
                                                                Desv. típica = raíz cuadrada de la varianza = 28.28 centimetros
                                                                Al menos el 50% de la gente mide entre: 1.70 - 28.28 y 1.70 + 28.28
                                                                                                        141.72 y 198.28

En qué se parecen los individuos de los portales 12, 14 y 16 entre si? EN NADA... pero todas tienen la misma media...
Y el cerebro humano es engañoso... en cuantito leemos una media, tendemos a pensar que todos los datos están por ahí.

    Para resolver ese cuadrado raro.. y al menos que la medida de dispersión esté en la misma unidad que la medida de tendencia central, le hacemos la raíz cuadrada... y obtenemos la DESVIACIÓN TÍPICA

Cómo se interpreta la desv. típica? Regla de Chevichev: Sea cual sea la variable, tiene garantía de que al menos el 50% de los sujetos está a una distancia inferior a 1 desv. típica de la media.