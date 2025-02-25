# Cuánto ocupa una columna en mi conjunto de datos.

## Esto es relevante?

- Impacto en el rendimiento
  - Tiempo de lectura / escritura en HDD?
  - Se tarda lo mismo en mandar más o menos datos por una red?
  - Se tarda lo mismo en cargar esos datos en la memoria RAM del servidor?
  - Se tarda lo mismo en procesarlos a nivel de la cpu? 
  (En los clouds me cobran por uso de RAM, CPU y red) 
- Impacto directo en €€€? Almacenamiento
  El almacenamiento es lo más caro que existe en un entorno de producción.
    Los HDD en una empresa, donde van a estar sometidos a una fuerte carga de trabajo tienen que ser de otra calidad... Y cuestan entre 3 y 10 veces más que un HDD normal.
    Cuántas copias hago de un dato en un entorno de producción? Al menos 3 -> Alta disponibilidad del dato
    Pero... además luego tenemos otra cosa... los backups -> Evitar corrupción de datos (automática o manual). Tendré guardados backups de las últimas N semanas (2)

    1Tb de almacenamiento (casa me cuesta 30€)
    En una empresa necesito 1Tb x 3 = 3 Tbs
                            1Tb x 2 x 2 = 4 Tbs
                            7Tbs... de almacenamiento CARO de narices (100€/tera)
    En casa es 30€
    En una empresa es 700€
---

12,40727903		580 nombres		16500 filas

Un carácter ocupa al menos 1 byte al guardarlo... depende del carácter puede ocupar más información
1 nombre de media ocupa 12 bytes
En nuestro fichero eso implica: 12 * 16500 = 198000 bytes = 200Kbs

Pero esa columna la podríamos haber guardado CODIFICADA.. con un número que represente a cada nombre.
Necesitaríamos números de 0 a 580.

En un byte cuántos valores diferentes puedo representar? 2^8 = 256
En dos bytes? 2^16 = 65536
En general muchos de estos sistemas cuando guardo números enteros requieren 4 bytes para guardar un número entero.
Eso significa que si hubiera guardado los datos como números, en mi fichero ocuparían 4 * 16500 = 66000 bytes = 66Kbs

Lo acabo de dejar en un tercio.

---

Siempre, cuando tengo un conjunto de datos, las variables:
- Ordinales SIEMPRE COMO NUMERICAS ! Codificadas como números
- Nominales... depende un poco. Si son datos medio identificativos... no tiene sentido codificarlas. Si son datos que se repiten mucho... sí tiene sentido codificarlas.
---


                   Genero
                     v
        Editorial > Juego < Año
                     v
                  Ventas
                     ^
                Plataforma

---

Ficheros CSV (Excel): Son archivos de texto
Ficheros parquet    : Son archivos binarios


El número 54326 en un csv ocupa 5 bytes
                en un parquet ocupa 2 bytes / 4 bytes
          100.000.000 en un csv ocupa 9 bytes
                en un parquet ocupa 4 bytes

Otro motivo importante es que los archivos parquet incluyen lo que denominamos el ESQUEMA de los datos.
La estructura de los datos está guardada en el propio fichero.

--- 
                                                    Qué puedo obtener?
Nombre              Nominal                             MODA (juego con más plataformas)
Plataforma          Nominal                             MODA (plataforma con más juegos)
Año                 Cuantitativo: Intervalo             MODA (año con más juegos)
                                                        MEDIANA (año que antes de él y después de él se publicaron la misma cantidad de juegos)
                                                        MEDIA (año promedio de publicación de juegos ???)
                                                        MAXIMO** \
                                                        MINIMO** / RANGO de estudio
                                                        SUMA
Genero              Nominal                             MODA (género con más juegos)
Editorial           Nominal                             MODA (editorial con más juegos)          

Ventas NA           Cuantitativos: Razón                MEDIA, MEDIANA, MAXIMO, MINIMO, SUMA
Ventas EU           Cuantitativos: Razón
Ventas JP           Cuantitativos: Razón
Ventas Otros        Cuantitativos: Razón
Ventas Global       Cuantitativos: Razón

Qué preguntas queremos responder sobre los datos.

La(s) variable(s) de estudio principal: Ventas en sus distintas regiones.

Ventas (cuantitativo) x Nominales (Género, Plataforma, Año, Editorial, Nombre)
- Genero: Géneros que más pasta dan ( y los que menos)
- Plataforma: Plataformas que más pasta dan ( y las que menos)
- Año: Años que más pasta dan ( y los que menos)
- Editorial: Editoriales que más pasta dan ( y las que menos)

Cómo puedo representar eso?
    Ventas x Género = Barras (En vertical pongo suma ventas, en horizontal pongo género)
                        -> Si además me interesa el valor absoluto
                        -> en general puedo representar más datos.
                      Sectores (En un círculo pongo la suma de ventas, y lo divido en sectores según el género)
                        -> Si me interesa la comparativa entre géneros
Lo puedo hacer para cada región. (en nuestro caso, son variables diferentes... y en global)

Ventas por género y plataforma
    Los géneros que venden más en cada plataforma
    Las plataformas que venden más en cada género
    -> barras apiladas  (valores relativos) (comparativa)
    -> barras agrupadas (valores absolutos)

Ventas por género y año
    Los géneros que venden más en cada año
    Los años que más venden en cada género
    Ver cambios de tendencia... según pasan los años... si hay géneros que dejan de vender... o que empiezan a vender más

Ventas x plataforma x año x genero
    Quiero ver si hay un cambio de tendencia para los géneros que triunfan en una plataforma

    Cómo represento esto?
    15 géneros x 20 plataformas x 40 años = 12000 datos
    Posiblemente me interese montar un filtro que pueda cambiar dinámicamente la información que estoy viendo.
     Para centrarme en una plataforma

Ventas x género x año x zona
    Cambios de tendencia de los géneros en cada zona
Ventas x plataforma x año x zona
            20      x 40  x 4 = 3200 datos <   FILTRO (zona/plataforma)
    Cambios de tendencia de las plataformas en cada zona

Una variable que siempre vamos a tener en cuenta en los estudios es cuál? TIEMPO!!!

---

Toma de decisiones INSTANTANEAS en base a la información que voy analizando > VAMOS BIEN???? VAMOS MAL????
DATO DE REFERENCIA !

En los cuadros de mando que montaré MUCHAS VECES METO REFERENCIAS ! Lo que espero... el objetivo.

Quiero este año facturar en cada zona al menos un 120% del año anterior... cada editorial
De cada combinación que haga:
    editorial x zona x año => VENTAS , necesito el valor de referencia del año anterior => VENTAS
    JOIN de la tabla consigo misma, haciendo el join por el campo editorial, zona y año - 1

Y aquí entran las peculiaridades de cada herramienta.
- Hay herramientas que me permiten hacer ese tipo de joins de forma dinámica.... y otras que no (como Amazon QuickSight)
- En algunas herramientas necesitaremos a priori tener la información ya preparada(como Amazon QuickSight)... y en otras no.
- En general SIEMPRE me interesará tener la información preparada... porque el tiempo de respuesta será mucho menor.
    - Eso si... con un coste de almacenamiento mayor.

Puede ser que necesite de antemano restar 1 al año... para crear la columna: Año de referencia