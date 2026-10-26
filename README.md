# Comparar la implementación de un diccionario de datos físico en diferentes SGBD

**Proyecto:** Red Social Estudiantil Pascualina\
**Institución:** Institución Universitaria Pascual Bravo\
**Facultad:** Ingeniería\
**Asignatura:** Bases de Datos 1\
**Año:** 2026

## Integrantes

-   Luis Rojas Cano
-   Emily Quintero Rivera
-   Sofia Hidalgo Córdoba
-   Jeiner Andrés Mosquera

**Docente:** Oralia del Socorro Cortés Grajales

## 1. Introducción

El diseño de bases de datos permite organizar la información de un
sistema y establecer relaciones entre sus elementos. En el caso de la
red social Pascualina, se busca crear una estructura que permita
gestionar la información de los estudiantes y las actividades académicas
y sociales que realizan dentro de la red.

Para desarrollar este trabajo se tomó como base el modelo lógico del
proyecto y se transformaron sus entidades en tablas, sus atributos en
columnas y sus relaciones en claves y restricciones.

La implementación se realizó en MySQL, SQL Server y PostgreSQL. Aunque
los tres sistemas permiten administrar bases de datos relacionales, cada
uno presenta diferencias en los tipos de datos y en la forma de definir
determinadas restricciones.

## 2. Objetivos

### Objetivo general

Implementar el modelo físico de la base de datos de la Red Social
Estudiantil Pascualina en MySQL, SQL Server y PostgreSQL, conservando la
estructura lógica del proyecto y estableciendo las relaciones necesarias
para mantener la integridad de la información.

### Objetivos específicos

-   Transformar las entidades del modelo lógico en tablas relacionales.
-   Definir los tipos de datos apropiados para los campos.
-   Implementar claves primarias, foráneas y únicas.
-   Resolver las relaciones muchos a muchos mediante tablas intermedias.
-   Incorporar el campo semiestructurado `datos_adicionales`.
-   Revisar la creación de las tablas y guardar evidencias de la
    ejecución.
-   Documentar las características físicas de cada sistema gestor
    mediante diccionarios de datos.

## 3. Alcance del proyecto

Pascualina está diseñada como una red social exclusiva para estudiantes.
El modelo contempla entidades relacionadas con los estudiantes y sus
interacciones:

-   Información básica de estudiantes y sus perfiles.
-   Publicaciones, comentarios y reacciones.
-   Intereses y grupos de estudio.
-   Seguimientos entre estudiantes.
-   Eventos e inscripciones.
-   Chats y participación en ellos.
-   Cursos compartidos.
-   Investigaciones e integrantes participantes.

No se incluyen profesores ni personal administrativo, porque están fuera
del alcance definido para el proyecto.

## 4. Sistemas gestores utilizados

  -----------------------------------------------------------------------
  Sistema gestor                      Características de la
                                      implementación
  ----------------------------------- -----------------------------------
  **MySQL**                           Utiliza tablas relacionales, claves
                                      y restricciones SQL. Los
                                      identificadores autogenerados usan
                                      `AUTO_INCREMENT`; el campo
                                      semiestructurado utiliza `JSON`.

  **SQL Server**                      Los identificadores usan
                                      `IDENTITY(1,1)`. Se utiliza
                                      `DATETIME2` y `NVARCHAR(MAX)` para
                                      almacenar los datos adicionales
                                      validados con `ISJSON`.

  **PostgreSQL**                      Utiliza columnas `IDENTITY`,
                                      `TIMESTAMP` y `JSONB`.
  -----------------------------------------------------------------------

La adaptación entre gestores es necesaria porque algunos tipos de datos
y mecanismos de validación se escriben de manera diferente en cada
sistema.

## 5. Inventario de tablas

La base de datos conserva las 18 tablas definidas para el proyecto.

  -------------------------------------------------------------------------------
                           N.º Tabla                        Propósito
  ---------------------------- ---------------------------- ---------------------
                             1 `estudiante`                 Almacena los datos
                                                            básicos de cada
                                                            estudiante.

                             2 `perfil`                     Contiene información
                                                            complementaria del
                                                            estudiante.

                             3 `publicacion`                Almacena las
                                                            publicaciones
                                                            realizadas en la red.

                             4 `comentario`                 Registra los
                                                            comentarios asociados
                                                            a publicaciones.

                             5 `reaccion`                   Registra las
                                                            reacciones de los
                                                            estudiantes a
                                                            publicaciones.

                             6 `interes`                    Contiene el catálogo
                                                            de intereses.

                             7 `estudiante_interes`         Relaciona estudiantes
                                                            con sus intereses.

                             8 `grupo`                      Registra grupos de
                                                            estudio o comunidades
                                                            estudiantiles.

                             9 `membresia`                  Registra la
                                                            pertenencia de
                                                            estudiantes a grupos.

                            10 `seguimiento`                Representa los
                                                            seguimientos entre
                                                            estudiantes.

                            11 `evento`                     Almacena eventos
                                                            creados por
                                                            estudiantes.

                            12 `inscripcion_evento`         Registra estudiantes
                                                            inscritos en eventos.

                            13 `chat`                       Representa las
                                                            conversaciones o
                                                            espacios de chat.

                            14 `participacion_chat`         Relaciona estudiantes
                                                            con los chats en los
                                                            que participan.

                            15 `curso_compartido`           Contiene el catálogo
                                                            de cursos
                                                            compartidos.

                            16 `estudiante_curso`           Relaciona estudiantes
                                                            con cursos.

                            17 `investigacion`              Registra iniciativas
                                                            o temas de
                                                            investigación.

                            18 `estudiante_investigacion`   Relaciona estudiantes
                                                            con investigaciones.
  -------------------------------------------------------------------------------

## 6. Tipos de datos y justificación

  -----------------------------------------------------------------------
  Tipo de dato            Uso en Pascualina       Justificación
  ----------------------- ----------------------- -----------------------
  `INT` o `INTEGER`       Identificadores y       Facilita identificar
                          referencias entre       registros y
                          tablas.                 relacionarlos.

  `VARCHAR(n)`            Nombres, apellidos,     Permite almacenar
                          documentos, correos,    textos con una longitud
                          títulos y categorías.   máxima definida.

  `TEXT` o `VARCHAR(MAX)` Contenido de            Permite almacenar
                          publicaciones y         textos extensos y
                          comentarios.            variables.

  `DATE`                  Fechas de eventos,      Almacena fechas sin
                          ingresos e              componente horario.
                          inscripciones.          

  `TIME`                  Hora de los eventos.    Almacena la hora por
                                                  separado de la fecha.

  `DATETIME`, `DATETIME2` Fechas de creación,     Permite registrar fecha
  o `TIMESTAMP`           publicaciones y         y hora en un mismo
                          comentarios.            campo.

  `JSON` o `JSONB`        Datos adicionales de    Permite almacenar
                          publicaciones.          información
                                                  complementaria
                                                  semiestructurada.
  -----------------------------------------------------------------------

Los tamaños de los campos de texto se establecieron según las
necesidades del modelo. Los campos que no son obligatorios, como algunas
descripciones y fotografías, pueden aceptar valores nulos cuando así
está definido en el script.

## 7. Campo semiestructurado `datos_adicionales`

La tabla `publicacion` contiene el campo `datos_adicionales`, destinado
a almacenar información complementaria que puede variar entre
publicaciones.

  -----------------------------------------------------------------------
  Sistema gestor          Tipo utilizado          Forma de validación
  ----------------------- ----------------------- -----------------------
  MySQL                   `JSON`                  Utiliza el tipo JSON
                                                  nativo.

  SQL Server              `NVARCHAR(MAX)`         Se incorporó una
                                                  restricción con
                                                  `ISJSON` para validar
                                                  el contenido cuando no
                                                  es nulo.

  PostgreSQL              `JSONB`                 Utiliza el tipo JSONB
                                                  para almacenar datos
                                                  semiestructurados.
  -----------------------------------------------------------------------

Este campo permite guardar metadatos variables sin tener que crear una
columna diferente para cada característica adicional. Por ejemplo, una
publicación podría incluir información complementaria sobre un recurso
académico o un material compartido.

El campo no reemplaza los atributos principales de la publicación. Los
datos esenciales del modelo deben continuar en sus columnas
correspondientes.

## 8. Claves y relaciones

Las claves y restricciones ayudan a conservar la coherencia entre los
datos:

-   **Clave primaria (PK):** identifica de manera única cada registro de
    una tabla.
-   **Clave foránea (FK):** conecta un registro con otro registro
    existente en una tabla relacionada.
-   **Clave única (UK):** evita la repetición de valores que deben ser
    únicos.
-   **Clave primaria compuesta:** se forma con varios campos y permite
    identificar de manera única una asociación.

Por ejemplo, la tabla `estudiante` utiliza `id_estudiante` como clave
primaria. Los campos `documento` y `correo` tienen restricciones de
unicidad. En `perfil`, `id_estudiante` es una clave foránea y también es
único, de manera que un estudiante no pueda tener más de un perfil
asociado.

### Relaciones muchos a muchos

  ----------------------------------------------------------------------------
  Tablas relacionadas     Tabla intermedia             Función
  ----------------------- ---------------------------- -----------------------
  Estudiante e interés    `estudiante_interes`         Relaciona cada
                                                       estudiante con sus
                                                       intereses.

  Estudiante y grupo      `membresia`                  Registra la pertenencia
                                                       a grupos y el rol
                                                       correspondiente.

  Estudiante y estudiante `seguimiento`                Registra quién sigue a
                                                       quién.

  Estudiante y evento     `inscripcion_evento`         Registra las
                                                       inscripciones a
                                                       eventos.

  Estudiante y chat       `participacion_chat`         Registra los
                                                       participantes de cada
                                                       chat.

  Estudiante y curso      `estudiante_curso`           Relaciona estudiantes
                                                       con cursos.

  Estudiante e            `estudiante_investigacion`   Relaciona estudiantes
  investigación                                        con investigaciones.
  ----------------------------------------------------------------------------

Las claves primarias compuestas de estas tablas evitan repetir una misma
asociación con los mismos identificadores.

## 9. Normalización

La normalización permite organizar la información y reducir
duplicaciones innecesarias.

-   **Primera forma normal (1FN):** los atributos almacenan valores
    individuales. Los intereses, cursos, grupos y participaciones no se
    guardan como listas dentro de una única columna, sino que se
    representan mediante tablas propias y tablas intermedias.
-   **Segunda forma normal (2FN):** en las tablas que utilizan claves
    primarias compuestas, los atributos adicionales describen la
    asociación completa. Por ejemplo, `fecha_ingreso` y `rol_grupo`
    corresponden a la pertenencia de un estudiante a un grupo.
-   **Tercera forma normal (3FN):** los datos descriptivos de
    estudiantes, grupos, intereses y cursos se conservan en sus tablas
    correspondientes. Las tablas intermedias almacenan los
    identificadores relacionados, evitando repetir innecesariamente
    nombres y descripciones.

Estas decisiones ayudan a mantener la información organizada y facilitan
las modificaciones posteriores.

## 10. Restricciones de integridad y diferencias entre gestores

  -----------------------------------------------------------------------
  Restricción o característica        Aplicación en Pascualina
  ----------------------------------- -----------------------------------
  Claves primarias                    Identifican los registros de cada
                                      tabla.

  Claves foráneas                     Mantienen las relaciones entre
                                      estudiantes, publicaciones, grupos,
                                      eventos y demás entidades.

  Restricciones `UNIQUE`              Evitan duplicar documentos,
                                      correos, nombres de intereses y
                                      nombres de grupos.

  Clave única en                      Limita a un perfil por estudiante.
  `perfil.id_estudiante`              

  Claves compuestas en tablas puente  Evitan repetir asociaciones con los
                                      mismos identificadores.

  Validación de `datos_adicionales`   Utiliza mecanismos específicos de
                                      cada sistema gestor.

  Restricción contra el               Está definida en SQL Server y
  auto-seguimiento                    PostgreSQL; no se incorporó en la
                                      versión MySQL ejecutada.
  -----------------------------------------------------------------------

Las acciones de actualización y eliminación de las claves foráneas se
mantienen según lo definido en cada script. Estas reglas pueden variar
entre gestores y deben documentarse a partir del código ejecutado.

La tabla `reaccion` no tiene una restricción `UNIQUE` que limite a una
sola reacción por estudiante y publicación. Por lo tanto, no debe
afirmarse que la base de datos impide esa situación mediante una
restricción de unicidad.

## 11. Limitaciones y mejoras futuras

El modelo actual contiene las tablas `chat` y `participacion_chat`, que
permiten representar conversaciones y sus participantes. Sin embargo, no
incluye una tabla para almacenar el contenido de los mensajes enviados.

Por esta razón, la estructura representa la participación en
conversaciones, pero todavía no implementa un sistema completo de
mensajería. Si el alcance futuro requiere enviar, consultar y conservar
mensajes, se deberá ampliar primero el modelo lógico y posteriormente
actualizar los scripts físicos.

Otra mejora consiste en probar las reglas de eliminación y actualización
para confirmar que el comportamiento corresponde con las necesidades del
sistema. También se recomienda verificar la validación de los datos
adicionales y definir ejemplos válidos para el campo
`datos_adicionales`.

## 12. Conclusiones generales

La implementación del modelo físico de la red social Pascualina permitió
transformar el modelo lógico en una estructura relacional de 18 tablas
para MySQL, SQL Server y PostgreSQL.

La utilización de claves primarias, foráneas, restricciones de unicidad
y tablas intermedias permite representar las relaciones entre
estudiantes, publicaciones, intereses, grupos, eventos, chats, cursos e
investigaciones. Estas estructuras contribuyen a mantener organizada la
información y a reducir las asociaciones inválidas.

La implementación también permitió reconocer que, aunque los tres
gestores pueden representar el mismo modelo lógico, existen diferencias
en los tipos de datos y en la definición de determinadas restricciones.
El campo `datos_adicionales` evidencia esta situación mediante el uso de
`JSON` en MySQL, `NVARCHAR(MAX)` con validación `ISJSON` en SQL Server y
`JSONB` en PostgreSQL.

## 13. Conclusiones individuales del equipo

Cada integrante debe revisar y adaptar su conclusión para que describa
su participación real en el trabajo.

-   **Luis Rojas Cano:** fue difícil comprender las diferentes formas de
    redactar los comandos para ejecutar el script en cada SGBD.
-   **Emily Quintero Rivera:** fue difícil ejecutar los scripts de MySQL
    y SQL Server en compiladores en línea, por lo que fue necesario
    descargar las aplicaciones, ya que algunos compiladores en línea
    tienen restricciones de caracteres.
-   **Sofia Hidalgo Córdoba:** aprendió que el diccionario de datos
    sirve para documentar la estructura física de la base de datos y que
    el campo `datos_adicionales` se maneja de forma distinta en cada
    gestor (`JSON`, `NVARCHAR` con `ISJSON` y `JSONB`).
-   **Jeiner Andrés Mosquera:** fue difícil adaptar el script a cada
    gestor; por ejemplo, SQL Server no aceptó algunas claves en cascada
    que MySQL sí permitió y fue necesario corregirlas.
