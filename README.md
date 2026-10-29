# Proyecto BBDD 

Base de datos relacional en **PostgreSQL**, alojada en **Supabase**, creada a partir de los datos sin normalizar de los alumnos y el claustro de una escuela de bootcamps (Data Science y Full Stack).

## Realizado en equipo


## Entregables

| Entregable |    |
|---|---|
| Diagrama entidad-relación | 
| Diagrama del modelo lógico | 
| Script de creación de tablas |
| Script de carga de datos | 
| Consultas de demostración | 


## Datos de entrada

En [`data/`](data/) están los 5 CSV originales (separador `;`):

- `clase_1.csv` … `clase_4.csv`: 52 alumnos, con el resultado (Apto / No Apto) de sus 5 proyectos.
- `claustro.csv`: 10 docentes, con su rol (TA / LI), vertical, promoción, campus y modalidad.

## Modelo de datos

**9 tablas** normalizadas en 3FN:

| Tipo | Tablas |
|---|---|
| Catálogos | `especialidad`, `promocion`, `campus`, `docente` |
| Entidades | `clase` (especialidad + promoción + campus), `proyecto`, `alumno` |
| Relaciones N:M | `hace` (alumno × proyecto → `apto`, la calificación) · `asignado` (docente × clase → `rol`, `modalidad`) |

### Normalización

- **1FN:** las columnas `Proyecto_*` de los CSV pasan a ser filas de la tabla `hace`.
- **2FN:** en las tablas con clave compuesta, cada dato depende de la clave completa. Por ejemplo, `apto` depende del alumno **y** del proyecto.
- **3FN:** sin dependencias transitivas. Por ejemplo, `fecha_comienzo` se guarda una sola vez en `promocion`, y el alumno solo guarda `id_clase`.

## Cómo crear la base de datos

1. Crear un proyecto PostgreSQL en [Supabase](https://supabase.com) (también vale Render u otro servidor PostgreSQL).
2. En el **SQL Editor**, ejecutar en este orden:
   1. `sql/01_crear_tablas.sql`
   2. `sql/02_insertar_datos.sql`
3. Probar las consultas de `sql/03_consultas_demo.sql`.

Para conectarse desde aplicaciones de terceros (pgAdmin, DBeaver, Python), usar los datos de **Connect** en Supabase con `SSL mode = require`.


## Escalabilidad

Todo lo que puede crecer es un **dato**, no una columna. Un campus, una especialidad, una promoción o un docente nuevos se añaden con un `INSERT`, sin cambiar la estructura:

```sql
INSERT INTO campus (nombre) VALUES ('Bilbao');
```

## Tecnologías

PostgreSQL · Supabase · pgAdmin 4 · draw.io · Python (pandas) para el análisis de los CSV
