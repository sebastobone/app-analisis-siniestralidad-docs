<!--markdownlint-disable MD007-->

# Descargar archivos

## Todo el análisis

En la sección **Retomar análisis** de la [página de selección de análisis](https://predeterminate-bandoliered-gino.ngrok-free.dev/seleccionar-analisis), presione el botón ⬇️. Esto descargará una carpeta con la siguiente estructura (sólo aparecen las carpetas relacionadas con la información que realmente cargó o generó):

- :material-folder: `adicionales/`: Archivos adicionales cargados.
- :material-folder: `afo/`: Archivos AFO cargados.
- :material-folder: `carga_manual/`: Archivos de siniestros, primas, y expuestos cargados manualmente.
- :material-folder: `consolidado/`: Archivos de siniestros, primas, y expuestos que se consolidan y luego [viajan a las plantillas](plantilla/consolidacion.md).
- :material-folder: `controles_informacion/`: Controles de información generados.
    - :material-folder: `pre_cuadre_contable/`: Controles antes del proceso de cuadre contable.
    - :material-folder: `post_cuadre_contable/`: Controles después del cuadre contable.
- :material-folder: `db/`: Fórmulas y valores generados dentro de las plantillas de Excel.
- :material-folder: `output/`: Resultados extraídos de las plantillas de Excel.
- :material-folder: `plantillas/`: Plantillas utilizadas en el análisis.
- :material-folder: `pre_cuadre_contable/`: Información usada para controles antes del cuadre contable.
- :material-folder: `pre_cuadre_contable/`: Información usada para controles después del cuadre contable.
- :material-folder: `queries/`: Queries cargados (siniestros, primas y expuestos).
- :material-folder: `raw/`: Información extraída de Teradata a partir de los queries.
- :material-file: `segmentacion_{negocio}/`: Archivo de segmentación cargado.

## Archivos y secciones individuales

En cada una de las secciones de la [página de análisis](https://predeterminate-bandoliered-gino.ngrok-free.dev/analisis) se generan tablas con la lista de archivos cargados o generados.

- Para descargar un archivo, presione el botón ⬇️.
- Para descargar todos los archivos, presione el botón **Descargar todos** al final de la tabla.
