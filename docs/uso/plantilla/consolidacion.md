<!--markdownlint-disable MD007-->

# Consolidación de información

A continuación, se describe el proceso de consolidación de la información para ser utilizada en las plantillas de Excel.

- Si hizo cuadre contable, se usa:

    - La información **post-cuadre**.
    - Los archivos extraídos o cargados **no incluidos en el cuadre**.

- Si no hizo cuadre, se usan **todos los archivos** de extracción o carga manual.

## Transformaciones adicionales

Los datos se convierten en formato de **triángulos** antes de enviarse a la plantilla.

### Siniestros

- **Triángulos**: se generan bases de triángulos con ocurrencias en las periodicidades especificadas en el [archivo de segmentación](../../config/segmentacion.md).
- **Para entremés**: se generan bases de triángulos con ocurrencias en las periodicidades especificadas en el [archivo de segmentación](../../config/segmentacion.md) y desarrollo mensual, junto con una base mensual para el periodo en curso.

### Primas y expuestos

Se crea una única base con las cuatro periodicidades posibles a partir de los datos mensuales. Para las periodicidades mayores a mensual, los datos se agregan así:

- **Primas:** Suma.
- **Expuestos:** Promedio.
