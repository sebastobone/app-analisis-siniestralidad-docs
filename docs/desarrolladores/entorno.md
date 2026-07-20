<!--markdownlint-disable MD007-->
<!--markdownlint-disable MD046-->

# Configurar entorno y ejecutar pruebas

## Clonar el repositorio

El código fuente de la aplicación se encuentra en el repositorio [`app-analisis-siniestralidad`](https://github.com/sebastobone/app-analisis-siniestralidad).

```sh
git clone https://github.com/sebastobone/app-analisis-siniestralidad.git
```

### Instalar librerías de desarrollo

Dentro de la carpeta del proyecto, instale las dependencias con [`uv`](https://docs.astral.sh/uv/):

```sh
uv sync --all-groups
```

### Configurar variables de entorno

Cree un archivo :material-file: `.env.private` en la raíz del proyecto con las siguientes variables:

```env
TIPO_TOKEN="bearer"
ADMIN_PASSWORD="___"
SECRET_KEY="___"
ALGORITMO_TOKEN="HS256"
MINUTOS_EXPIRA_TOKEN_ACCESO=1440
SEMANAS_EXPIRA_TOKEN_PLANTILLA=104
TERADATA_HOST="___"
TERADATA_USER="___"
TERADATA_PASSWORD="___"
URL_API_DESARROLLO="http://127.0.0.1:8000"
URL_API_PRODUCCION="___"
PRODUCCION=False
TAMANO_MAXIMO_DESCARGA=52428800  # 50 MB
```

!!! warning "Credenciales sensibles"
    `SECRET_KEY` firma los tokens JWT de sesión: genere un valor propio y no lo comparta. `ADMIN_PASSWORD`, `TERADATA_USER` y `TERADATA_PASSWORD` son credenciales reales; nunca las suba al repositorio (`.env.private` ya está en `.gitignore`).

### Arrancar el servidor

Con `PRODUCCION=False`, el servidor arranca en modo desarrollo, con recarga automática:

```sh
uv run scripts/run.py
```

Este script ejecuta `uvicorn.run("src.app:app", reload=dev)`, donde `dev = not configuracion.produccion`. Por defecto queda disponible en <http://127.0.0.1:8000>.

## Pruebas automatizadas

La aplicación cuenta con un conjunto de **pruebas automatizadas** ubicadas en la carpeta :material-folder: `tests`.

Estas pruebas forman parte del proceso de aseguramiento de calidad y se ejecutan automáticamente antes de publicar cada nueva versión.

Su objetivo es **garantizar la estabilidad, exactitud y confiabilidad** de la aplicación, evitando que los cambios introduzcan errores en los procesos críticos que dependen de ella.

### Alcance de las pruebas

#### Pruebas generales

A partir de un **negocio ficticio** con datos simulados, se valida que:

- Las **alertas y errores de extracción de información** se generen de manera correcta y consistente.
- Las **alertas y errores de carga de información** se generen de manera correcta y consistente.
- Las **validaciones del archivo de segmentación** funcionen según lo esperado.
- Las cifras **reales y estimadas** se mantengan consistentes entre todos los archivos, procesos y rutas relevantes.
- Todas las funciones de la plantilla ejecuten exactamente los comportamientos descritos en esta documentación.

#### Pruebas por negocio

Además, se simula un **flujo completo de estimación** para cada negocio, validando que:

- El **archivo de segmentación** esté estructurado de forma correcta.
- Las **consultas SQL** estén bien parametrizadas y devuelvan los datos esperados.
- El proceso de **cuadre contable** se ejecute correctamente y respete las reglas definidas en la segmentación.
- Se generen los **controles de información** y las **evidencias de extracción** necesarias para auditoría.

### Ejecutar las pruebas

Una vez configurado el entorno, ejecute todas las pruebas con:

```sh
uv run pytest
```

#### Correr pruebas sin conexión a Teradata

Si desea ejecutar únicamente las pruebas que **no requieren conexión a base de datos** (útiles en entornos sin acceso a Teradata), use:

```sh
uv run pytest -m "not teradata"
```
