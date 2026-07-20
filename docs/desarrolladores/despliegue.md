<!--markdownlint-disable MD007-->

# Despliegue

Esta sección describe cómo se sirve y opera la aplicación en producción.

## Servidor

- La aplicación corre sobre una **máquina Windows** (requisito de las plantillas de Excel, que dependen de una instalación local de **Excel** y **Python**).
- El servidor local se expone a internet mediante un túnel de [ngrok](https://ngrok.com/), en [https://predeterminate-bandoliered-gino.ngrok-free.dev/](https://predeterminate-bandoliered-gino.ngrok-free.dev/) (desde la v1.0.0).
- No hay balanceo de carga ni redundancia: es una única máquina de larga duración.

## Publicar una nueva version

El despliegue es manual, directamente en la máquina de producción:

1. `git pull` (o `git clone` inicial) sobre la copia local del repositorio.
2. Ejecutar `construir_static.py` para regenerar los archivos estáticos (JS, HTML, CSS) con *cache busting*, de forma que los navegadores de los usuarios carguen la versión más reciente.
3. Ejecutar el [script de arranque](entorno.md#arrancar-el-servidor) (`run.py`) para levantar el servidor.

No existe un pipeline de CI/CD (por ejemplo, GitHub Actions): las pruebas se corren manualmente antes de publicar, y la publicación de cada versión se documenta en [Versionamiento](../versionamiento.md).

## Registro (logging)

- Los logs de producción se retienen durante **180 días**, con rotación a los **500 MB**.

## Respaldo de datos

- La carpeta `data/` del servidor contiene **todo lo almacenado por la aplicación**: base de datos SQLite, archivos cargados y plantillas de cada análisis.
- Se respalda **diariamente** al SharePoint corporativo, además de poder respaldarse manualmente con el script `db_backup.py`.

### Migrar a una máquina nueva

Si la máquina de producción debe cambiarse:

1. Clonar el repositorio en la máquina nueva.
2. Recuperar el respaldo más reciente de `data/` desde SharePoint e insertarlo en la copia del repositorio.
3. Continuar con el proceso normal de [publicación](#publicar-una-nueva-version).
