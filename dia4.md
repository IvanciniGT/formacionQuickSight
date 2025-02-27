
# IA

El tema de las IAs no es nada nuevo... últimamente se ha puesto muy de moda.
Pero llevamos usando técnicas para conseguir IAs décadas.

En general el conceptode IA es ser capaces de hacer un programa que simule comportamientos de razonamiento humanos:
- Los programas basados en reglas predeterminadas (LIMITADOS)
  Esas reglas las creamos los humanos. 
- Los programas generados por las propias computadoras: Machine learning
    - Tengo un huevo (o no) de datos históricos 
    - Y la computadora los analiza con un objetivo en mente.
        Trata de generar una ecuación matemática que ayude a calcular un valor (Conocido de antemano para el conjunto de datos inicial/históricos) de forma que la tasa de éxito de esa ecuación sea la mayor posible.

## Machine Learning

Para qué usamos estos programas?
- Predicciones
- Clasificaciones

Y eso es el 80% de la IA que existe en el mundo a día de hoy.
La que más nos ha aportado ... y sigue aportando.
Por ejemplo:
- Tengo mi email... y quiero que se me clasifiquen los correos por:
    - Spam
    - No spam
    - Redes sociales
    - Publicidad
    - Importantes

Un nuevo uso que ha surgido recientemente es el de la generación de datos nuevos.

    Tradicional( predicción / Clasificación)
        DATOS HISTÓRICOS 
            Xs ---> Y
            Datos de una persona (demográficos, sociales...) ---> Si va a pagar la hipoteca en el futuro.

            Nuevos Xs ---> Y'

    Hoy en día: IAs Generativas (Redes neuronales)
        DATOS HISTORICOS
            Xs ---> Y
        
            Foto (1 millón de pixels: 3 millones de variables) -> Y : Manzana

        Lo que le pedimos a un programa es que genere nuevos Xs que no existen en la realidad.

            Y' ---> X's
            Manzana -> X`s (Foto de la manzana.. que no existe en la realidad)

    GPT-3 -> 100 millones de euros en el entrenamiento del modelo


    Xs -> Y -> X's
          Y -> X's


-----

Se podría usar dentro de una app web, o de el portal corporativo de una empresa.
Y a ese portal o app accederé mediante una URL, que tiene un dominio: (intranet.bbva.es)
https://intranet.bbva.es/portal/panel17QuickSight

    <iframe
        width="960"
        height="720"
        src="https://eu-west-1.quicksight.aws.amazon.com/sn/embed/share/accounts/820634527231/dashboards/60d434a4-558b-4a9e-badd-89382b5e282c?directory_alias=Formacion">
    </iframe>


Usuarios de AWS (Roles para ello)
                Cuenta de usuario -> Quicksight
Empleados que tengo en la empresa        |   ^
    LDAP / Active Directory <------------+   |
    Proveedor de Identidad completo (IAM): Keycloak, Okta, Auth0

---

# Seguridad de acceso a los datos:

Otorgar permiso de acceso al conjunto de datos:
- Usuarios              A un usuario le permito que acceda / administre mi conjunto de datos
- Grupos                A un grupo (y por ende a todos los usuarios de ese grupo) le permito que acceda / administre mi conjunto de datos
- Paneles/Análisis      A un panel o análisis le permito que acceda a mi conjunto de datos
                        ^ Cualquier persona que tenga acceso al panel, puede ver los datos de mi conjunto de datos.

Otra cosa es a qué datos pueden acceder los Usuarios/Grupos de mi conjunto de datos, ya sea:
- Accediendo directamente al conjunto de datos
- Accediendo a través de un panel/análisis

En este caso es donde filtramos:
- Columnas              Lo que puedo es restringir ciertas columnas a ciertos usuarios/grupos
                        Genero: Publica
                        Ventas_Europa: Usuarios del grupo Europa
                        Ventas_America: Usuarios del grupo America
                        Ventas_Japón: Usuarios del grupo Japón
                        Ventas_Totales: Federico y Ramón y Menchu!

                        Nombre: Es un dato sensible y no quiero que la gente pueda verlo.... Salvo auditores
                                El resto necesitan ver un identificador raro... que no les dé información sensible, pero que si permita identificar al sujeto.

                                Autoditores
                                    ID    Nombre             Edad      Otros datos
                                    as    Iván                47
                                    asd   Bárbara             24
                                Resto
                                    ID                                          Otros datos
                                    DSAFKJHSADFGKL·$")=REWT
                                    flkjd lhjfdash hljkfsdakhj dsfa hkjdsfahkjl


    Cuántas columnas tengo en un Dataset? POCAs (decenas)

- Filas / Registros
    
    Cuántas filas tengo en un Dataset? POTENCIALMENTE MILLONES

  - Limitarla en base a usuario/grupo
      - Qué usuarios /grupos de usuarios pueden ver qué filas?
              Necesitamos es un CONJUNTO NUEVO DE DATOS con las restricciones

                    USUARIO  Genero    PLATAFORMA
                    Bárbara  Acción    PC
                    Iván               PC
                    Menchu   Acción    

                Y posteriormente aplicamos esa tabla sobre la otra, para configurar los permisos de acceso.

        Problema que tiene esto: MANTENIMIENTO < QUICKSIGHT

  - Securizar en base a ETIQUETAS

        Quiero una regla que diga:
            Cuando los usuarios tengan etiqueta JUEGOS_GENERO="" pueden ver todas las filas independientemente del género que sea
            Un usuario con etiqueta JUEGOS_GENERO="ACCION|PLATAFORMA" sólo puede ver las filas que tengan GENERO="ACCION"

            SHOOTING_Objetos
            SHOOTING_Personas


---


    EMPRESAS < PERSONAS <   INSCRIPCIONES >     CURSOS
                            Persona             Curso
                            Curso               Importe
                            Fecha
                            Aprobado o no

    En esta tabla Inscripciones tengo 1M registros en 2 años de operación.

Si quiero saber:
    - La gente se inscribe más a principio de mes o a final de mes?

    EMPRESAS < INSCRIPCIONES > CURSOS
                Empresa
                Curso
                Número de Inscripciones                                                 \
                Número de Aprobados                                                      >  Frecuencias
                Número de Suspensos = Número de Inscripciones - Número de Aprobados     /   Recuentos
                Intervalo de Fecha (Enero-2024, Febrero-2024, Marzo-2024, Abril-2024, Mayo-2024, Junio-2024, Julio-2024, Agosto-2024, Septiembre-2024, Octubre-2024, Noviembre-2024, Diciembre-2024)

Por ejemplo me sirve para:
    - Analizar tendencias de cómo varian las inscripciones a lo largo del tiempo para las diferentes empresas
    - Analizar tendencias de cómo varian las inscripciones a lo largo del tiempo para los diferentes cursos


    EMPRESAS < INSCRIPCIONES > CURSOS
                Empresa
                Curso
                Número de Inscripciones                                                 \
                Número de Aprobados                                                      >  Frecuencias
                Número de Suspensos = Número de Inscripciones - Número de Aprobados     /   Recuentos
                Intervalo de Fecha 
                    1-e-2024
                    2-e-2024
                    3-e-2024

        Aquí la tabla inscripciones va a tener:
            Como mucho 24 x nº de empresas (quizás tengo 1000) = 24.000 registros
                                                                 En lugar de 1M

La estructura inicial me permite hacer TODO lo que quiera... pero no es la más eficiente.
La estructura final no me permite hacer TODO lo que quiera... pero es la más eficiente para algunas cosas.



SELECT "t"."220f638a-a489-46ac-8b7f-914221dce719.cif" AS "cif", "t"."220f638a-a489-46ac-8b7f-914221dce719.nombre" AS "empresa", "t0"."5cc881fd-c13b-469f-b4d4-3cab6912f3f1.numero_dni" AS "numero_dni", "t0"."5cc881fd-c13b-469f-b4d4-3cab6912f3f1.letra_dni" AS "letra_dni", "t0"."5cc881fd-c13b-469f-b4d4-3cab6912f3f1.nombre" AS "persona", "t0"."5cc881fd-c13b-469f-b4d4-3cab6912f3f1.apellidos" AS "apellidos", "t0"."5cc881fd-c13b-469f-b4d4-3cab6912f3f1.email" AS "email", "t1"."1f1c5486-c584-4af3-96cd-4a4fbf9c144d.fecha" AS "fecha", "t1"."1f1c5486-c584-4af3-96cd-4a4fbf9c144d.aprobado" AS "aprobado", "t2"."8ea1c9dd-58e5-46a5-8618-7914a26093ab.duración" AS "duración", "t2"."8ea1c9dd-58e5-46a5-8618-7914a26093ab.importe" AS "importe", "t2"."8ea1c9dd-58e5-46a5-8618-7914a26093ab.titulo" AS "curso"
FROM (
SELECT "id" AS "220f638a-a489-46ac-8b7f-914221dce719.id", "cif" AS "220f638a-a489-46ac-8b7f-914221dce719.cif", "nombre" AS "220f638a-a489-46ac-8b7f-914221dce719.nombre